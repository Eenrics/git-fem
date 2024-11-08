- https://theprimeagen.github.io/fem-git

## manual pages
```
man man
man git -
man git-<opt>
```
- bold text: type exactly as shown
- italics: replace with appropriate argument
- `[-abc]`: any or all argument within [] are optional
- -a|-b: options are mutually exclusive
- argument ...: can be repeated
- [expression]: entire expression within

navigation with man
- `j`: goes on line down
- `k`: goes one line up
- `d`: goes half page down
- `u`: goes half page up
- `b`: goes one page up
- `f`: goes one page down
- `/`: search for a word
- `n`: goes to the next search result
- `N`: goes to the previous search result

## GIT
- git is a distributed version control system.
- git commands are divided into high level (porcelain) and low level (plumbing) commands.

### Terms
- **Repo**: a git tracked project
- **Commit**: a point in time representing a project in its entirety. 
It is sha hash of 40 characters (0-9, a-f) calculated from the content of change, author, time... . 
If you know the first 7 characters of a commit, git can identify it.
Commit exist in `.git/objects` directory with the first 2 characters as directory name and the rest 18 characters as file name.
- **Index** / **Staging**: a staging area where commits are prepared
- **Squash**: combining multiple commits into one
- **Working tree**: the git repo on the file system (of your version)

### Flow
**Untracked** --(git add)--> **Staged** --(git commit)--> **Tracked** --(git push)--> **Remote**

### Commands (Configs)
- man git-<ops> # for help
- `git config --get --global init.defaultBranch` # get the default branch
- `git config --global --unset init.defaultBranch` # unset the default branch
- `git config --global rerere.enabled` # check if rerere is enabled
- `git config --global --unset rerere.enabled` # enable rerere
- All git configs **keys** are in this shape: `<section>.<key>`. 
- `--global` will set the config globally (for all future repos). 
- To add a **key value**, `git config --add --global <key> "<value>"`. 
- To view any value of git config `git config --get <key>`. Eg: `git config --add --global user.name "John Doe"`. 

####
- `git init` # initialize a git repo
- `man git-log` # manual page for git log
- `git log --graph --decorate --parents` # show the commit history 
(graph: show the branches, decorate: show the refs like branch names i.e. (HEAD -> main, origin/main, origin/HEAD), parents: show the parent commits). When redirecting to a file of git log without --decorate, it will show the commit history without the refs.
- `git log --oneline <branch-name>` # show the commit history of a branch
- `git log <remote-name>/<branch-name>` # show the commit history of a remote branch
- `git log -S "<search-term>"` # search for a term in the commit history
- `git cat-file -p <SHA>` # show the contents of a commit
- `tree` is a directory and `blob` is a file
Eg. 
```bash
git cat-file -p 6741de0d6f14de44dee180bcb1192c7b72eddb05
git cat-file -p d343f28b9cc00e13c0e69ea7e57166336bc44474
git cat-file -p 018274e2803b2eb5f34d5d2717232f5016099779
```
- git does not store diffs, it stores the entirity of the file
### Key Facts
- All git config keys are in the following shape: `<section>.<key>`
- To add a key value, `git config --add --global <key> "<Value>"`
- To view any value of git config `git config --get <key>`
Eg: 
```
git config --add --global user.name "John Doe"
git config --get user.name
```
- A key is made up of two parts: section and keyname
Eg:
```bash
git config --add fem.dev "John Doe"
```
- To list or get the value of a key,
```bash
git config --list
git config --get-regexp fem
git config --get fem.dev
git config --get-all fem.dev
cat .git/config # git config is a file and stored in .git/config in a toml format
```
- To remove / unset a key,
```bash
git config --unset fem.dev
git config --unset-all fem.dev
```
- To remove the whole section,
```bash
git config --remove-section fem
```
- Can set globally or locally
```bash
git config --add --global fem.dev "Global John Doe"
git config --add --local fem.dev "Local John Doe"
```
- You only need the first 7 characters of SHA to identify a commit
- The first 2 characters of SHA are used as directory names in .git/objects and the rest 18 characters are used as file names
- To create a branch, `git branch <branch-name>`. This will create a branch but not switch to it. To switch to it, `git checkout <branch-name>` or `git switch <branch-name>`
- To create a branch and switch in one command, `git switch -c <branch-name>` or `git checkout -b <branch-name>`
- You can find your branch in `.git/refs/heads/<branch-name>` as SHA
- To go to a branch, `git checkout <branch-name>` or `git switch <branch-name>`
- To delete a branch, `git branch -d <branch-name>`.

