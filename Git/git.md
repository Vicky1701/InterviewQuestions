# Git Interview Preparation Guide

## 1. Core Git Concepts

### Q1: What is Git and why is it important?
**A:** Git is a distributed version control system that tracks changes in source code during software development. It allows multiple developers to collaborate on projects, maintain complete history of changes, and work on different features simultaneously without conflicts.

**Key Benefits:**
- Distributed architecture (every clone is a full backup)
- Branching and merging capabilities
- Complete change history
- Offline work capability
- Data integrity through checksums

### Q2: Explain the three states of files in Git.
**A:** Git has three main states for files:

1. **Modified**: Changes made to files in working directory but not staged
2. **Staged**: Changes marked to go into next commit (in staging area/index)
3. **Committed**: Changes safely stored in Git repository

### Q3: What are the three main sections of a Git project?
**A:** 
1. **Working Directory**: Where you edit files
2. **Staging Area (Index)**: Snapshot of what will go in next commit
3. **Repository (.git directory)**: Where Git stores metadata and object database

---

## 2. Basic Git Commands & Workflow

### Q4: Walk me through initializing a new Git repository and making your first commit.
**A:**
```bash
# Initialize new repository
git init

# Add files to staging area
git add .
# OR add specific file
git add filename.txt

# Commit changes
git commit -m "Initial commit"

# Add remote repository
git remote add origin https://github.com/username/repo.git

# Push to remote
git push -u origin main
```

### Q5: What's the difference between `git clone` and `git init`?
**A:**
- **`git init`**: Creates a new empty Git repository in current directory
- **`git clone`**: Downloads an existing repository from remote location, including full history

```bash
git init                    # Create new repo
git clone <url>            # Copy existing repo
```

### Q6: Explain the difference between `git add .` and `git add -A`.
**A:**
- **`git add .`**: Stages new and modified files in current directory (not deleted files)
- **`git add -A`**: Stages all changes (new, modified, and deleted files) in entire repository

### Q7: What is the staging area and why is it useful?
**A:** The staging area (index) is an intermediate area where you prepare commits. It allows you to:
- Review changes before committing
- Commit only specific changes from modified files
- Create clean, logical commits
- Use `git add -p` for partial staging

---

## 3. Branching & Merging

### Q8: How do you create and switch to a new branch?
**A:**
```bash
# Create new branch
git branch feature-branch

# Switch to branch
git checkout feature-branch

# Create and switch in one command
git checkout -b feature-branch

# Modern way (Git 2.23+)
git switch -c feature-branch
```

### Q9: What's the difference between `git checkout` and `git switch`?
**A:**
- **`git checkout`**: Multi-purpose command (switch branches, restore files, detach HEAD)
- **`git switch`**: Dedicated command for switching branches (Git 2.23+)
- **`git restore`**: Dedicated command for restoring files

```bash
# Old way
git checkout branch-name
git checkout -- file.txt

# New way
git switch branch-name
git restore file.txt
```

### Q10: Explain the difference between merge and rebase.
**A:**

**Merge:**
- Creates a merge commit
- Preserves branch history
- Non-destructive operation
- Shows when branches were integrated

```bash
git merge feature-branch
```

**Rebase:**
- Replays commits on top of target branch
- Creates linear history
- Rewrites commit history
- Cleaner project history

```bash
git rebase main
```

**When to use:**
- **Merge**: For feature branches, shared branches, preserving context
- **Rebase**: For private branches, cleaning up before sharing, linear history preference

### Q11: What's the difference between fast-forward and recursive merge?
**A:**

**Fast-forward merge:**
- No divergent commits
- Simply moves branch pointer forward
- No merge commit created
- Linear history maintained

**Recursive merge:**
- Branches have diverged
- Creates new merge commit
- Combines changes from both branches
- Shows explicit merge point

```bash
# Force merge commit even in fast-forward case
git merge --no-ff feature-branch
```

### Q12: How do you resolve merge conflicts?
**A:**
1. **Identify conflicts**: Git marks conflicted files
2. **Open conflicted files**: Look for conflict markers
   ```
   <<<<<<< HEAD
   Your changes
   =======
   Their changes
   >>>>>>> branch-name
   ```
3. **Resolve manually**: Edit file to desired state
4. **Mark as resolved**: `git add conflicted-file.txt`
5. **Complete merge**: `git commit`

**Tools for conflict resolution:**
```bash
git mergetool                    # Launch merge tool
git status                       # See conflict status
git diff                         # View differences
```

---

## 4. Remote Repositories

### Q13: Explain the difference between `git fetch` and `git pull`.
**A:**

**`git fetch`:**
- Downloads commits, files, and refs from remote
- Doesn't modify working directory
- Safe operation
- Updates remote-tracking branches

**`git pull`:**
- Equivalent to `git fetch` + `git merge`
- Downloads and immediately integrates changes
- Can cause conflicts
- Modifies current branch

