# Git and project dependencies
NICOLA PAOLUCCI
Developer Advocate

Consider the following questions:

How do you handle project dependencies with git?

Our project is made up of multiple inter-dependent repositories. Currently we manage those with svn:externals. What's the best way to handle those with git?

How do you split a very big repository in smaller components using git?

These are some of our most frequently asked questions.

The topic appears to be a big pain point for many software teams adopting git, so in this article I'll try to shed some light on the issue.

Obviously project dependencies and build infrastructure are two intertwined areas, and even internally at Atlassian a discussion sparked on the "Future of Builds".

Having separate repositories as opposed to having a single one can make some things harder. But it's a relatively natural – sometimes mandatory – step in the evolution of a software project for at least two major reasons: increasing build times, and shared dependencies between projects.

Painting with broad strokes: guidelines and sub-optimal solutions
So back to the question: How do you track and manage project dependencies with git?

If possible, you don't!

Jokes aside, let me answer with broad strokes first and dig deeper afterwards. Please realize that there is no silver bullet – in git or otherwise – that will painlessly solve all the issues related with project dependencies.

Once a project grows over a certain size, it makes sense to split it in logical components, but don't wait to have a 100 million plus lines of code in a single repository before you do. So the following are just guidelines so that you can devise your own approach.

First choice: Use an appropriate build/dependency tool instead of git
A dependency management tool is my current recommended way forward to handle the growing pains and the build times of sizeable projects.

Keep your modules separated in individual repositories and manage their interdependency using a tool built for the job. There is one for (almost) every technology stack out there. Some examples:

Maven (or Gradle) if you use Java
Npm for node apps
Bower, Component.io, etc if you use Javascript (Updated!)
Pip and requirements.txt if you use Python
RubyGems,Bundler if you use Ruby
NuGet for .NET
Ivy (or some custom CMake action) for C++ (Updated!)
CocoaPods for Cocoa iOS apps
Composer or Phing for PHP (Added!)
In Go the build/dependency infrastructure is somewhat built into the language (though people have been working on a more complete solution, see godep) For our Git server (Bitbucket) we use both Maven and Bower. At build time the tool of choice will pull the right versions of the dependencies so that your main project can be built. Some of these tools have limitations and make assumptions that are not optimal, but are proven and viable.
The pains of splitting your project
Simplistically at the start of a project everything is packed in one build. But as the project grows, that may result in your build to be too slow – at which point you need "caching," which is where dependency management comes in. This by the way means submodules (see below) lend themselves very well to dynamic languages for example. Basically I think most people need to worry about build times at some point, which is why you should use a dependency management tool.

Splitting components into separate repositories comes with some serious pain. In no particular order:

Making a change to a component requires a release
Takes time and can fail for lots of stupid reasons
Feels stupid for small changes
It requires manually setting up new builds for each component
Hinders the discoverability of repositories
Refactoring when not all the source is available in a single repository
In some setups (like ours) updating APIs requires a milestone release of the product, and then the plugin, and then the product again We've probably missed a few things, but you get the idea. A perfect solution to the problem is far from here.
Second choice: Use git submodule
If you can't or don't want to use a dependency tool, git has a facility to handle submodules. Submodules can be convenient, especially for dynamic languages. They won't necessarily save you from slow build times, though. I already have written some guidelines and tips about them and also explored alternatives. The Internet at large also has arguments against them.

1:1 match between svn:external and git
BUT! If you're looking for a 1-to-1 match between svn:externals and git, you want to use submodules making sure the submodules track only release branches and not random commits.

Third choice: Use other build and cross-stack dependency tools
Not always you will enjoy a project that is completely uniform and can be built and assembled with a single tool. For example some mobile projects will need to juggle both Java and C++ dependencies, or use proprietary tools to generate assets. For those more complex situations you can enhance git with an extra layer on top. A great example in this arena is Android's repo.

Other build tools that are worth exploring:

Repobuild
Chromium's depo_tools system
Distributed build
Facebook's Buck
Twitter's Pants
Conclusion and further reading
Suggestions for further reading on the build infrastructure (and Maven) topic from Charles O'Farrell:

In quest of the ultimate build tool
And his follow up Maven is broken by design
Recursive Maven considered harmful
I'd like to conclude with this excellent quote from latter article above. Even though it is about Maven, it could equally be applied to other build and dependency tools:

"A cache does nothing except speed things up. You could remove a cache entirely and the surrounding system would work the same, just more slowly. A cache has no side effects, either. No matter what you've done with a cache in the past, a given query to the cache will give back the same value to the same query in the future.

 

The Maven experience is very different from what I describe! Maven repositories are used like caches, but without having the properties of caches. When you ask for something from a Maven repository, it very much matters what you have done in the past. It returns the most recent thing you put into it. It can even fail if you ask for something before you put it in."

Nicola Paolucci
Nicola is an all-round hacker who loves exploring and teaching bleeding edge technologies. He writes and talks about Git, development workflows, code collaboration and more recently about Docker. Prior to his current role as Developer Instigator at Atlassian he led software teams, built crowd sourcing applications for geo-spacial data, worked on huge e-commerce deployments. Little known facts about Nicola: he gesticulates a lot while speaking (being Italian), lives in Amsterdam and rides a Ducati.