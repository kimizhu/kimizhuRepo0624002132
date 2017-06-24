# Managing work in forked Git repositories

Working on [docs.microsoft.com](https://docs.microsoft.com) involves multiple repositories, most of which we don't have write access to. This means we have to fork the repository, make changes in our fork, and create a pull request to have those changes incorporated back into the original repo. For some repos, such as azure-docs-pr, you may find yourself working on multiple bugs in parallel.

Working in this way can lead to complications. For each bug, you want to have your pull request contain changes just for that bug, and no others; otherwise, you're exposing your stakeholders to confusion, as they're asked to review PRs containing changes they didn't request. Even worse, if they approve the changes in your PR, they'll incorporate not only the changes they approved, but all other changes that were captured in the PR, including any for other bugs that got inadvertently included. In this way, unapproved and actually incorrect changes can be accidentally published.

To make sure you can keep your changes for each bug you're working on separate, and ensure that your PRs contain only the changes needed for the bug in question, follow these best practices.

## Keep your master branch clean
When you fork a repo like azure-docs-pr, treat your own master branch as a one-way conduit for pulling changes down from the source. Don’t do any work in it, and don’t sync changes from one of your other branches back up to it. To keep it current, fetch and rebase from the source repo’s master branch.

In the Git shell:
1. ```git checkout master```
2. ```git fetch Microsoft``` (This is the Git label for the source repo; in the rare event this doesn't work, use ```git remote``` to find the correct name)
3. ```git rebase Microsoft/master```
4. ```git pull``` (This pulls everything from origin/master into your master branch)
5. ```git push``` (This pushes your changes back into master; your origin is now up to date with the source repo as well)

Your fork is now caught up to Microsoft/master.

## Create a new branch for each bug
Whenever you need to work in your fork, first fetch/rebase your master from the source repo using the steps above. Then, create your new bug branch *from your master branch*. This new ensures that your bug branch is current with the Microsoft/master branch. 

## Keep your bug branch pure: one branch, _one bug_
It often happens that, while you're working on one bug, another one is assigned to you; or, while you're working on a bug, you notice one or more problems outside the scope of the bug that also need to be addressed. Always remember that your work will be put into a pull request, to be approved by someone on another team who is expecting to see _just your changes for your current bug!_ Never succumb to the temptation to mix in work on these other problems that may appear to you. Doing so will introduce issues: 
* The person tasked with approving your pull request (see "Submitting your work for review" below) will find themselves reviewing changes they hadn't asked for, and which they may not be qualified to approve.
* The changes you've added on your own may seem obvious to you, but you never know when you've actually done something wrong. Thus, you risk propagating errors into the docs.
* You may have no way to track down the right person who _should_ review your out-of-scope changes.

Instead, take note of the changes you believe need to be done and create a new bug in Technical Content, assigning it Apex Production and @mentioning the owner of the repo in which the problem exists; see [Publishing to different repos](repo-publishing.md), "Repo contacts". The owner should be able to authorize the changes. If the new bug is then reassigned to you, you can then create a new bug branch following the procedure above.

## Submitting your work for review
Do your work in the bug-named branch you created. When you’re ready to share with the stakeholder for review, do this: 
1.	Fetch and rebase, either directly from the source repo’s master branch, or first checkout your own master and fetch/rebase it, then fetch/rebase your bug branch from your own master. 
    a. To rebase directly from the source repo, you can follow the Git shell steps above, checking out your bug branch in step 1 and following the other steps as is.
    b. To rebase from your own master, first follow the Git shell steps above in your master branch; then checkout your bug branch and follow the same steps, except that instead of fetching Microsoft, you fetch origin; then rebase origin/master.
2.	Publish your bug branch to GitHub. You can do this directly from Visual Studio Code, but here's the Git command: ```git push -u origin *bugbranchID*```
3.	Do NOT merge your bug branch's changes up to your own master. Instead, create a PR from your bug branch directly back to the source repo (generally the master branch unless instructed otherwise). When you do this, you should be able to PR just the current changes from your bug, and won’t have to carry anything extra.
4.  For subsequent rounds of changes, repeat steps 1-3, always making sure _never_ to merge your bug branch's changes into your own master branch. 
5.	Once your PR is approved, you can delete your bug branch. (```git branch -d *NNNNNN*```)
> ## NOTE
> You don’t have to merge it to your master branch even now; once your PR is approved, your changes are in the source repo’s master, and you’ll get them the next time you fetch/rebase your master from the source. You can simply delete your additional branch. Note that the GitHub pull request’s Conversation tab will give you the option of deleting your branch; feel free to use it.

