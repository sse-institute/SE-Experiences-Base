# Submodules: Core concept, workflows and tips
NICOLA PAOLUCCI
Developer Advocate

Including submodules as part of your Git development allows you to include other projects in your codebase, keeping their history separate but synchronized with yours. It's a convenient way to solve the vendor library and dependency problems. As usual with everything git, the approach is opinionated and encourages a bit of study before it can be used proficiently. There is already good and detailed information about submodules out and about so I won't rehash things. What I'll do here is share some interesting things that will help you make the most of this feature.

Table of contents
1. Core concept

2. Possible workflows

3. Useful tips incoming

4. How to swap a git submodule with your own fork

5. How do I remove a submodule?

6. How do I integrate a submodule back into my project?

7. How to ignore changes in submodules

8. Danger Zone! Pitfalls interacting with remotes

9. Conclusions


Core concept
First, let me provide a brief explanation on a core concept about submodules that will make them easier to work with.

Submodules are tracked by the exact commit specified in the parent project, not a branch, a ref, or any other symbolic reference.

They are never automatically updated when the repository specified by the submodule is updated, only when the parent project itself is updated. As very clearly expressed in the Pro Git chapter mentioned earlier:

When you make changes and commit in that [submodule] subdirectory, the superproject notices that the HEAD there has changed and records the exact commit you’re currently working off of; that way, when others clone this project, they can re-create the environment exactly.

Or in other words :

[...] git submodules [...] are static. Very static. You are tracking specific commits with git submodules - not branches, not references, a single commit. If you add commits to a submodule, the parent project won't know. If you have a bunch of forks of a module, git submodules don't care. You have one remote repository, and you point to a single commit. Until you update the parent project, nothing changes.

Possible workflows
By remembering this core concept and reflecting on it, you can understand that submodule support some workflows well and less optimally others. There are at least three scenarios where submodules are a fair choice:

When a component or subproject is changing too fast or upcoming changes will break the API, you can lock the code to a specific commit for your own safety.

When you have a component that isn't updated very often and you want to track it as a vendor dependency. I do this for my vim plugins for example.

When you are delegating a piece of the project to a third party and you want to integrate their work at a specific time or release. Again this works when updates are not too frequent.

Credit to finch for the well-explained scenarios.

Useful tips incoming
The submodule infrastructure is powerful and allows for useful separation and integration of codebases. There are however simple operations that do not have a streamlined procedure or strong command line user interface support.

If you use git submodules in your project you either have run into these or you will. When that happens you will have to look the solution up. Again and again. Let me save you research time: Instapaper, Evernote or old school bookmark this page (:D:D) and you will be set for a while.

So, here is what I have for you:

How to swap a git submodule with your own fork
This is a very common workflow: you start using someone else's project as submodule but then after a while you find the need to customize it and tweak it yourself, so you want to fork the project and replace the submodule with your own fork. How is that done?

The submodules are stored in .gitmodules:

$ cat .gitmodules [submodule "ext/google-maps"] path = ext/google-maps url = git://git.naquadah.org/google-maps.git
You can just edit the url with a text editor and then run the following:

$ git submodule sync
This updates .git/config which contains a copy of this submodule list (you could also just edit the relevant [submodule] section of .git/config manually).

Stack Overflow reference

How do I remove a submodule?
It is a fairly common need but has a slightly convoluted procedure. To remove a submodule you need to:

1. Delete the relevant line from the .gitmodules file.

2. Delete the relevant section from .git/config.

3. Run git rm --cached path_to_submodule (no trailing slash).

4. Commit and delete the now untracked submodule files.

Stack Overflow reference

How do I integrate a submodule back into my project?
Or, in other words, how do I un-submodule a git submodule? If all you want is to put your submodule code into the main repository, you just need to remove the submodule and re-add the files into the main repo:

1. Delete the reference to the submodule from the index, but keep the files:

git rm --cached submodule_path (no trailing slash)
2. Delete the .gitmodules file or if you have more than one submodules edit this file removing the submodule from the list:

git rm .gitmodules
3. Remove the .git metadata folder (make sure you have backup of this):

rm -rf submodule_path/.git
4. Add the submodule to the main repository index:

git add submodule_path git commit -m "remove submodule"
NOTE: The procedure outlined above is destructive for the history of the submodule, in cases where you want to retain a congruent history of your submodules you have to work through a fancy "merge". For more details I defer you to this very complete Stack Overflow reference.

How to ignore changes in submodules
Sometimes your submodules might become dirty by themselves. For example if you use git submodules to track your vim plugins, they might generate or modify local files like helptags. Unfortunately, git status will start to annoy you about those changes, even though you are not interested in them at all, and you have no intention of committing them.

The solution is very simple. Open the file .gitmodules at the root of your repository and for each submodule you want to ignore add ignore = dirty, like in this example:

[submodule ".vim/bundle/msanders-snipmate"] path = .vim/bundle/msanders-snipmate url = git://github.com/msanders/snipmate.vim.git ignore = dirty
Thanks to Nils for the great explanation.

Danger Zone! Pitfalls interacting with remotes
As the Git Submodule Tutorial on kernel.org reminds us there are a few important things to note when interacting with your remote repositories.

The first is to always publish the submodule change before publishing the change to the superproject that references it. This is critical as it may hamper others from cloning the repository.

The second is to always remember to commit all your changes before running git submodule update as if there are changes they will be overwritten!

Conclusion
Armed with these notes you should be able to tackle many common recurring workflows that come up when using submodules.

Nicola Paolucci
Nicola is an all-round hacker who loves exploring and teaching bleeding edge technologies. He writes and talks about Git, development workflows, code collaboration and more recently about Docker. Prior to his current role as Developer Instigator at Atlassian he led software teams, built crowd sourcing applications for geo-spacial data, worked on huge e-commerce deployments. Little known facts about Nicola: he gesticulates a lot while speaking (being Italian), lives in Amsterdam and rides a Ducati.


source:
https://www.atlassian.com/git/articles/core-concept-workflows-and-tips
