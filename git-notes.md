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
