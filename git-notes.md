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

## Git Undo & Recovery

### Restore Modified Files

```bash
git restore <file>
```

* Discards unstaged changes.
* Restores Working Directory from Staging Area.

Example:

```bash
git restore restore-lab.txt
```

### Unstage Files

```bash
git restore --staged <file>
```

* Removes changes from Staging Area.
* Keeps Working Directory changes intact.

Example:

```bash
git restore --staged restore-lab.txt
```

### Git Areas

* **HEAD** → Last committed snapshot.
* **Staging Area** → What will be included in the next commit.
* **Working Directory** → Current files being edited.

### Reset Types

#### Soft Reset

```bash
git reset --soft HEAD~1
```

* Moves HEAD only.
* Staging Area unchanged.
* Working Directory unchanged.

Result:

```text
HEAD != Staging Area = Working Directory
```

#### Mixed Reset

```bash
git reset --mixed HEAD~1
```

* Moves HEAD.
* Resets Staging Area to match HEAD.
* Working Directory unchanged.

Result:

```text
HEAD = Staging Area != Working Directory
```

#### Hard Reset

```bash
git reset --hard HEAD~1
```

* Moves HEAD.
* Resets Staging Area.
* Resets Working Directory.

Result:

```text
HEAD = Staging Area = Working Directory
```

### Easy Memory Rule

```text
soft  -> affects HEAD

mixed -> affects HEAD + Staging Area

hard  -> affects HEAD + Staging Area + Working Directory
```

### Lab Observations

```bash
git reset --soft HEAD~1
```

* Removes commit from branch history.
* Keeps changes staged.

```bash
git restore --staged restore-lab.txt
```

* Unstages changes.
* Does not modify file contents.

```bash
git restore restore-lab.txt
```

* Discards Working Directory changes.
* Restores file from Staging Area 

15 June 2026 (Mon) Git & GitHub

## Undo & Recovery

### Restore and Unstage

* `git restore file`

  * Restores Working Directory from Staging Area.
  * Discards unstaged changes.

* `git restore --staged file`

  * Restores Staging Area from HEAD.
  * Unstages changes without modifying Working Directory content.

### Three-Area Model

* HEAD = Last committed snapshot.
* Staging Area (Index) = Next commit snapshot.
* Working Directory = Current files on disk.

### Reset Modes

* `git reset --soft HEAD~1`

  * Moves HEAD only.

* `git reset --mixed HEAD~1`

  * Moves HEAD and resets Staging Area.

* `git reset --hard HEAD~1`

  * Moves HEAD, resets Staging Area, and resets Working Directory.

Memory Rule:

* soft  → HEAD
* mixed → HEAD + Staging Area
* hard  → HEAD + Staging Area + Working Directory

### Recover Deleted Files

* Deleted but not staged:

  * `git restore file`

* Deleted and staged:

  * `git restore --staged file`
  * `git restore file`

* Deleted and committed:

  * Find commit using `git log --oneline`
  * Recover file from older commit:

    * `git checkout <commit> -- <file>`

### Detached HEAD

* Enter detached state:

  * `git checkout <commit>`
  * `git switch --detach <commit>`

* HEAD points directly to a commit instead of a branch.

* Commits created in detached state can become unreachable if no branch references them.

### Detached HEAD Recovery

* Create branch from detached commit:

  * `git branch recovered-work <commit>`

* Create and switch to branch from current detached commit:

  * `git switch -c <branch-name>`

### Practical Lab Outcome

Created multiple histories from Commit B:

* main → C
* recovered-work → D → E
* new-brach-at-B → F

Observed that switching branches changes Working Directory contents to match the checked-out branch tip.

## Branching & Collaboration

### Creating Branches

```bash
git branch
git branch feature-login
```

- Branch = Independent line of development.
- Allows working on features without affecting main branch.

### Switching Branches

```bash
git switch feature-login
git switch main
```

- `git switch` is the modern command for changing branches.

### Understanding git checkout

```bash
git checkout feature-login
git checkout -b feature-login
```

- Older command used for switching branches.
- Can create and switch to a branch in one command.
- `git switch` is preferred for branch operations.

### Merging Branches

```bash
git merge feature-login
```

- Combines changes from one branch into another.
- Usually merge feature branch into main after testing.

### Merge Conflict Resolution

Conflict markers:

```text
<<<<<<< HEAD
Main branch code
=======
Feature branch code
>>>>>>> feature-login
```

Resolution steps:

1. Edit file manually.
2. Remove conflict markers.
3. Keep desired content.
4. Stage resolved file.
5. Commit merge.

```bash
git add file.txt
git commit
```

### Fast-Forward Merge

- Happens when main has no new commits since branch creation.
- Git simply moves branch pointer forward.
- No merge commit created.

### Merge Commit

- Created when both branches have new commits.
- Preserves branch history.
- Creates a new commit joining both histories.

### Merge vs Rebase

#### Merge

```bash
git merge feature-login
```

- Preserves complete branch history.
- May create merge commit.
- Safe for shared branches.

#### Rebase

```bash
git rebase main
```

- Replays commits on top of another branch.
- Creates cleaner linear history.
- Rewrites commit history.

### Important Concepts

- Branch = Independent development path.
- HEAD = Pointer to current branch/commit.
- Fast-Forward Merge = Pointer move only.
- Merge Commit = New commit combining histories.
- Merge = Preserve history.
- Rebase = Rewrite history for cleaner timeline.

### Commands Practiced

