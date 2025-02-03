# Working with Git: 8 difficulties and solutions for them
Deviniti Technology-Driven Blog
Magda Korgul


Published in
Deviniti Technology-Driven Blog


Git’s popularity in open source projects is growing and now surpasses SVN and TFS. However, implementing this system efficiently isn’t simple and requires some preparation. In practice, adopting Git may translate into difficult compromises between the developer’s preferences and the company’s needs. What exactly is Git, what are the challenges it presents to enterprises, and how to seamlessly implement this system?

From the team’s perspective, working with a distributed version control system (DVCS) is more complicated than working with a central system. It’s the level of complexity that is at the root of most of the current barriers to the wider use of such systems. — Gartner

Quick reminder — what is Git?
Before we get down to the best practices, let’s organize our knowledge of Git. What is this software exactly? Git is a distributed version control system, so it’s used for controlled project changes. Thanks to the continuous recording of changes taking place in the project, the system allows you to quickly identify problems and ensure a better quality of new versions. Git allows both for individual work and the developing software by several specialists at the same time. It also lets you view history that helps to quickly identify where an error occurred in the code, also when working in a team. The solution offers a simple way to return to previously abandoned ideas.

Unlike central versioning systems, Git doesn’t rely on one main code repository. Distributed control ensures that there are as many Git repositories as there are team members, and each developer has access to the full sources of the project and history of changes locally. Such freedom can both encourage creativity and make it difficult to structure work. The freedom to create branches requires a certain systematization of project management.

Let’s get straight to the 8 difficulties of working with Git and solutions for them.

Workflow
An obvious reason why enterprises use Git is that developers are so fond of its distributed workflow, even if the fully distributed model is rarely used. Programmers often have to switch threads at work, and the light, local branching supported by Git helps a lot. It’s the way of handling branches that serves as an important distinguishing feature of Git. But how do you manage the complexity that Git introduces when you view a topic from a perspective broader than a single computer? How should you share experimental branches with other team members? When should you remerge the contents of these branches back with the main branch?

One of the solutions is Git-flow. The Git-flow method addresses the need to automate work with Git. Branching is an integral part of writing code in this system. The scheme of operation is the same, regardless of whether we develop new functionalities or fix bugs:

the beginning is the main branch
creating a new branch
making changes
returning to the main branch
merging modifications
removal of the unnecessary branch.
These repetitive activities require the automation that Git-flow provides. It introduces a specific structure of branches in the repository, each of which has an assigned task (Master is the production branch, Hotfix is ​​used for patches, Release is the release branch, Develop is the development branch, and Feature is the functional branch).


Default Git-flow structure
Tools such as GitSwarm, GitHub, GitLab, or Bitbucket implement simple pull request processes that can provide certain responses. The best time to resolve these issues is before your teams deploy Git. Attempting to change the process after starting the work and updating the audit history is more complicated and requires more effort and time.

Workflow — best practices
Define your branching strategy with clear steps and transition conditions. It’s best to automate your code flow as much as possible.
Define revision as an integral part of your strategy. Interdisciplinary collaboration is crucial in modern product development.
Equip all scenarios with tools tailored to their specific needs to maximize productivity and ease of use.
Resources
A popular rule when working with Agile methodologies is that you should keep all the intellectual property in the same place to form a single source of truth. However, it turns out that the interdisciplinary nature of teams and project requirements exponentially increases the number and size of resources in a typical project. Source code is often just a drop in the digital sea compared to documentation, graphics, models, audio, video, and even an entire virtual machine (VM) environments for testing and deployment.

This type of expansion poses a serious challenge when adopting Git in the enterprise because its internal mechanisms limit the maximum practical size of the repository to 1 or 2 GB (although such limitations are solved by Bitbucket via FLS, for example). Even repositories far below this limit could experience performance problems if the type and size of the resources and the commands executed cause this. Performing even a simple blame command on a repository with large binary resources can visualize this problem painfully.

Moreover, these kinds of resources are created by designers or artists who may lack technical skills and therefore won’t use the Git command line, even if it could help them with their work. Our Git management strategy should be about how our non-technical coworkers store their work, how to manage these files along with the Git source code, and how to put together the correct versions of all files in our production and release system. For example, this is useful in the Game Dev industry, where it’s necessary to store large multimedia files.

What could be the solution?
The most common ways to manage large digital assets are moving large files outside of a Git repository or splitting resources into several repositories that are magically linked by DevOps for production, testing, release, and other tasks. Tools like git-annex and Git LFS can help to move digital assets outside of the repository. Influencing the dependent modules of Git along with a set of proprietary scripts allows taming the phenomenon of resource stretching between repositories (the so-called Git sprawl). Here, however, you should watch out for possible further problems related to backing up or distributing content between company branches. Whenever possible, an enterprise should look for a system that can reap the benefits of distributed version control (DVCS) but keeps all resources in one place.

