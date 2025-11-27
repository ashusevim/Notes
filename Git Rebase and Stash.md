# Git Advanced

# Git stash

- temporarily shelves uncommitted local changes
- it basically allows you to revert the working directory to match with the HEAD commit
- this is very useful when you quickly needs to change the context or a different branch or task without committing the incomplete work

## use case

- For example, you are working on some features, and an urgent bug report comes in and you needs to switch to a different branch to fix the bug
1. stash your changes: save your current work
2. switch branches & fix the bug: your working directory is not clean
3. return and reapply: come back to that feature branch and restore your stash changes to continue where you left off

# Git Rebase

- **`git rebase`** is a way to move or combine a sequence of commits to a new base commit.