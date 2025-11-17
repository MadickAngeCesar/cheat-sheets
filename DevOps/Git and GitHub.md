# Git and GitHub Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Configuration](#setup--configuration)
- [Basic Commands](#basic-commands)
- [Branching](#branching)
- [Merging](#merging)
- [Rebasing](#rebasing)
- [Remote Operations](#remote-operations)
- [Stashing](#stashing)
- [Tagging](#tagging)
- [Undoing Changes](#undoing-changes)
- [Git Log & History](#git-log--history)
- [GitHub Specific](#github-specific)
- [Advanced Topics](#advanced-topics)
- [Best Practices](#best-practices)

---

## Setup & Configuration

### Initial Setup
```bash
# Set username
git config --global user.name "Your Name"

# Set email
git config --global user.email "your@email.com"

# Set default editor
git config --global core.editor "code --wait"

# Set default branch name
git config --global init.defaultBranch main

# View all config
git config --list

# View specific config
git config user.name

# Edit config file
git config --global --edit
```

### SSH Setup
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your@email.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add SSH key
ssh-add ~/.ssh/id_ed25519

# Copy public key
cat ~/.ssh/id_ed25519.pub

# Test connection
ssh -T git@github.com
```

---

## Basic Commands

### Repository Initialization
```bash
# Initialize new repository
git init

# Clone existing repository
git clone https://github.com/user/repo.git

# Clone to specific directory
git clone https://github.com/user/repo.git my-project

# Clone specific branch
git clone -b develop https://github.com/user/repo.git

# Shallow clone (last commit only)
git clone --depth 1 https://github.com/user/repo.git
```

### Basic Workflow
```bash
# Check status
git status

# Short status
git status -s

# Add files to staging
git add file.txt
git add .
git add *.js

# Add all changes
git add -A

# Add specific parts interactively
git add -p

# Commit changes
git commit -m "Commit message"

# Commit with description
git commit -m "Title" -m "Description"

# Add and commit
git commit -am "Message"

# Amend last commit
git commit --amend

# Push changes
git push

# Pull changes
git pull

# Fetch changes
git fetch
```

---

## Branching

### Branch Operations
```bash
# List branches
git branch

# List all branches (including remote)
git branch -a

# List remote branches
git branch -r

# Create new branch
git branch feature-name

# Switch to branch
git checkout feature-name
git switch feature-name

# Create and switch to branch
git checkout -b feature-name
git switch -c feature-name

# Create branch from specific commit
git branch feature-name abc123

# Rename current branch
git branch -m new-name

# Rename other branch
git branch -m old-name new-name

# Delete branch
git branch -d feature-name

# Force delete branch
git branch -D feature-name

# Delete remote branch
git push origin --delete feature-name

# View branch with last commit
git branch -v

# View merged branches
git branch --merged

# View unmerged branches
git branch --no-merged
```

---

## Merging

### Merge Operations
```bash
# Merge branch into current
git merge feature-name

# Merge with commit message
git merge feature-name -m "Merge message"

# No fast-forward merge
git merge --no-ff feature-name

# Squash merge
git merge --squash feature-name

# Abort merge
git merge --abort

# Continue merge after conflict resolution
git merge --continue
```

### Conflict Resolution
```bash
# View conflicts
git status

# View conflict in file
git diff

# Choose theirs for conflict
git checkout --theirs file.txt

# Choose ours for conflict
git checkout --ours file.txt

# Mark conflict as resolved
git add file.txt

# Continue merge
git commit
```

---

## Rebasing

### Rebase Operations
```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase
git rebase --continue

# Skip current commit
git rebase --skip

# Abort rebase
git rebase --abort

# Rebase onto specific commit
git rebase abc123
```

### Interactive Rebase
```bash
# Start interactive rebase
git rebase -i HEAD~3

# Commands in interactive rebase:
# pick = use commit
# reword = use commit, but edit message
# edit = use commit, but stop for amending
# squash = use commit, but meld into previous
# fixup = like squash, but discard message
# drop = remove commit

# Example:
pick abc123 First commit
squash def456 Second commit
reword ghi789 Third commit
```

---

## Remote Operations

### Remote Management
```bash
# List remotes
git remote

# List remotes with URLs
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# View remote info
git remote show origin
```

### Push & Pull
```bash
# Push to remote
git push origin main

# Push and set upstream
git push -u origin main

# Push all branches
git push --all

# Push tags
git push --tags

# Force push
git push --force

# Force push safely
git push --force-with-lease

# Pull from remote
git pull origin main

# Pull with rebase
git pull --rebase

# Fetch all remotes
git fetch --all

# Fetch and prune
git fetch --prune
```

---

## Stashing

### Stash Operations
```bash
# Stash changes
git stash

# Stash with message
git stash save "Work in progress"

# Stash including untracked files
git stash -u

# Stash everything including ignored files
git stash -a

# List stashes
git stash list

# Show stash content
git stash show
git stash show -p

# Apply stash
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Remove stash
git stash drop

# Remove specific stash
git stash drop stash@{2}

# Clear all stashes
git stash clear

# Create branch from stash
git stash branch feature-name
```

---

## Tagging

### Tag Operations
```bash
# List tags
git tag

# List tags with pattern
git tag -l "v1.*"

# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag specific commit
git tag -a v1.0.0 abc123

# Push tag
git push origin v1.0.0

# Push all tags
git push --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0

# Checkout tag
git checkout v1.0.0

# View tag info
git show v1.0.0
```

---

## Undoing Changes

### Discard Changes
```bash
# Discard changes in working directory
git checkout -- file.txt
git restore file.txt

# Discard all changes
git checkout -- .
git restore .

# Unstage file
git reset HEAD file.txt
git restore --staged file.txt

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Reset to specific commit
git reset --hard abc123

# Revert commit (create new commit)
git revert abc123

# Revert merge commit
git revert -m 1 abc123
```

### Reset vs Revert
```bash
# Reset (changes history)
git reset --hard HEAD~1

# Soft reset (keep changes staged)
git reset --soft HEAD~1

# Mixed reset (keep changes unstaged)
git reset HEAD~1

# Revert (creates new commit)
git revert HEAD

# Revert without commit
git revert -n HEAD
```

---

## Git Log & History

### Viewing History
```bash
# View commit history
git log

# One line per commit
git log --oneline

# Graph view
git log --graph

# Pretty format
git log --graph --oneline --decorate --all

# Last n commits
git log -5

# Commits by author
git log --author="John"

# Commits in date range
git log --since="2 weeks ago"
git log --after="2024-01-01" --before="2024-12-31"

# Commits with specific message
git log --grep="fix"

# Commits that changed specific file
git log file.txt

# Show changes in commits
git log -p

# Show stats
git log --stat

# Show specific commit
git show abc123

# Show file at specific commit
git show abc123:file.txt
```

### Searching History
```bash
# Search for string in commits
git log -S "function_name"

# Search with regex
git log -G "regex_pattern"

# Find who changed line
git blame file.txt

# Blame specific lines
git blame -L 10,20 file.txt
```

---

## GitHub Specific

### GitHub CLI
```bash
# Install GitHub CLI
# Windows: winget install GitHub.cli
# Mac: brew install gh
# Linux: sudo apt install gh

# Login
gh auth login

# Create repository
gh repo create my-repo --public

# Clone repository
gh repo clone user/repo

# View repository
gh repo view

# Create pull request
gh pr create

# List pull requests
gh pr list

# View pull request
gh pr view 123

# Merge pull request
gh pr merge 123

# Create issue
gh issue create

# List issues
gh issue list

# View issue
gh issue view 123
```

### Pull Requests
```bash
# Create PR branch
git checkout -b feature-name

# Push branch
git push -u origin feature-name

# Update PR
git push origin feature-name

# Sync with main
git checkout main
git pull
git checkout feature-name
git merge main

# Or with rebase
git checkout feature-name
git rebase main
git push --force-with-lease
```

### Forking Workflow
```bash
# Fork repository on GitHub

# Clone your fork
git clone https://github.com/yourusername/repo.git

# Add upstream remote
git remote add upstream https://github.com/original/repo.git

# Sync with upstream
git fetch upstream
git checkout main
git merge upstream/main

# Create feature branch
git checkout -b feature-name

# Push to your fork
git push origin feature-name

# Create pull request on GitHub
```

---

## Advanced Topics

### Cherry Pick
```bash
# Apply specific commit
git cherry-pick abc123

# Cherry pick without commit
git cherry-pick -n abc123

# Cherry pick range
git cherry-pick abc123..def456

# Abort cherry pick
git cherry-pick --abort
```

### Submodules
```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recursive https://github.com/user/repo.git

# Update all submodules
git submodule update --remote

# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
```

### Bisect (Find Bug)
```bash
# Start bisect
git bisect start

# Mark current as bad
git bisect bad

# Mark old commit as good
git bisect good abc123

# Test and mark
git bisect good  # or bad

# Automated bisect
git bisect run test.sh

# End bisect
git bisect reset
```

### Worktrees
```bash
# Create worktree
git worktree add ../hotfix hotfix-branch

# List worktrees
git worktree list

# Remove worktree
git worktree remove ../hotfix

# Prune worktrees
git worktree prune
```

### Reflog
```bash
# View reflog
git reflog

# Recover lost commit
git reflog
git checkout abc123

# Recover deleted branch
git reflog
git checkout -b recovered-branch abc123
```

---

## Best Practices

### Commit Messages
```bash
# ✅ Good: Clear, descriptive commit message
git commit -m "Add user authentication feature"
git commit -m "Fix: Resolve login redirect issue"
git commit -m "Refactor: Simplify database query logic"

# ❌ Bad: Vague commit message
git commit -m "Update"
git commit -m "Fix bug"
git commit -m "Changes"

# ✅ Good: Conventional commits
git commit -m "feat: Add user registration"
git commit -m "fix: Resolve memory leak in API"
git commit -m "docs: Update README with setup instructions"
git commit -m "style: Format code according to style guide"
git commit -m "refactor: Restructure authentication module"
git commit -m "test: Add unit tests for user service"
git commit -m "chore: Update dependencies"
```

### Branch Naming
```bash
# ✅ Good: Descriptive branch names
feature/user-authentication
bugfix/login-redirect
hotfix/security-patch
release/v1.2.0

# ❌ Bad: Vague branch names
feature1
fix
update
```

### Workflow Tips
```bash
# ✅ Good: Commit often, push regularly
git add .
git commit -m "Add feature X"
git push

# ✅ Good: Keep commits atomic
# Each commit should represent one logical change

# ✅ Good: Pull before push
git pull --rebase
git push

# ✅ Good: Use .gitignore
node_modules/
.env
.DS_Store
*.log

# ✅ Good: Review changes before commit
git diff
git status
git commit

# ✅ Good: Use branches for features
git checkout -b feature/new-feature
# ... work on feature
git push -u origin feature/new-feature

# ✅ Good: Keep main/master stable
# Always work on feature branches
# Merge only tested code

# ✅ Good: Clean up branches
git branch -d feature/completed
git push origin --delete feature/completed
```

### Security
```bash
# ✅ Never commit sensitive data
# Add to .gitignore:
.env
secrets.json
*.key
*.pem

# ❌ If already committed
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch secrets.json" \
  --prune-empty --tag-name-filter cat -- --all

# Better: Use BFG Repo-Cleaner
bfg --delete-files secrets.json
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

---

**Pro Tip:** Write clear commit messages using conventional commits format, use feature branches for all new work, rebase instead of merge for cleaner history, and always review changes before committing with `git diff` and `git status`!