# Merge
- `git merge <branch-name>` will merge the branch into the current branch
- If the best common ancestor for the two branches is the same as the current branch, git will do a fast forward merge (no new commit is created and it won't ask for a commit message).

# Rebase
- rebase updates the commit where the branch originally points to. It will take the commits from the branch you are on and apply them on top of the branch you are rebasing on. Then we can fast forward merge the branch.
- `git rebase <target-branch>` will rebase the current branch on top of the branch you are rebasing on.
    1. checkout the latest commit on <target-branch>
    2. play one commit at a time from <current-branch>
    3. once finished, will update<current-branch> to the current commit sha
- It alters the history of the branch. You have to force push the branch to the remote. Don't rebase a branch that is shared with others.

# Head and Reflog and Cherry-pick
- `HEAD` is a reference to the current commit. It is a pointer to the current branch.
- It is stored in `.git/HEAD` file.
- `Reflog` is a log of where HEAD has been. It is stored in `.git/logs/HEAD` file.
- `git reflog` will show the reflog of HEAD. 
- `git reflog -<number>` will show the last <number> of reflog entries. 
- `git reflog show <branch-name>` / `git reflog <branch-name>` will show the reflog of the branch.
- We can recover a commit that has been lost by using the reflog or a branch that has been deleted. 
We can use the reflog sha and we can see the files that were changed using cat-file.
Or we can merge that sha into the current branch. But this might not be the best way to do it as it can bring other previous commits that we don't want.
- We can pick one specific commit from another branch and apply it to the current branch using cherry-pick. 
Cherry-pick requires your working tree to be clean. (no uncommitted changes)
- `git cherry-pick <SHA>` will apply the commit to the current branch.

# Remote Repositories
- remote repositories are the one that is not your current repository. It can be remotely on github, gitlab or even on your local machine.
- `git remote` will show the remote repositories.
- `git remote add <remote-name> <url>` will add a remote repository. i.e. `git remote add origin ../remote-repo`
- `git remote remove <remote-name>` will remove a remote repository.
- `git remote -v` will show the remote repositories with their urls.
- If you have remote git source of truth repo, it is convention to name it `origin`.
- If you forked a repo, it is conventional to name your forked repo `origin` and the original repo `upstream`.
- `git fetch` or `git fetch <remote-name>` will fetch the remote repository. We have to merge the fetched branch to the current branch `git merge <remote-name>/<branch-name>`.
- `git pull` or `git pull <remote-name> <branch-name>`: fetch and merge the remote branch to the current branch.
- `git pull --rebase` or `git pull --rebase <remote-name> <branch-name>`: fetch and rebase the remote branch to the current branch.
- `git branch --set-upstream-to=<remote-name>/<branch-name> <branch-name>`: will set the upstream branch to the remote branch.
- `git config --add --global pull.rebase true`: will set the default pull to rebase globally.

# Stash
- stash is a stack of temporary changes.
- `git stash`: will stash the changes.
- `git stash -m "<message>"`: will stash the changes with a message.
- `git stash list`: will show the list of stashes.
- `git stash show [--index <index>]`: will show the stash.
- `git stash pop`: will apply the stash and remove it from the stash list.
- `git stash pop --index <index>`: will apply the stash and remove it from the stash list.
- We can use the stash to move the changes to another branch or to pull the changes from another branch and pop the stash.

# Merge Conflicts
- merge conflicts occur when two branches diverge and git cannot automatically merge them.
- `git merge <branch-name>` will merge the branch into the current branch.
- `git status` will show the files with conflicts with the message `both modified`.
- `git diff` will show the changes in the file.
- `git diff --staged` will show the changes that are staged.
- `git diff --base` will show the base file. `git diff --ours` will show the changes in the current branch. `git diff --theirs` will show the changes in the branch you are merging.
- `git checkout --ours <file>` will keep the changes in the current branch. `git checkout --theirs <file>` will keep the changes in the branch you are merging.
- `git checkout --theirs <file>` will keep the changes in the branch you are merging.
- after resolving the conflicts, `git add <file>` will resolve the conflict. `git status` will not show anything as there is no change. `git commit` will commit the changes or `git merge --continue` will continue the merge.
- `git merge --abort` will abort the merge and go back to the state before the merge.

# Rebase Conflicts
- when rebasing a conflict, the places of theirs and ours are reversed when compared to merge conflicts. This is because rebase checks out the branch you are rebasing and plays on top. So `ours` is theirs changes and `theirs` is our changes.
- `git rebase <branch-name>` will rebase the current branch on top of the branch you are rebasing on.
- `git status` will show the files with conflicts with the message `both modified`.
- `git diff` will show the changes in the file.
- `git diff --base` will show the base file. `git diff --ours` will show the changes in the branch you are rebasing. `git diff --theirs` will show the changes in the current branch.
- after resolving the conflicts, `git add <file>` will resolve the conflict. `git status` will not show anything as there is no change. `git rebase --continue` will continue the rebase.
- `git rebase --abort` will abort the rebase and go back to the state before the rebase.
- `git rebase --skip` will skip the commit that has a conflict.
- One of the danger of using git rebase is that it alters the history of the branch and you might lose some commits.
- The other problem is that once you have conflicts, and you have resolved them, when pulling changes from remote again, you will have conflicts again even the one you have resolved before. For this one we can use reuse recorded resolution (rerere).
- `git rebase -i <commitish>`: This will squash multiples commits into one. i.e. `git rebase -i HEAD~3` will squash the last 3 commits into one.

# Rerere
- We have to enable rerere to use it. `git config --add rerere.enabled true`

- You don't want to mix rebase and merge. If you rebase a branch and then merge it, you will have conflicts. If you merge a branch and then rebase it, you will have conflicts.

# Git Log    
- `git log --grep="<pattern>"` or `git log --grep "<pattern>"` will search for a pattern in the commit message.
- `git log --grep="<pattern>" -p` will include the changes in the commit.
- `git log -p -- <file1> <file2>` will show the commits of only these files.
- `git log -S "<pattern>" -p -- <file>` will search for a pattern in the changes of a file.

# Git Bisect
- Git bisect is a binary search tool to find the commit that introduced a bug.
- `git bisect start` will start the bisect.
- `git bisect bad` will mark the current commit as bad.
- `git bisect good <SHA>` will mark the commit as good. i.e. the initial commit.
- Then we will need to repeat `git bisect bad` or `git bisect good` until we find the commit that introduced the bug.
- `git bisect reset` will reset the bisect.
- `git bisect run <command>` will run a command to find the commit that introduced the bug.

# Git Revert, Git Reset, Git Restore and Worktrees
- `git revert <commitish>`: will create a new commit that will undo the changes of the commit. It is like putting an antidote to the commit. It does not remove the commit from the history but it adds a new commit that will undo the changes.
- We could have conflicts when reverting a commit. We can use `git revert --continue` to continue the revert, `git revert --abort` to abort the revert or `git revert --skip` to skip the commit.
- `git reset --soft HEAD~1`: your branch walkback 1 commit and the changes are staged. You can commit the changes. This is useful to make a commit that is partially finished and you want to edit the commit and change the contents.
- `git commit --ammend`: allows you to meld the current staged changes into the previous commit and edit the commit message. `git commit --ammend --no-edit` will amend the last commit without changing the commit message.
- `git reset --hard`: will do the same thing as soft except it drops changes to the index and workingtree. It will destroy tracked unstaged changes and staged changes. It does not destroy untracked changes. Only destroys what git knows about. We can restore destroyed changes using reflog.
- `git reset --hard HEAD~1`: will walk back 1 commit and destroy the changes. `git reset --hard <commitish>` will walk back to the commit and destroy the changes.
- `git reset --hard HEAD$` will walk back to the last commit and destroy the changes. `git reset --hard HEAD@{1}` will walk back to the last commit and destroy the changes.
- `git restore <file>`: will restore the file to the last commit. `git restore --staged <file>` will unstage the file. `git restore --source <commitish> <file>` will restore the file to the commit.
- `git worktree add <path>`: will add a new worktree to the repo. This is useful when you want to work on a different branch without changing the current branch. You can work on the new branch and then delete the worktree. You can have multiple worktrees.
- `git worktree add <path> <branch-name>`: will add a new worktree to the repo and switch to the branch. `git worktree add <path> <commitish>` will add a new worktree to the repo and switch to the commit.
- `git worktree list`: will show the list of worktrees.
- `git worktree remove <path>`: will remove the worktree. Or we can do `rm -rf <path>` and then `git worktree prune` to remove the worktree.