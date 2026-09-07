# Git Commands Cheat Sheet

A practical Git reference covering the most common commands for **clone, branch, fetch, pull, merge, push, commit, reset, stash, rebase, remote management, undoing changes, and troubleshooting**.

---

## Table of Contents

- [1. Git Setup](#1-git-setup)
- [2. Create a Repository](#2-create-a-repository)
- [3. Clone a Repository](#3-clone-a-repository)
- [4. Check Repository Status](#4-check-repository-status)
- [5. Add and Commit Changes](#5-add-and-commit-changes)
- [6. Branch Commands](#6-branch-commands)
- [7. Remote Commands](#7-remote-commands)
- [8. Fetch](#8-fetch)
- [9. Pull](#9-pull)
- [10. Merge](#10-merge)
- [11. Push](#11-push)
- [12. Rebase](#12-rebase)
- [13. Stash](#13-stash)
- [14. Undo Changes](#14-undo-changes)
- [15. Reset](#15-reset)
- [16. Revert](#16-revert)
- [17. Resolve Merge Conflicts](#17-resolve-merge-conflicts)
- [18. Compare Branches](#18-compare-branches)
- [19. View History](#19-view-history)
- [20. Delete Branches](#20-delete-branches)
- [21. Tags](#21-tags)
- [22. Clean Untracked Files](#22-clean-untracked-files)
- [23. Recover Lost Commits](#23-recover-lost-commits)
- [24. Common Team Workflow](#24-common-team-workflow)
- [25. Safe Workflow for Avoiding Code Loss](#25-safe-workflow-for-avoiding-code-loss)
- [26. Common Problems](#26-common-problems)
- [27. Quick Command Reference](#27-quick-command-reference)

---

# 1. Git Setup

## Check Git version

```bash
git --version
```

## Configure username

```bash
git config --global user.name "Your Name"
```

## Configure email

```bash
git config --global user.email "your@email.com"
```

## View configuration

```bash
git config --global --list
```

## Set default branch to `main`

```bash
git config --global init.defaultBranch main
```

---

# 2. Create a Repository

## Initialize Git in an existing project

```bash
git init
```

Then check:

```bash
git status
```

## Add remote repository

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Check remote:

```bash
git remote -v
```

---

# 3. Clone a Repository

## Clone a repository

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

## Clone into a specific folder

```bash
git clone https://github.com/USERNAME/REPOSITORY.git my-project
```

## Clone a specific branch

```bash
git clone -b branch-name https://github.com/USERNAME/REPOSITORY.git
```

## Clone only the latest commit

```bash
git clone --depth 1 https://github.com/USERNAME/REPOSITORY.git
```

---

# 4. Check Repository Status

```bash
git status
```

Shows:

- Current branch
- Modified files
- Staged files
- Untracked files
- Whether your branch is ahead/behind the remote

---

# 5. Add and Commit Changes

## Add one file

```bash
git add filename.ts
```

## Add multiple files

```bash
git add file1.ts file2.ts
```

## Add all changes

```bash
git add .
```

Or:

```bash
git add -A
```

## Commit

```bash
git commit -m "Add new feature"
```

## Add and commit tracked files

```bash
git commit -am "Fix login issue"
```

> `git commit -am` does **not** include new untracked files.

---

# 6. Branch Commands

## List local branches

```bash
git branch
```

## List all branches

```bash
git branch -a
```

## Create a branch

```bash
git branch feature/login
```

## Create and switch to a branch

```bash
git checkout -b feature/login
```

Recommended modern syntax:

```bash
git switch -c feature/login
```

## Switch branch

```bash
git checkout branch-name
```

Modern syntax:

```bash
git switch branch-name
```

## Rename current branch

```bash
git branch -m new-branch-name
```

## Show current branch

```bash
git branch --show-current
```

---

# 7. Remote Commands

## Show remotes

```bash
git remote -v
```

## Add remote

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

## Change remote URL

```bash
git remote set-url origin https://github.com/USERNAME/NEW-REPOSITORY.git
```

## Remove remote

```bash
git remote remove origin
```

## Show detailed remote information

```bash
git remote show origin
```

---

# 8. Fetch

`git fetch` downloads information from the remote repository **without changing your current working files**.

## Fetch from origin

```bash
git fetch origin
```

## Fetch all remotes

```bash
git fetch --all
```

## Fetch and prune deleted remote branches

```bash
git fetch --prune
```

Or:

```bash
git fetch -p
```

## Fetch a specific branch

```bash
git fetch origin branch-name
```

## Example

Suppose you are on:

```text
nahid
```

and someone pushed new code to:

```text
nahid-dev
```

Fetch it:

```bash
git fetch origin nahid-dev
```

Now you can inspect it without changing your branch.

---

# 9. Pull

`git pull` generally performs:

```text
git fetch
+
git merge
```

## Pull current branch

```bash
git pull
```

## Pull from origin

```bash
git pull origin main
```

## Pull a specific branch

```bash
git pull origin branch-name
```

## Pull using rebase

```bash
git pull --rebase
```

## Pull a specific remote branch with rebase

```bash
git pull --rebase origin main
```

### Fetch vs Pull

| Command | What it does |
|---|---|
| `git fetch` | Downloads remote changes only |
| `git pull` | Downloads + integrates remote changes |
| `git merge` | Integrates another branch |
| `git rebase` | Replays your commits on top of another branch |

### Safer inspection workflow

Instead of immediately running:

```bash
git pull
```

you can first run:

```bash
git fetch origin
```

Then inspect:

```bash
git log --oneline HEAD..origin/main
```

and:

```bash
git diff HEAD origin/main
```

Then decide whether to merge or rebase.

---

# 10. Merge

Merge combines another branch into your current branch.

## Merge another local branch

First switch to the branch that should receive the changes:

```bash
git switch main
```

Then:

```bash
git merge feature/login
```

## Merge a remote branch

First fetch:

```bash
git fetch origin
```

Then:

```bash
git merge origin/branch-name
```

## Example: merge `nahid-dev` into `nahid`

```bash
git switch nahid
git fetch origin
git merge origin/nahid-dev
```

If there are no conflicts, Git completes the merge.

Then push:

```bash
git push origin nahid
```

---

# 11. Push

## Push current branch

```bash
git push
```

## Push to origin

```bash
git push origin branch-name
```

## First push for a new branch

```bash
git push -u origin branch-name
```

After using `-u`, you can usually use:

```bash
git push
```

## Push tags

```bash
git push origin --tags
```

## Delete remote branch

```bash
git push origin --delete branch-name
```

---

# 12. Rebase

Rebase moves your commits so they are based on the latest version of another branch.

## Rebase current branch onto main

```bash
git fetch origin
git rebase origin/main
```

## Example

You are on:

```text
nahid
```

You want your work on top of the latest `nahid-dev`:

```bash
git switch nahid
git fetch origin
git rebase origin/nahid-dev
```

If conflicts happen:

```bash
git status
```

Fix the conflicting files, then:

```bash
git add .
git rebase --continue
```

Abort if necessary:

```bash
git rebase --abort
```

> Rebase rewrites commit history. Avoid rebasing commits that other people are already depending on unless your team agrees.

---

# 13. Stash

Stash temporarily saves uncommitted changes.

## Save changes

```bash
git stash
```

## Save with a message

```bash
git stash push -m "Work in progress"
```

## Include untracked files

```bash
git stash -u
```

## List stashes

```bash
git stash list
```

## Apply latest stash

```bash
git stash apply
```

## Apply a specific stash

```bash
git stash apply stash@{1}
```

## Apply and remove stash

```bash
git stash pop
```

## Delete a stash

```bash
git stash drop stash@{0}
```

## Delete all stashes

```bash
git stash clear
```

---

# 14. Undo Changes

## Undo changes in one file before commit

```bash
git restore filename.ts
```

## Undo all unstaged changes

```bash
git restore .
```

> Be careful: this permanently discards those working-tree changes.

## Unstage a file

```bash
git restore --staged filename.ts
```

## Unstage everything

```bash
git restore --staged .
```

---

# 15. Reset

`git reset` moves the current branch/HEAD to another commit.

## Soft reset

Keeps changes staged:

```bash
git reset --soft HEAD~1
```

Useful when you want to undo the last commit but keep everything staged.

## Mixed reset

Keeps changes in your working directory but unstages them:

```bash
git reset HEAD~1
```

Equivalent:

```bash
git reset --mixed HEAD~1
```

## Hard reset

Deletes local changes:

```bash
git reset --hard HEAD~1
```

> ⚠️ Dangerous. Uncommitted changes can be permanently lost.

## Reset to a specific commit

```bash
git reset --hard COMMIT_HASH
```

---

# 16. Revert

`git revert` creates a **new commit** that reverses an existing commit.

This is generally safer than reset for shared branches.

## Revert a commit

```bash
git revert COMMIT_HASH
```

## Revert the latest commit

```bash
git revert HEAD
```

### Reset vs Revert

| Command | Best use |
|---|---|
| `git reset` | Local/private history |
| `git revert` | Shared/public history |

---

# 17. Resolve Merge Conflicts

When Git reports:

```text
CONFLICT (content): Merge conflict
```

First check:

```bash
git status
```

Git will show the conflicting files.

A conflict may look like:

```text
<<<<<<< HEAD
Your current code
=======
Incoming code
>>>>>>> origin/main
```

Edit the file and keep the correct code.

Then:

```bash
git add filename.ts
```

After resolving all conflicts:

```bash
git commit
```

For a rebase:

```bash
git add filename.ts
git rebase --continue
```

## Abort a merge

```bash
git merge --abort
```

## Abort a rebase

```bash
git rebase --abort
```

---

# 18. Compare Branches

## See commits in remote branch but not local branch

```bash
git log --oneline HEAD..origin/main
```

## See commits in local branch but not remote

```bash
git log --oneline origin/main..HEAD
```

## Compare files

```bash
git diff main..feature/login
```

## Compare current branch with remote

```bash
git diff HEAD origin/main
```

## Show changed files

```bash
git diff --name-only
```

## Show statistics

```bash
git diff --stat
```

## Compare two commits

```bash
git diff COMMIT1 COMMIT2
```

---

# 19. View History

## Basic log

```bash
git log
```

## One-line history

```bash
git log --oneline
```

## Graph view

```bash
git log --oneline --graph --decorate --all
```

This is especially useful for understanding branches and merges.

## Show latest commits

```bash
git log -5 --oneline
```

## Show a specific commit

```bash
git show COMMIT_HASH
```

---

# 20. Delete Branches

## Delete local branch

```bash
git branch -d branch-name
```

## Force delete local branch

```bash
git branch -D branch-name
```

> `-D` can delete a branch even if it has unmerged commits.

## Delete remote branch

```bash
git push origin --delete branch-name
```

---

# 21. Tags

Tags are commonly used for releases.

## Create tag

```bash
git tag v1.0.0
```

## Create annotated tag

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
```

## List tags

```bash
git tag
```

## Push a tag

```bash
git push origin v1.0.0
```

## Push all tags

```bash
git push origin --tags
```

## Delete local tag

```bash
git tag -d v1.0.0
```

## Delete remote tag

```bash
git push origin --delete v1.0.0
```

---

# 22. Clean Untracked Files

Preview what will be removed:

```bash
git clean -n
```

Remove untracked files:

```bash
git clean -f
```

Remove untracked files and directories:

```bash
git clean -fd
```

> ⚠️ Use `git clean` carefully. It can permanently delete untracked files.

---

# 23. Recover Lost Commits

If you accidentally reset, rebase, or lose a commit, check:

```bash
git reflog
```

Example:

```text
abc1234 HEAD@{0}: reset: moving to HEAD~1
def5678 HEAD@{1}: commit: Add payment verification
```

You can recover the previous state:

```bash
git reset --hard def5678
```

> `git reflog` is one of the most useful commands when you think your code or commits have disappeared.

---

# 24. Common Team Workflow

A safe normal workflow:

```bash
git status
git switch your-branch
git fetch origin
git status
git add .
git commit -m "Your changes"
git push origin your-branch
```

If your branch needs the latest changes from `main`:

```bash
git fetch origin
git merge origin/main
```

Then:

```bash
git push origin your-branch
```

---

# 25. Safe Workflow for Avoiding Code Loss

Before merging or pulling another branch, use:

```bash
git status
```

If you have uncommitted work, either commit it:

```bash
git add .
git commit -m "WIP: save current work"
```

or stash it:

```bash
git stash -u
```

Then fetch:

```bash
git fetch origin
```

Inspect remote changes:

```bash
git log --oneline HEAD..origin/branch-name
```

Compare code:

```bash
git diff HEAD origin/branch-name
```

Then merge:

```bash
git merge origin/branch-name
```

If everything looks correct:

```bash
git push origin your-branch
```

---

# 26. Common Problems

## Problem: Push rejected

Example:

```text
! [rejected] branch -> branch (non-fast-forward)
```

First:

```bash
git fetch origin
```

Inspect:

```bash
git log --oneline HEAD..origin/branch
```

Then merge:

```bash
git merge origin/branch
```

Resolve conflicts if necessary, then:

```bash
git push origin branch
```

---

## Problem: "Your branch is behind"

Run:

```bash
git fetch origin
```

Then:

```bash
git status
```

You can merge:

```bash
git merge origin/branch-name
```

Or rebase:

```bash
git rebase origin/branch-name
```

---

## Problem: "Your branch is ahead"

Your local branch contains commits that haven't been pushed.

Run:

```bash
git push
```

---

## Problem: "Your branch is ahead and behind"

First inspect:

```bash
git fetch origin
git status
```

See commits:

```bash
git log --oneline --graph --decorate --all
```

Compare:

```bash
git diff HEAD origin/branch-name
```

Then choose:

```bash
git merge origin/branch-name
```

or:

```bash
git rebase origin/branch-name
```

---

## Problem: I accidentally pulled and my code changed

Do not immediately run more destructive commands.

Check:

```bash
git reflog
```

Also inspect:

```bash
git log --oneline --graph --decorate --all
```

If necessary, recover the previous HEAD:

```bash
git reset --hard HEAD@{1}
```

> Confirm the reflog entry before using `reset --hard`.

---

## Problem: I want to overwrite remote with my local branch

⚠️ Only do this if you are certain the remote commits should be replaced.

Safer force push:

```bash
git push --force-with-lease origin branch-name
```

Avoid:

```bash
git push --force
```

unless you fully understand the consequences.

### Why `--force-with-lease`?

It protects against overwriting remote work that you have not seen since your last fetch.

---

# 27. Quick Command Reference

| Task | Command |
|---|---|
| Initialize repository | `git init` |
| Clone repository | `git clone URL` |
| Check status | `git status` |
| Add file | `git add file` |
| Add all | `git add .` |
| Commit | `git commit -m "message"` |
| List branches | `git branch -a` |
| Create branch | `git switch -c branch` |
| Switch branch | `git switch branch` |
| Fetch remote | `git fetch origin` |
| Fetch all | `git fetch --all` |
| Remove deleted remote branches | `git fetch --prune` |
| Pull | `git pull` |
| Pull with rebase | `git pull --rebase` |
| Merge | `git merge branch` |
| Push | `git push` |
| First push | `git push -u origin branch` |
| Rebase | `git rebase origin/main` |
| Stash | `git stash` |
| Apply stash | `git stash pop` |
| View history | `git log --oneline` |
| Graph history | `git log --oneline --graph --all` |
| Compare branches | `git diff branch1..branch2` |
| Unstage | `git restore --staged .` |
| Discard local changes | `git restore .` |
| Undo last commit, keep changes | `git reset --soft HEAD~1` |
| Undo last commit, unstage changes | `git reset HEAD~1` |
| Delete local branch | `git branch -d branch` |
| Delete remote branch | `git push origin --delete branch` |
| Revert commit | `git revert COMMIT_HASH` |
| Recover history | `git reflog` |
| Show commit | `git show COMMIT_HASH` |
| Clean untracked files | `git clean -n` |
| Force push safely | `git push --force-with-lease` |

---

# Recommended Daily Workflow

For most development work:

```bash
# 1. Check your current state
git status

# 2. Get the latest remote information
git fetch origin

# 3. Check what changed
git log --oneline HEAD..origin/main

# 4. Work on your feature
# ...make changes...

# 5. Review changes
git diff

# 6. Stage
git add .

# 7. Commit
git commit -m "Describe your changes"

# 8. Push
git push origin your-branch
```

If you need to bring another branch into your branch:

```bash
git fetch origin
git merge origin/other-branch
```

Then:

```bash
git push origin your-branch
```

---

# Golden Rules

1. **Always run `git status` before important Git operations.**
2. **Use `git fetch` when you want to inspect remote changes without modifying your working branch.**
3. **Do not blindly use `git pull` when you are unsure what changed remotely.**
4. **Commit your work before large merges/rebases.**
5. **Use `git stash -u` when you need to temporarily save uncommitted work.**
6. **Use `git revert` for safely undoing commits on shared branches.**
7. **Be very careful with `git reset --hard`.**
8. **Prefer `git push --force-with-lease` over `git push --force`.**
9. **Use `git reflog` if you think a commit has disappeared.**
10. **Before merging, fetch first:**

```bash
git fetch origin
```

11. **Never overwrite a shared branch unless you know exactly what you are doing.**
12. **For production/shared branches, prefer predictable history and team-agreed workflows.**

---

## Most Important Commands to Remember

```bash
git status
git fetch origin
git pull
git add .
git commit -m "message"
git push
git branch -a
git switch branch-name
git merge origin/branch-name
git log --oneline --graph --all
git diff
git stash
git reflog
```

