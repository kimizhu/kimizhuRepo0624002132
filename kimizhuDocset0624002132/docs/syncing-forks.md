# Syncing your fork with the source repo

> [!NOTE]
> See also [other approaches](#other-approaches) below.

Often, when working on Git repositories such as those used for docs.microsoft.com, you'll find yourself needing to work on a repo for which you have only read access. For such repos, you can't simply clone the repo locally and push your changes directly back to the repo. Instead, you need to create a fork of the repo, clone it locally, work in your fork's local clone, push your changes back up to your fork's origin repo on the GitHub server, and then create a pull request back to the original repo to ask that your changes be incorporated into the project.

There's one problem with this working process: there's no way to keep your fork's origin repo in sync with the source repo directly. As a result, for repos that are actively maintained, you may find that when you create your pull request, your origin repo is many commits behind the source repo, raising the possibility of merge conflicts that can prevent your PR from being accepted or merged.

There is a way to sync your fork's origin repo with its upstream source indirectly, however. You'll need to use the Git shell to pull the latest commits from the upstream source repo, merge them with your local clone, and then push them back up to your fork's origin on the GitHub server. 

Follow these steps:

1.	Within GitHub Desktop, select your local clone of the fork repo. 
2.	Select your local master branch.
3.	Right-click the repo’s name and select **Open in Git Shell.**
4.	When the Powershell window opens, make sure your master branch is checked out. Your prompt should look like this:  
`~\Documents\GitHub\azure-docs-pr [master =]>` 
5.	Type: **`git remote -v`**  
This will confirm you have the original repo as an upstream repository. The response should look like:
<pre>Microsoft       https://github.com/Microsoft/azure-docs-pr.git (fetch)  
Microsoft       https://github.com/Microsoft/azure-docs-pr.git (push)  
origin  https://github.com/MattLusherRO/azure-docs-pr.git (fetch)  
origin  https://github.com/MattLusherRO/azure-docs-pr.git (push)</pre>
"Microsoft" is the original, upstream source repo; "origin" is your fork’s repo on the GitHub server.
6.	To get the latest changes from Microsoft, type: **`git merge Microsoft/master`** 
This will fetch all the latest commits and merge them into your master branch. 
7.	The prompt will change to show how many commits are in your local clone, but not in your server repo:  
`~\Documents\GitHub\azure-docs-pr [master 80]>`
8.	Type: **`git push origin master`** (note the lack of a slash)  
This will sync all those commits from the upstream source repo back up to your origin. 

Once this is done, you should be in sync with the upstream repo, your fork's origin, and your local clone. If you work quickly, you might even be able to push your commit and then make your PR before the next change in the upstream repo. ;-)

## Other approaches
* **Rebase**: [Tutorial on rebasing](https://benmarshall.me/git-rebase/)
* **Squash**: [Squash commits into one](https://www.devroom.io/2011/07/05/git-squash-your-latests-commits-into-one/)

## Resources
* [Tutorial on rebasing vs. merging](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)