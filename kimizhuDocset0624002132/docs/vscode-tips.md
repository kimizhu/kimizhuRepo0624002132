# Working with Visual Studio Code
Visual Studio Code is a flexible, lightweight code editor that works well with the files we maintain on Docs.microsoft.com, as it incorporates Git workflow directly; you can navigate branches, sync with the remote server, and more. Following are some tips for making best use of Visual Studio Code.

## Customizing your installation
Visual Studio Code makes it easy to customize its setup and operation to your liking. Visual Studio Code settings are stored in a .json file, C:\Users\&lt;user&gt;\AppData\Roaming\Code\User\settings.json, which you can edit in any text editor; but more conveniently, in VS Code itself. Select File &gt; Preferences &gt; Settings and the .json file will open, ready to edit. Scrolling through the settings file will reveal all the many ways in which you can customize the way VS Code works for you. 

### Selecting the Bash shell (or any other) as the integrated terminal
VS Code lets you open a terminal window, integrated with the VS Code user interface, for running command-line commands and scripts; it's accessible with Ctrl+`. By default, the shell used for this integrated terminal is Windows PowerShell. Since we use VS Code with GitHub, you may find it convenient to run the Git Bash shell instead. Here's how:

1. Open the settings.json file (see above).
2. Scroll down to the Integrated Terminal section. 
3. Expand the section to see its settings. You'll find 17 settings in this section. Look for this one:  
   "terminal.integrated.shell.windows": "C:\\WINDOWS\\Sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
4. Edit the path here to point to the Bash shell. If you've installed Git and GitHub with the standard settings, it should be here:  
   "C:\\Program Files\\Git\\bin"
5. Save your changes and close the file.
6. Test your setting: Press Ctrl+` and check which shell appears in the integrated terminal window.

### Setting the default working directory for the integrated terminal
Immediately after the setting that selects which shell to use, you can find the setting for shell arguments. This allows you to open the terminal with preset parameters, among which is the default working directory to use for Git commands. 

To set the default working directory for the shell, find this setting:  
"terminal.integrated.shellArgs.windows": [],

Within the [], insert the following: "--cd=_your Git path_" where -your Git path_ is the path you set when you installed Git to be the top folder containing your Git repos.

Save the .json file and close.

## Working with Git in Visual Studio Code
If you've [installed Git](https://git-scm.com/download), Visual Studio Code incorporates Git functionality very conveniently and transparently. From within VS Code, you can:

* pull the latest changes from remote
* rebase your local tracking branch from its remote 
* create a new branch
* publish and sync your new branch
* undo your last commit

and more. Visual Studio Code lets you select operations from the Git panel, type them into the command palette, or open your default shell (Ctrl+`) to execute Git commands; you can also view Git output.

### Working with Git: Visual Studio Code, vs. GitHub.com vs. GitHub Desktop vs. Git Shell
When you first get started with Git version control, you'll discover that you can do almost all the Git operations you would normally do from any of the Git-related tools at your disposal: the GitHub.com site, the GitHub desktop application, your chosen Git shell (Bash, PowerShell, etc.) or from within Visual Studio Code. Whatever routine you develop is up to you; however, here are some suggestions for which of these tools is best for which Git operations.

| Operation | Suggested tool |
|---|---|
| [Fork a repo](https://help.github.com/articles/fork-a-repo/) | GitHub.com |
| [Rebase from source repo](multiple-bugs.md) | Git shell |
| [Create or delete a remote branch](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/) | GitHub.com |
| [Create a local branch](https://code.visualstudio.com/docs/editor/versioncontrol#_branches-and-tags) | Visual Studio Code |
| [Publish a new local branch to origin](https://code.visualstudio.com/docs/editor/versioncontrol#_git-status-bar-actions) | Visual Studio Code |
| [Commit changes](https://code.visualstudio.com/docs/editor/versioncontrol#_commit) | Visual Studio Code |
| [Undo last commit (before pushing changes or publishing the branch)](https://blogs.msdn.microsoft.com/user_ed/2016/02/08/visual-studio-code-all-the-git-features/#_git-undo-last-commit) | Visual Studio Code | 
| [Undo last commit (after pushing or publishing)](http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html) | Git shell |
| [Undo multiple commits](http://serebrov.github.io/html/2014-01-04-git-revert-multiple-recent-comments.html) | Git shell |
| [Push changes to origin](https://code.visualstudio.com/docs/editor/versioncontrol#_git-status-bar-actions) | Visual Studio Code |
| [Create a pull request](https://help.github.com/articles/creating-a-pull-request/) | GitHub.com |
| [Delete a branch (local, local and remote)](http://stackoverflow.com/questions/32102810/how-do-i-delete-a-local-branch-on-github-desktop) | GitHub Desktop |

### Additional resources for Git in Visual Studio Code
For more information about using the Git features of Visual Studio Code, see these resources:

* [Version Control in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol)
* [Tutorial video: Git Version Control in VS Code](https://code.visualstudio.com/docs/introvideos/versioncontrol)
