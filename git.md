# [Git](https://git-scm.com/)

---

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
