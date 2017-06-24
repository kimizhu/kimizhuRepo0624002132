# Publishing to different repos

Every hub and landing page built in DocsProd ultimately gets moved to a different repo and published from there. The production team's access to and control over those repos varies, but in general it is best to assume that the writing team owns publishing their own content.

## Handoff process
Due to variability in repo processes, we provide the option to content teams to pick up the page from DocsProd or to submit a PR into their repo. It is somewhat preferable to allow the writing teams to pick up the content especially for new landing pages because they know exactly where the page is going, if it's replacing existing pages, and so forth. But in the case of updates to existing pages or where the target is clear, the APEX production team can submit the PR using the following standard process:

> [!NOTE]
> Most teams use **master** as a general purpose staging branch and prefer all pull requests be made against it, so unless instructed otherwise we raise PRs there, but can of course use any branch designated by the content teams. 

1. Since we don't have write access to any of these repos, create a fork of the appropriate repo.
2. For higher turnover sites like Azure, creating a branch with the bug number will help keep multiple bugs straight, so create a branch as necessary in the fork.
3. Make the fix, commit it to your fork and raise the pull request against the origin. (Make sure the only files in your PR are the ones you intend!)
4. All OP PRs get validated and built; you'll receive a mail and the PR conversation on GitHub will be updated. In most cases there should also be a task or bug in VSO; be sure to put a link to the PR there as well.
5. Make sure everything looks good and according to customer instructions. (See note on common images below.)
6. Sign off on the PR by writing ```#sign-off``` in the PR conversation on GitHub. In some cases (like Azure) this is absolutely required to clear the do-not-merge flag, but it is a standard for us across all content so we can indicate that we have looked at and approved the built results. If you've been in communication directly with someone about the PR, you can also tag them using the ```@<ID>``` mechanism. Indicate in the VSO item that the PR has been signed off.
7. Move on and wait. Different repos have different cadences in which they approve PRs, and some get overlooked. Having the requestor aware of the status of the PR through VSO will alow them to speed up the process if necessary; they are almost always going to be closer to the repo owners. If necessary, use the [Repo contacts](#repo-contacts) below to ask for expediting or for status.
8. Once the PR is merged into master (or the appropriate origin branch), then resolve the VSO item back to the requestor.

> [!NOTE]
> Because we have a global image library, some images may not appear when you're looking at your built version of the pull request. This is because the pull request is essentially a branch (designated by pr-en-us-NNNN) and there is no corresponding branch of the DocsMedia repo where the common images are stored. This can be concerning to a customer if they are looking at the pull request, or even to us if we're not sure about a specific update or image reference.
> The easiest way to fix this, since all members of the production team have write access to DocsMedia, is to create a branch there that matches the branch of the PR. This will build a set of those images at that branch and the images will work. For example, if you have a PR that builds to branch ```pr-en-us-1066```, create a branch in DocsMedia called ```pr-en-us-1066``` and as soon as it builds you will see the images appear.

## Getting access to repos
We're working on getting the APEX-production and APEX-production-leads access to all repos, but in the interim if you don't have access to any of the below go to the [Microsoft OSS hub](https://repos.opensource.microsoft.com) and look up the repo, then request access to one of the teams that provides read access to it.

## Repo contacts
The people listed below can provide assistance with permissions and individual repo procedures. In some cases this is the same as the content owner, but in other cases is not. Contact Kim Tapia St. Amant for information on the actual content owners if needed. The individuals below are either repo owners or very conversant with merging/publishing processes in each repo; unless listed as "Also content owner," the people listed here will generally not be able to answer any questions regarding content specifics, schedules, etc.

Unless otherwise listed, these repos are in the Microsoft organization. Contacts given are Microsoft aliases, not GitHub IDs.

| Repo                                                                  | Contacts (MS aliases)      | Notes                                        |
| --------------------------------------------------------------------- | -------------------------- | -------------------------------------------- |
| [Azure-Docs-pr](https://github.com/microsoft/azure-docs-pr/) (Azure)  | AZDocPRs                   | Alias is vendor team responsible for PRs     |
| [SQL-Docs-pr](https://github.com/Microsoft/sql-docs-pr) (SQL)         | BarbKess;CraigG            |                                              |
| [EMDocs-pr](https://github.com/Microsoft/EMDocs-pr) (EMS)             | AngRobe                    | AngRobe also content owner; includes Intune  |
| [SCCMDocs-pr](https://github.com/Microsoft/SCCMDocs-pr) (SCCM)        | AngRobe                    | AngRobe also content owner                   |
| [DotNet/docs](https://github.com/dotnet/docs) (.NET)                  | MairaW                     | Not in MS/MSDocs org                         |
| [ASPNet/docs](https://github.com/aspnet/docs) (ASP.NET)               | TDykstra                   |                                              |
| [VCppDocs](https://github.com/Microsoft/vcppdocs) (C++)               | GHogen                     | Also content owner                           |
| [VSDocs](https://github.com/microsoft/vsdocs) (Visual Studio)         | GHogen                     | Also content owner                           |
| [Dynamics365HubPages](https://github.com/Microsoft/dynamics365hubpages) (Dynamics 365) | MargoC                    |                                              |
| TBD (Entity Framework)                                                | TBD                        | Not published; contact Kim for content owner |
| External (Windows)                                                    | JillFra;KMac;RyanTh        | Windows owns content post-handoff            |

