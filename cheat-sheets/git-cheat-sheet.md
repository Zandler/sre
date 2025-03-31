# GIT CHEAT SHEET

### Setup


|**DESCRIPTION** | **COMMAND** |
|---|---|
| Set you name in version control audit | git config --global user.name <firstname-lastname> |
| Set your email for audit in version control | git config --global user.email <email> | 
| Make git colored | git config --global color-ui auto |
| Init local repository | git init |
| Clone repo remote to local| git clone <repository-url> |


### Daily using
|**DESCRIPTION** | **COMMAND** |
|---|---|
| Show status in working directory | git status |
| Add files to tracking change for commit (stage) | git add . **OR** git add ,especific-file> |
| Reset file to unstage in working dir | gir reset <file> |
| Show files changed but not staged (add) | git dff |
| Show files staged but not commited | git diff --staged |
| Commit files staged (like snaphost, but with files change) | git commit -m "message" |

### Daily using for branch

|**DESCRIPTION** | **COMMAND** |
|---|---|
| List local branchs | git branch |
| Create branch | git branch <name> |
| Switch to another branch |  git checkout <branch-name> |
| Marge branch history in to the current | git merge <branch> |
| show logs in the current branch | git log |

### Revision 
|**DESCRIPTION** | **COMMAND** |
|---|---|
| show logs in the current branch | git log |
| Show commit on branch_1 that not in branch_2 | git log branch_1 branch_2 |
| Apply commit on current branch ahead of another | git rebase <branch> |
| Clear stage, e cameback to especific commit | git reset --hard <commit> |
 