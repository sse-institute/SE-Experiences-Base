# Dealing with Maven dependencies when switching to Git
Matt Shelton
Developer Advocate

So we're moving to Git and we like git-flow. Now what? Let's test it all out! My team is great. They threw together a hit list of developer workflows in Confluence, all based on what we had been doing as a team and all of the weird things they thought we might have to do in the future. Then, in a project structure mirroring our own (but with no code in it - just a pom.xml), tried every workflow.

Maven dependencies were about to prove themselves to be our biggest problem in all of this.

Maven Build Numbering Maven produces 1.0.0-SNAPSHOT builds until you release. When you release, -SNAPSHOT is removed and your version is 1.0.0. Your build process needs to be able to support incrementing your minor version up after the fact so that subsequent work on your next effort will produce builds like 1.1.0-SNAPSHOT. You're not tied to three digits -- you can define this when you start a project, but we use three. Anyway, the -SNAPSHOT part is really important to understand. This is always going to represent the latest pre-release cut of a project.

Artifacts
See, our big concern in all of these workflows was how we were going to ensure that our project versions and inter-project dependencies were properly managed.

Each time Maven dependencies are retrieved for a build, it will, by default, pull those down from Ye Olde Internet(e)TM. Those artifacts are stored locally so that subsequent builds can be performed more quickly. One solution to make this a bit less painful is to use an artifact repository on your local network for acting as a local cache for those external dependencies. LAN retrieval is almost always going to be faster than downloading even from the quickest of CDNs. We use Artifactory Pro as our artifact repository. In addition, since we have a multi-module structure, we store our own build artifacts in Artifactory as well. When we build one of our common packages, we can pull that specific version in via maven dependency resolution and retrieve the artifact right out of the artifact repository.

This works swimmingly. Artifactory also lets you synchronize your artifacts between instances so if you wanted to, say, use it to replicate your release repository to your data centers for production deployments, you could do this without needing to build a separate process.

Maven dependencies, feature branches and pull requests
All of our builds go into Artifactory. With SVN, we had been using a snapshot repository for keeping the latest 2 snapshot builds, a staging repository for any release builds not yet approved, and a release repository only for the builds blessed to go into production.[1] These builds are numbered like I described earlier, and are retrievable by a predictable URL pattern based on repository and version.

The primary workflow for each developer was to create a feature branch from the develop branch for their work, complete it, and place a pull request to have that work merged back into the develop branch. For a single project, this works mostly without issue, but let me paint a picture for you of the first problem we ran into head-first, and one that had us seriously reconsidering the entire migration:

As I said before we have multiple layers of dependency between our projects. There's a very good reason for this - both historically and strategically - for our products. We've considered alternate architectures that would eliminate this problem, but they'd introduce others. We can make our lives easier (and we did, but that's for a later post), but for now it's strategic for us to keep our structure as it is.

So developer A, let's call her Angela, starts work on a feature in Jira. This requires two branches: one from our common project and one from product X. The version for common is 2.1.0-SNAPSHOT. The version for productX is 2.4.0-SNAPSHOT. She works locally for a while and then finally pushes back up to Bitbucket Data Center. Bamboo picks up these changes, builds the common package and uploads common-2.1.0-SNAPSHOT to Artifactory, then builds productX with a dependency on common-2.1.0-SNAPSHOT, uploading productX-2.4.0-SNAPSHOT as well. Unit tests pass!

Developer B, let's call him Bruce, starts work on another feature in Jira, for a different product: productY. This also requires two branches: one from our common project and one from productY. The version for common is, as above, 2.1.0-SNAPSHOT. The version of product Y is 2.7.0-SNAPSHOT. He works locally for a while and then finally pushes his changes up to Bitbucket Data Center. Bamboo picks up these changes, builds the common package and uploads common-2.1.0-SNAPSHOT to Artifactory, then builds productX with a dependency on common-2.1.0-SNAPSHOT, uploading productX-2.4.0-SNAPSHOT as well. Unit tests pass!

Angela, meanwhile, finds a small bug in her productX code and writes a unit test to validate her fix. She runs it locally and it passes. She pushes her changes to Bitbucket, and Bamboo picks up the change and builds productX. The build succeeds, but some of her unit tests fail. It's not the new ones she wrote, but the first ones from her initial changes to the feature. Somehow the Bamboo build has found a regression that her local build didn't? How is that possible?

Because her common dependency, the one Bamboo pulled in when it built productX, was no longer her copy. Bruce over-wrote common-2.1.0-SNAPSHOT in artifactory when his feature build completed. There was no source code conflict - both developers were working in isolation on their own branches, but the source of truth for Maven's artifact retrieval was corrupted.

Head. Meet desk.