```bash
git branch
git branch feature-login
git switch feature-login
git switch main
git checkout feature-login
git checkout -b feature-login
git merge feature-login
git rebase main
git status
git log --oneline --graph --all

## 21 Jun 2026 (Sun) Git & GitHub

### Git Restore & Recovery
- `git restore file` restores file from Staging Area → Working Directory.
- Used when file is modified but not staged.
- After restore:
  - Working Directory = Staging Area.

### Git Unstage Changes
- `git restore --staged file`
- Removes changes from Staging Area.
- Working Directory remains unchanged.
- After command:
  - HEAD = Staging Area
  - Working Directory still contains modifications.

### HEAD vs Staging Area vs Working Directory
- Scenario:
  - Modify file → `git add file` → Modify file again.
- Result:
  - HEAD = Last committed version.
  - Staging Area = Version at time of `git add`.
  - Working Directory = Latest modified version.

### Git Reset
#### Soft Reset
- `git reset --soft HEAD~1`
- HEAD moves back one commit.
- Staging Area unchanged.
- Working Directory unchanged.

#### Mixed Reset
- `git reset --mixed HEAD~1`
- HEAD moves back one commit.
- Staging Area reset to HEAD.
- Working Directory unchanged.

#### Hard Reset
- `git reset --hard HEAD~1`
- HEAD moves back one commit.
- Staging Area reset to HEAD.
- Working Directory reset to HEAD.
- Can permanently discard uncommitted changes.

### Recover Deleted Files
#### File Deleted but Not Staged
- `rm report.txt`
- Recover using:
  - `git restore report.txt`
- Git copies:
  - Staging Area → Working Directory.

#### File Deleted and Staged
- `rm report.txt`
- `git add .`
- Recovery:
  - `git restore --staged report.txt`
  - `git restore report.txt`

#### File Deleted and Commit Already Made
- Find commit:
  - `git log -- report.txt`
- Recover file:
  - `git restore --source=<commit-id> report.txt`
  - or `git checkout <commit-id> -- report.txt`

### Detached HEAD
- Occurs when checking out a commit directly.
- HEAD points to commit instead of branch.
- New commits created here are not attached to any branch.
- To preserve commits:
  - `git switch -c branch_name`
  - or `git branch branch_name`

### Review Results
- Reviewed Git Recovery & Reset flashcards.
- Strong understanding of:
  - HEAD
  - Staging Area (Index)
  - Working Directory
- Identified weak areas:
  - Detached HEAD
  - File recovery from history
  - Restore source/destination concepts

## Git Recovery & Restore Review

- Reviewed `git restore` and `git restore --staged`.
- Practiced recovering deleted tracked files.
- Learned file recovery from commit history:
  - `git checkout <commit-id> -- file`
  - `git restore --source=<commit-id> file`
- Reviewed Detached HEAD:
  - HEAD points directly to a commit.
  - Commits can become unreachable.
  - Preserve work using:
    - `git switch -c <branch-name>`
    - `git branch <branch-name>`

### Key Concepts
- HEAD
- Staging Area (Index)
- Working Directory
- File Recovery
- Detached HEAD

## Learned recovery of lost commits using:

git reflog


# Git & GitHub Notes - 19 June 2026 (Fri)

## Branching & Collaboration

### Branch Creation

```bash
git branch feature-login
```

- Branch = Pointer to the latest commit.
- Creating a branch does not copy files.
- Creating a branch does not switch to it.
- Git branches are lightweight because only a pointer is created.

### Branch Switching

```bash
git switch feature-login
git switch master
```

- HEAD points to the current branch.
- New commits move the branch pointed to by HEAD.
- Switching branches updates the Working Directory to match the target branch commit.

### git checkout

```bash
git checkout feature-login
git checkout -b feature-payment
```

- `git checkout branch-name` → Switch to existing branch.
- `git checkout -b branch-name` → Create and switch to a new branch.
- `git switch` was introduced to simplify branch operations.

### Branch Isolation

Example:

```text
master ---------> Initial Commit

feature-login --> Add login feature
```

- Changes committed in one branch are not automatically visible in another branch.
- Switching branches restores files to the state of the commit pointed to by that branch.

### Fast-Forward Merge

```bash
git merge feature-login
```

Condition:

```text
master -------------> A

feature-login ------> B
```

- Master has no new commits after branching.
- Git simply moves the branch pointer forward.
- No new commit is created.

### Merge Commit

Condition:

```text
master -------------> M1

feature-payment ----> P1
```

- Both branches contain unique commits.
- Fast-forward merge is not possible.
- Git creates a new commit representing the merge result.

Example:

```text
850a888 Merge branch 'feature-payment'
```

### Merge Conflict

Conflict example:

```text
<<<<<<< HEAD
hotfix on master
=======
Payment Feature Added
>>>>>>> feature-payment
```

- HEAD = Current branch version.
- Incoming section = Merged branch version.
- Conflict occurs when Git cannot safely decide how to combine changes.

### Conflict Resolution

Steps:

```bash
nano app.txt
```

Remove conflict markers and keep desired content.

Verify:

```bash
cat app.txt
```

Stage resolved file:

```bash
git add app.txt
```

Meaning during conflict resolution:

```text
I have resolved the conflict.
Use this version.
```

Complete merge:

```bash
git commit
```

### Important Concepts

```text
Branch = Pointer to latest commit

HEAD = Pointer to current branch

Fast-Forward Merge
= Pointer move only

Merge Commit
= New commit joining histories

Merge Conflict
= Git cannot decide automatically

git add during conflict
= Mark conflict as resolved
```

### Commands Practiced

```bash
git branch
git branch feature-login
git switch feature-login
git switch master
git checkout feature-login
git checkout -b feature-payment
git merge feature-login
git merge feature-payment
git status
git log --oneline
git log --oneline --decorate
git log --oneline --decorate --all
cat app.txt
nano app.txt
git add app.txt
git commit