```bash
git fetch origin              # Safe download
git pull origin main          # Download + merge
```

### Q14: How do you set up tracking between local and remote branches?
**A:**
```bash
# Set upstream when pushing
git push -u origin feature-branch

# Set upstream for existing branch
git branch --set-upstream-to=origin/main main

# Check tracking relationships
git branch -vv
```

### Q15: What's the difference between upstream and downstream?
**A:**
- **Upstream**: The remote repository you cloned from (typically origin)
- **Downstream**: Repositories that have cloned from yours

**Related concepts:**
- **Origin**: Default name for cloned remote repository
- **Upstream branch**: Remote branch that local branch tracks

---

## 5. Undoing Changes

### Q16: Explain the different types of `git reset`.
**A:**

**`git reset --soft HEAD~1`:**
- Moves HEAD pointer
- Keeps staging area and working directory unchanged
- Uncommits but keeps changes staged

**`git reset --mixed HEAD~1` (default):**
- Moves HEAD pointer
- Unstages changes
- Keeps working directory unchanged

**`git reset --hard HEAD~1`:**
- Moves HEAD pointer
- Clears staging area
- Discards working directory changes
- **DANGEROUS**: Permanently loses changes

### Q17: When should you use `git revert` vs `git reset`?
**A:**

**Use `git revert`:**
- For shared/public history
- Creates new commit that undoes changes
- Safe for collaboration
- Preserves history

**Use `git reset`:**
- For local/private commits only
- Rewrites history
- When you want to completely remove commits
- Before pushing to remote

```bash
git revert HEAD              # Safe undo
git reset --hard HEAD~1     # Dangerous undo
```

### Q18: How do you use `git stash`?
**A:**
Git stash temporarily saves uncommitted changes:

```bash
git stash                    # Stash current changes
git stash push -m "message"  # Stash with message
git stash list               # List all stashes
git stash apply              # Apply most recent stash
git stash apply stash@{2}    # Apply specific stash
git stash pop                # Apply and remove stash
git stash drop               # Delete stash
git stash clear              # Delete all stashes
```

### Q19: What's the difference between `git checkout --` and `git restore`?
**A:**

**`git checkout -- <file>` (old way):**
```bash
git checkout -- file.txt    # Discard working directory changes
git checkout HEAD file.txt  # Restore from specific commit
```

**`git restore` (new way, Git 2.23+):**
```bash
git restore file.txt                    # Discard working directory changes
git restore --staged file.txt          # Unstage file
git restore --source=HEAD~1 file.txt   # Restore from specific commit
```

---

## 6. Git History & Logs

### Q20: How do you view Git history in different formats?
**A:**
```bash
git log                           # Full log
git log --oneline                 # Compact format
git log --graph                   # ASCII graph
git log --decorate               # Show branch/tag names
git log --oneline --graph --all  # Everything in graph format
git log --since="2 weeks ago"    # Time-based filtering
git log --author="John Doe"      # Filter by author
git log file.txt                 # History for specific file
```

### Q21: How do you compare changes with `git diff`?
**A:**
```bash
git diff                         # Working directory vs staging
git diff --staged               # Staging vs last commit
git diff HEAD                   # Working directory vs last commit
git diff commit1 commit2        # Between specific commits
git diff branch1..branch2       # Between branches
git diff --name-only            # Show only filenames
git diff --stat                 # Show statistics
```

### Q22: What is `git blame` and when would you use it?
**A:**
`git blame` shows who last modified each line of a file:

```bash
git blame file.txt              # Show blame for entire file
git blame -L 10,20 file.txt     # Blame for specific lines
git blame -w file.txt           # Ignore whitespace changes
```

**Use cases:**
- Finding who introduced a bug
- Understanding code ownership
- Tracking feature development
- Code review context

---

## 7. Git Hooks

### Q23: What are Git hooks and how do you use them?
**A:**
Git hooks are scripts that run automatically at specific Git events. They're stored in `.git/hooks/` directory.

**Common hooks:**
- **pre-commit**: Runs before commit is created
- **pre-push**: Runs before push to remote
- **post-merge**: Runs after successful merge
- **post-receive**: Runs on remote after receiving push

**Example pre-commit hook:**
```bash
#!/bin/sh
# Run tests before commit
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed, commit aborted"
    exit 1
fi
```

### Q24: How do you set up a pre-commit hook to run tests?
**A:**
1. Navigate to `.git/hooks/`
2. Create `pre-commit` file (no extension)
3. Make it executable: `chmod +x pre-commit`
4. Add your script:

```bash
#!/bin/sh
echo "Running tests before commit..."
./run-tests.sh
if [ $? -eq 0 ]; then
    echo "Tests passed!"
    exit 0
else
    echo "Tests failed! Commit aborted."
    exit 1
fi
```