For about a month after we discovered this problem we tried everything to get around this. Through our TAM[2], we talked to people on the Bamboo team who use git-flow, and we talked to the developer who maintains git-flow, a java implementation of git-flow. They were all super helpful, but short of a process that required a list of manual steps for each developer every time they worked on a feature, we couldn't find a resolution that was tolerable.

If you're curious what we considered, here's everything we tried:

1. Modify the version number on branch creation, or immediately thereafter.

We can do this with mvn jgitflow:feature-start to create the branch.
We can use a Bitbucket Data Center hook or a local githook.
We can manually set with mvn version:set-version after we create the branch.
We can automate the change with the [maven-external-version] plugin.
2. Modify the version number when finishing the branch and merging back to develop.

We can do this with mvn jgitflow:feature-finish to finish the branch.
Use a git merge driver to handle pom conflicts.
Use an asynchronous post-receive hook in Bitbucket Data Center
3. Do it all manually. (Just kidding. We didn’t consider this option very long.)

Possible workflows
By remembering this core concept and reflecting on it, you can understand that submodule support some workflows well and less optimally others. There are at least three scenarios where submodules are a fair choice:

When a component or subproject is changing too fast or upcoming changes will break the API, you can lock the code to a specific commit for your own safety.

When you have a component that isn't updated very often and you want to track it as a vendor dependency. I do this for my vim plugins for example.

When you are delegating a piece of the project to a third party and you want to integrate their work at a specific time or release. Again this works when updates are not too frequent.

Credit to finch for the well-explained scenarios.

Each one of these options had some sort of negative side-effect. Chiefly, manual steps for a developer each and every time they needed a feature branch. And we wanted them to create feature branches all the time. Also in most cases we could not effectively use pull requests, which was a deal-breaker.

This consumed 1-2 people for almost two months until we had a (mindblown) revelation as to why we were approaching this problem from the wrong direction.

One version to rule them all
Hindsight being 20/20, I can clearly see that our biggest mistake was that we were focusing our attention on the git-flow tools rather than using the tools we already had to implement the workflow we wanted. We had:

Jira
Bamboo Data Center
Maven
Artifactory Pro
Turns out, those were all of the tools we needed.

One of our engineers got the very bright idea that since the problem wasn't the build management itself but rather the artifacts being over-written, that we should fix Artifactory instead. His idea was to use a Maven property to set the snapshot repository URL to a custom URL which included the Jira issue ID, and then write out its artifacts to a dynamically-created repository in Artifactory with a custom template. Maven’s dependency resolver will find artifacts in the develop snapshot repository if we haven’t needed to branch them, for instance if we’re only working on a product and not also common.

We set that handy little property variable in our build settings file, and wrote a Maven plugin to populate it during the earliest part of maven’s build lifecycle. On paper, this sounded incredible and re-invigorated the team to work harder to solve this problem. Trouble was that we couldn't actually do this. The earliest stage of the maven lifecycle is 'validate'. By the time plugins bound to validate have been run, the repository URLs were already resolved. Because of this, our variable never populated and the URL is not branch-named after all. Even though we had been using a layout in a separate repository from our develop snapshots, it wouldn’t be isolated for parallel development.

Head, meet desk again, your BEST FRIEND.

After a beer, the aforementioned engineer did some more digging and research into another way to add functionality to maven: extensions.

“Here’s to beer: the cause of, and solution to, all of life’s problems.” - Homer Simpson

Extensions, like plugins, give you a whole host of power to enhance your Maven workflow, however they are executed before lifecycle goals, and have greater access to the Maven internals. By utilizing the RepositoryUtils package, we forced Maven to re-evaluate its URLs using a custom parser and then re-set them using our updated values.[3]

Extension in place and tested, we started knocking off pre-migration tasks one after another, going from "this is never going to happen" to "this IS going to happen Monday... so now I need to write ten pages of documentation by tomorrow". I'll write more soon about how the tools work together to achieve our new development workflow, and some of the lessons we learned about the process.

Footnotes
[1]: One downside here was that I had to use a script I wrote to hit the Artifactory REST API to "promote" builds from staging to release. It's fast enough, but begging for more automation.

[2]: Technical Account Manager. More information here.

[3]: After the initial development efforts, we found that we had to do even more to make this work 100% of the time, like when a snapshot is newer in Artifactory (from another engineer) than your local snapshot, Maven grabs the remote artifact because hey, it's NEWER, so it must be BETTER, right?

Matt Shelton
Matt Shelton is a DevOps advocate and practitioner who oversees the delivery of DevOps (and other related) service offerings for Atlassian in the Americas. He rarely blogs anymore and focuses on building the next generation of advocates for better ways of working.


source:
https://www.atlassian.com/git/articles/maven-dependencies-versions-merging
