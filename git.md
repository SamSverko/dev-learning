# [Git](https://git-scm.com/)

---

## Create repositories

`git init`
Turn an existing directory into a git repository.

`git clone [url]`
Clone a repository that already exists on GitHub.

---

## Synchronize changes

`git remote -v`
Lists the set of repositories ("remotes") whose branches you track.

`git fetch`
Downloads all history from the remote tracking branches.

`git merge`
Combines the remote tracking branch into the current local branch

`git push`
Uploads all local branch commits to GitHub

`git pull`
Updates your current local working branch with all new commits from the corresponding remote branch on GitHub. git pull is a combination of git fetch and git merge.

---

## Branches

`git status`
Displays paths that have differences between the index file and the current HEAD commit.

`git branch`
Lists all branches.

`git branch [branch-name]`
Creates a new branch.

`git merge [branch]`
Combines the specified branch’s history into the current branch.

`git branch -d [branch]`
Deletes the specified branch.

`git fetch --prune`
Cleans outdated branches. It will connect to a shared remote repository remote and fetch all remote branch refs. It will then delete remote refs that are no longer in use on the remote repository.

---

## Make changes

`git log`
Lists version history for the current branch.

`git log —-pretty=oneline`
Lists version history with each commit formatted to one line.

`git add [file]`
Shapshots the file in preparation for versioning.

`git commit -m “[descriptive message]”`
Records file snapshots permanently in version history.

`git stash`
Store all your changes away for later use.

`git stash apply [stash]`
Overwrites the current repository to specified stash.

`git stash drop [stash]`
Deletes the specified stash.

---

## Redo commits

`git reset [commit]`
Undoes all commits after specified commit, preserving changes locally.

`git reset —hard [commit]`
Discard all history and changes back to the specified commit.

---

## Rebasing

`git rebase origin/master`
Brings commits of branch up to date with master.

`git rebase -i HEAD~<N>`
Helps you to clean up/revise your commits in batch, by rewriting them.

`git commit --amend`
this helps you to rewrite the immediately most recent commit

## Information

* `git status` Show the working tree status.

## Saving changes

* `git add .` Add all changed files.
* `git commit -m "My head line" -m "My content line."`  Record changes to the repository with head line and content message.
* `git push` Update remote refs along with associated objects.

- CREATE NEW REPO ON GITHUB.COM FIRST.
- `git init` - Initialize Git on a director.
- `git remote add origin [URL]` - Add the origin repo (the one from GitHub) to the local repo you just created.
- `git add .` - Add all files that have been changes.
- `git status` - Show what changes have been made locally.
- `git commit -m "Commit message"` - Commit with a message/title.
- `git push origin master` - Push changes up to master branch on GitHub.
- `git tag` - Show repo tags.
- `git tag -a v0.1.1 -m "First patch test"` - Add an annotated (allowing v0.1.1 naming) tag with message. By not specifying any commit to this tag, it will automatically attach to the most recent commit.
- `git show v0.1.1` - Show that commit based on the tag.
- `git push --tags origin master` - Push tags to origin master without needing to push code.
- MASTER BRANCH WILL ONLY BE PRODUCTION RELEASES.
- ALL DEVELOPMENT WILL BE FROM THE DEVELOPMENT BRANCH, USING FEATURE BRANCHES.
- `git log --oneline --decorate --graph --all` - Print out one lime for each commit, decorate with colours, and put in a graph.
- `git glog` - Setup alias for previous command.
- `git branch develop` - Create new branch `develop`.
- `git branch` - Display branches.
- THE HEAD POINTER TELLS US WHERE THE GIT REPOSITORY IS CURRENTLY POINTING (USUALLY TO THE MASTER BRANCH).
- ALWAYS DO A `git status` BEFORE CHECKING OUT TO A BRANCH.
- `git checkout develop` - Check out to `develop` branch. This will point the HEAD to the `develop` branch.
- [ON `develop` BRANCH] `git merge feature-1` - Merge `feature-1` into `develop` branch.
- `:wq` - Write and exit from Vim editor.
- `git branch -d feature-1` - Completely delete the local branch.
- `git push origin --delete feature-1` - Completely delete the remote branch.
- `git remote -v` - View remote/origin source (fetch and push).
- UPSTREAM IS THE REMOTE, DOWNSTREAM IS THE LOCAL.
- `git pull origin master` - Pull from remote master.
- `git stash` - Take all your changes and stash them away for later use.
- `git stash list` - List all stashed changes.
- `git stash apply stash@{0}` - Take changes and overwrite current repo with changes.
- `git stash drop stash@{0}` - Drop specific stash.
