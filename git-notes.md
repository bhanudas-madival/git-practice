# Git Practice — Day 1

## git log
- git log
- git log --oneline
- git log --stat
- git log -p

## git show
Shows commit details

## git diff
Shows changes before commit

## git tag
- git tag v1.0.0
- git tag -a v1.0.0 -m "release"

## vim exit
:q
:wq

## pager exit
q

11 June 2026 (Thu) Git Fundamentals

# Git Fundamentals

- `git init` creates a Git repository by creating the hidden `.git/` directory.
- `.git/` stores commit history, branches, configuration, and repository metadata.
- `git status` shows repository state: untracked, modified, staged files, and current branch.
- Git file lifecycle:
  - Untracked → Staged → Committed
- `git add <file>` stages a specific file.
- `git add .` stages all changes in the current directory tree.
- `git rm --cached <file>` unstages/removes a file from Git tracking without deleting the file from disk.
- Staging Area allows selecting specific changes before creating a commit.
- `git commit -m "message"` creates a snapshot/checkpoint of staged changes.
- Good commit messages answer:
  - "This commit will..."
- `git diff`
  - Working Directory vs Staging Area
  - Shows unstaged changes.
- `git diff --staged`
  - Last Commit vs Staging Area
  - Shows what will be included in the next commit.
- `git log` displays commit history.
- `git log --oneline` displays compact commit history.
- Commit hashes uniquely identify commits.
- Working Directory → Staging Area → Commit History is the core Git workflow.

# Lab Performed

- Created repository with `git init`
- Created `notes.txt`
- Verified repository state with `git status`
- Staged file using `git add`
- Unstaged file using `git rm --cached`
- Created first commit
- Modified tracked file
- Compared changes using `git diff`
- Compared staged changes using `git diff --staged`
- Created second commit
- Viewed history using `git log` and `git log --oneline`
