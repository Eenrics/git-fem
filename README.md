```
man man
```
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
- **Commit**: a point in time representing a project in its entirety
- **Index**: a staging area where commits are prepared
- **Squash**: combining multiple commits into one

### Flow
**Untracked** --(git add)--> **Staged** --(git commit)--> **Tracked** --(git push)--> **Remote**

### Commands
- man git-<ops> # for help
- `git config --get --global init.defaultBranch` # get the default branch
- `git config --global --unset init.defaultBranch` # unset the default branch
- `git config --global rerere.enabled` # check if rerere is enabled
- `git config --global --unset rerere.enabled` # enable rerere

### Key Facts
- All git config keys are in the following shape: `<section>.<key>`
- To add a key value, `git config --add --global <key> "<Value>"`
- To view any value of git config `git config --get <key>`
Eg: 
```
git config --add --global user.name "John Doe"
git config --get user.name
```