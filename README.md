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