---

## 8. Git Best Practices

### Q25: What makes a good commit message?
**A:**
**Format:**
```
Short summary (50 chars or less)

Detailed explanation if needed. Wrap at 72 characters.
- Use bullet points for multiple changes
- Reference issue numbers: Fixes #123
```

**Good examples:**
```
Add user authentication system

Implement JWT-based authentication with login/logout endpoints.
Includes password hashing and session management.
Fixes #45
```

**Bad examples:**
```
fix stuff
WIP
asdfgh
Updated files
```

### Q26: What are atomic commits and why are they important?
**A:**
**Atomic commits** contain one logical change that:
- Can be described in a single sentence
- All tests pass
- Can be safely reverted
- Don't break the build

**Benefits:**
- Easier debugging with `git bisect`
- Cleaner history
- Safer reverts
- Better code review

**Example:**
```bash
# Good: Separate commits
git commit -m "Add user model"
git commit -m "Add user validation"
git commit -m "Add user controller"

# Bad: Everything in one commit
git commit -m "Add complete user system"
```

### Q27: When should you use rebase vs merge?
**A:**

**Use Rebase when:**
- Working on private/feature branches
- Want linear history
- Cleaning up commits before sharing
- Updating feature branch with main

**Use Merge when:**
- Integrating feature branches
- Working with shared branches
- Want to preserve branch context
- Following team conventions

**Golden Rule**: Never rebase commits that exist outside your repository.

### Q28: How do you properly use .gitignore?
**A:**
`.gitignore` specifies files Git should ignore:

**Common patterns:**
```gitignore
# Dependencies
node_modules/
*.jar

# Build outputs
dist/
build/
target/

# IDE files
.vscode/
.idea/
*.swp

# OS files
.DS_Store
Thumbs.db

# Environment files
.env
*.log

# Specific files
config/database.yml
```

**Important notes:**
- Create before adding files
- Use `git rm --cached <file>` to untrack already tracked files
- Use `git check-ignore -v <file>` to debug ignore rules

### Q29: What's the difference between `git rm` and just deleting a file?
**A:**

**Just deleting file:**
- File removed from working directory
- Git sees it as "deleted" but change not staged
- Need `git add` to stage deletion

**Using `git rm`:**
- Removes file and stages the deletion
- One command to delete and stage
- `git rm --cached <file>` removes from Git but keeps file locally

```bash
rm file.txt          # Delete file
git add file.txt     # Stage deletion

# OR

git rm file.txt      # Delete and stage in one command
```

### Q30: How do you handle large files in Git?
**A:**

**Problems with large files:**
- Slows down clones
- Increases repository size
- Poor performance

**Solutions:**
1. **Git LFS (Large File Storage):**
   ```bash
   git lfs install
   git lfs track "*.zip"
   git add .gitattributes
   ```

2. **Separate repository** for assets
3. **External storage** (S3, CDN) with references
4. **Submodules** for large dependencies

**Best practices:**
- Keep binary files separate
- Use `.gitignore` for build artifacts
- Consider alternatives like package managers

---

## Quick Reference Commands

```bash
# Setup
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

# Basic workflow
git init / git clone <url>
git add <files>
git commit -m "message"
git push origin main

# Branching
git branch <name>
git checkout -b <name>
git merge <branch>
git rebase <branch>

# Remote operations
git remote add origin <url>
git fetch origin
git pull origin main
git push -u origin main

# Undoing changes
git reset --hard HEAD~1
git revert HEAD
git stash
git restore <file>

# Information
git status
git log --oneline --graph
git diff
git blame <file>
```

---

## Advanced Interview Questions

### Q31: Explain `git bisect` and when you'd use it.
**A:** `git bisect` helps find the commit that introduced a bug using binary search:

```bash
git bisect start
git bisect bad                    # Current commit is bad
git bisect good <commit-hash>     # Known good commit
# Git checks out middle commit
git bisect good/bad              # Mark current commit
# Repeat until bug is found
git bisect reset                 # Return to original state
```

### Q32: What is a detached HEAD state?
**A:** Detached HEAD occurs when HEAD points to a specific commit instead of a branch:

**Causes:**
- `git checkout <commit-hash>`
- `git checkout <tag>`

**Solutions:**
```bash
git switch -c new-branch-name    # Create branch from current state
git switch main                  # Return to branch (loses changes)
```

### Q33: How do you squash commits?
**A:** Combine multiple commits into one:

**Interactive rebase:**
```bash
git rebase -i HEAD~3            # Squash last 3 commits
# Change "pick" to "squash" for commits to combine
```

**Merge with squash:**
```bash
git merge --squash feature-branch
git commit -m "Add complete feature"
```

This comprehensive guide covers all the essential Git concepts for interview preparation. Practice these commands and concepts to build confidence in your Git knowledge!