Resources — best practices
Keep all your assets in one place, ideally in one version control tool. If you won’t do it and choose to use Git, think about adopting a “monorepo” (a single, large repository — with all the previously described consequences of using such a repository with Git) instead of relying on tools that share your intellectual property.
Use a Git management suite that can handle all files, including digital assets — not just the source code.
Determine an archiving strategy for legacy resources that matches your branching strategy to keep your repositories organized and easy to use.
Continuous Delivery
The workflow and type of resources your employees create may vary, but the end product always needs to be tested and integrated with maximum use of automation. Automation in conjunction with the Git architecture can put a heavy burden on the system, so planning ahead is imperative to get meaningful performance from Continuous Delivery systems.

A common practice in CDs is to build and test on an up-to-date copy of all files. However, when using Git, it’s easy to overload servers this way for two reasons:

like most distributed systems, Git downloads all the versions of files by default during a clone operation (in this case, you can use the Git Shallow clone operation, which will only download the latest commit, not the entire repository history),
Git doesn’t support narrow cloning (i.e., using only the files you need, instead of all of them).
In conclusion, when cloning a Git repository, we get all the versions of all files by default.

The problems I described above are usually irrelevant in small, medium, and open source projects. But they might become insurmountable obstacles in large-scale enterprise projects. The sheer number of joining operations with a large number of production branches can turn out to be the “banana peel” on the project’s pathway. Git encourages developers to branch out all their tasks or features. The sheer size of the repositories can, however, make life difficult for developers because of long cloning times or sync times of the local repo with the remote repo.

Continuous Delivery — best practices
Use shallow cloning to get only the latest versions of files and try to use the Git management system that supports narrow cloning. Thanks to this, you will significantly reduce the load on the server.
Choose the right build cycle for your projects. Build once an hour or a day instead of every time when uploading new files.
Automate code joining and increase its frequency to the maximum. That way, you’ll be able to spot integration issues before they build up and clog your code flow.
Reliability
Even the best tool is useless if it doesn’t work. This statement holds true, especially in a business environment that needs to work around the clock all over the world. Git was built with simple file systems in mind, and most Git management systems are built directly on top of the base version of Git. Meeting your company’s high availability and disaster recovery requirements can be challenging.

Most Git management systems offer services like backups and snapshots, which is a good first line of defense. Some systems increase the level of availability with standby virtual machines, using various means to reflect changes between file systems or to swap servers if necessary. However, this doesn’t change the fact that if we need dial-a-load scalability combined with high availability, our choice is significantly limited.

We should also remember that while cloning a repository for safety may seem easy, in the case of Git, we will clone the entire repository. This approach is not a problem for small to medium-sized projects. However, large projects eat up network bandwidth, disks, and time. If our resources are measured by more than megabytes, we need to take a detailed look at the level of reliability that our Git management system offers.

Reliability — best practices
Backups are good, but a disaster recovery plan is better. Create a plan that balances acceptable losses against costs, and check it regularly.
When choosing a Git management system, look for one that provides both clustering and a high level of availability so that everyone can work even when hardware fails.
Security
Git comes with a limited level of security, offering solid authentication but completely ignoring authorization. This approach works well if you just want to make sure that everyone uploading changes are who they say they are because their identity is verified through digital signatures. But what if you want to restrict access to a specific repository, branch, or file containing trade secrets? Unfortunately, Git doesn’t offer anything to address these needs, which is the main reason why so many management systems exist. However, they aren’t on an equal level when it comes to ensuring security.

Git management systems
Git management systems, as a rule, offer easy creation of users and groups, as well as limiting access to the project (read: repo) using these ways. Moreover, they usually provide small sets of roles to which we can assign different permissions. Some (such as GitLab) extend permissions to branches as well, allowing you to restrict access to specific users. The best solutions offer various levels of monitoring and tracking of usage patterns.

Also, think about your needs for auditing and logging operations. In an environment with increased levels of security or regulation, it may become necessary to keep a permanent record of who made changes, when, and for what purpose. Standard Git allows for a lot of flexibility in overwriting history or even completely hiding changes. If being fully audited is important to you, look for a version control tool that offers reliable, secure record.


Granting access in GitLab
Security in everyday work
When using only Git, be sure to follow the highest principles of security. Assign permissions to each repository and only grant access to those developers who need it. Use a secure key vault such as Vault to provide access to the Git repository when the system requires it beyond the control of the developers. Also, remember to carefully check the configuration of access settings — otherwise, you may risk data leakage.

Security — best practices
Make sure that your branch structure not only matches the workflow but also shares resources according to security needs.
Choose a Git management system that provides a maximum granularity of access control, as well as timely proactive monitoring of user actions to flag and report questionable behavior before you’re exposed to intellectual property loss.
If branches aren’t enough, organize your resources in advance to keep your security-critical files together, making it easier to define access restrictions later.
Repository management
Whether you need to have a repository management system or not depends a lot on the limitations of our Git management system. If we choose a system that can’t handle digital assets, a large number of files, or a large total repository size, Git sprawl will emerge. It’s also easy to cause Git sprawl if the process is based on builds from components. A large number of independently created and versioned components can cause problems with Git dependent modules if our management system doesn’t offer a different mechanism. Git itself rather makes it easier to map external repositories among themselves, but you need to deal with the selection of dedicated tools, methodologies and making some compromises beforehand.

Regardless of the cause, the more we divide our resources, the more problems we encounter when combining them for production, testing, and other tasks. The explosion in the number of Git repositories will be a headache for full-time DevOps, so they need to be carefully planned and maintained afterward.

Managing repositories — best practices
Before choosing a solution, take into account how much time the DevOps team can spend on managing the repositories.
If possible, investigate the possibility of using tools and techniques to “manage” high-level components.
Use a Git management system that avoids Git sprawl as much as possible, or that at least takes care of the problem behind the scenes.
Visibility
This is an issue that Git itself completely ignores, and management systems don’t help much: the need for full, transparent visibility of all stages of the production line. Most Git management systems provide dashboards that list all projects, dashboards for individual projects, and even a handful of useful statistics. Many also provide a simple problem tracking tool or ability to integrate with external Application Lifecycle Management (ALM) tools.

However, if you choose a vendor that doesn’t provide a complete solution, you will likely need to develop plugins, scripts, and other integrations on your own. The Git sprawl mentioned before complicates the matter even more — for obvious reasons, it’s easier to analyze data from a monorepo than from hundreds or thousands of repositories.

Visibility — best practices
Accurately define your information priorities and key metrics. Follow them when choosing a Git management system.
Think to what extent the solution that you choose will trigger Git sprawl. If necessary, consider selecting tools and techniques for data aggregation.
Choose a vendor that offers more than just Git management, especially if you need dedicated metrics or extensive support for integration with other tools.

Operations Dashboard in GitLab
Hidden costs
The neccesity of learning
An often-overlooked cost of adapting and using Git is the need to learn how it works. The widespread implementation of Git across companies can give an illusion that the system only needs to be downloaded, and you can start working. However, developers generally find learning Git difficult. Using the system is even more difficult for people with no technical background. Fortunately, there are several ways to make learning Git easier. Great GUIs for Git are the already mentioned systems such as Bitbucket or GitLab, which allow non-technical people to work with it easily. On the other hand, graphic overlays like Tortoise are very useful for merging.


Environments Dashboard in GitLab
Security
Git hosting services often allow repositories to be “secured” with different permissions and even protect individual branches by only allowing specific groups to push. However, few offer single file control and granular permissions we know from centralized systems. Anyone with access to the repository has access to everything inside as well. This means that restrictions on access to intellectual property are associated with splitting the repository, thus amplifying the Git sprawl phenomenon. The need for security is, therefore, another hidden cost of Git, especially when we use outsourcing, which is so popular nowadays. Simultaneously restricting access to intellectual property content and allowing an external team to work can significantly impact the project’s performance.

Scaling costs
Unfortunately, Git wasn’t designed with any scalability in mind. The original Git doesn’t make the most of the hardware, so expanding it with more processors or memory chips won’t help you get a vertical scale effect. Most web hosting sites try to deal with this by overlaying a web front-end over the original Git, leaving the solution to the very nature of web applications that can process more queries asynchronously. This approach is as good as the file system we’re using since Git relies on it entirely. It’s worth knowing, however, that Git’s unique storage system leaves much to be desired as repositories grow in size. This problem was partially solved by creating a series of overlays and workflow changes to leave large files outside the version control system, bundling them into working folders. Tools such as git-annex and Git LFS can simplify this process.

Hidden costs — best practices
Remember to properly assess the skills of team members with less technical knowledge and equip them with the appropriate tools or possibly easy-to-use plugins for applications they use on a daily basis.
Be careful when accessing the repository content, especially to external contributors.
Conclusion
We have considered eight key aspects where Git can become a problem in the enterprise. When using this system, it’s important to be aware of its imperfections and know how to deal with them. Another option is to use an alternative to Git that is tailored to your business needs but still offers the features that developers like. However, if you decide to implement Git, the best practices in this guide should help you plan and select the optimal Git management system, minimizing the hassle.

Are you interested in version control systems? Learn more about Git repository tools like Bitbucket and GitLab or Perforce Helix Core.

Check out other articles available on Deviniti Technology-Driven Blog:

Banking Systems Security: A Complete Guide
9 custom software integrations — from an idea to the outcome
Bitbucket vs. GitLab — which one is better for repo management?
This article was originally published in Polish on the Deviniti blog.

Git
Gitflow
DevOps
Version Control
Continuous Integration
