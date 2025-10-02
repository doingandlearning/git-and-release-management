# Section 2: Everyday Git Essentials

## Learning Objectives
- Get comfortable with the commands you'll use daily: clone, commit, branch, push, pull
- See how Git provides an auditable history of who changed what, and when
- Develop a shared, repeatable workflow that reduces risk

## Key Talking Points

### 1. The Daily Git Workflow
**The Essential Commands You'll Use Every Day:**
- `git clone` - Get a copy of a repository
- `git status` - See what's changed
- `git add` - Stage changes for commit
- `git commit` - Save changes with a message
- `git push` - Send changes to remote repository
- `git pull` - Get changes from remote repository
- `git branch` - Work with branches
- `git log` - See commit history

### 2. Repository Setup and Configuration

#### Initial Git Configuration
```bash
# Set your identity (do this once per machine)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check your configuration
git config --list
```

#### Creating and Cloning Repositories
```bash
# Start a new project
git init my-project
cd my-project

# Get an existing project
git clone https://github.com/user/repo.git
cd repo
```

### 3. The Commit Workflow

#### Making Good Commits
```bash
# 1. Check what's changed
git status

# 2. See the actual changes
git diff

# 3. Stage specific files
git add filename.txt
# or stage all changes
git add .

# 4. Commit with a descriptive message
git commit -m "Add user authentication feature"

# 5. Push to remote
git push origin main
```

#### Commit Message Best Practices
- **Present tense**: "Add feature" not "Added feature"
- **Imperative mood**: "Fix bug" not "Fixed bug"
- **Be descriptive**: "Add user login validation" not "Update code"
- **Keep it concise**: First line under 50 characters
- **Add details if needed**: Use the body for complex changes

### 4. Working with Remote Repositories

#### Understanding Remotes
```bash
# See configured remotes
git remote -v

# Add a remote
git remote add origin https://github.com/user/repo.git

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git
```

#### Push and Pull Operations
```bash
# Push to remote (first time)
git push -u origin main

# Push to remote (subsequent times)
git push

# Pull latest changes
git pull

# Pull from specific branch
git pull origin feature-branch
```

### 5. Branching Basics

#### Creating and Switching Branches
```bash
# Create a new branch
git branch feature-name

# Switch to a branch
git switch feature-name
# or (older syntax)
git checkout feature-name

# Create and switch in one command
git switch -c feature-name

# List all branches
git branch -a

# Delete a branch
git branch -d feature-name
```

### 6. Viewing History and Changes

#### Git Log Variations
```bash
# Basic log
git log

# One line per commit
git log --oneline

# Show changes in commits
git log -p

# Show graph of branches
git log --graph --oneline --all

# Show commits by author
git log --author="John Doe"

# Show commits in date range
git log --since="2 weeks ago"
```

#### Git Diff Variations
```bash
# Changes in working directory
git diff

# Changes staged for commit
git diff --staged

# Changes between commits
git diff commit1..commit2

# Changes between branches
git diff main..feature-branch
```

### 7. The Auditable History

#### What Git Tracks
- **Who**: Author and committer information
- **What**: Exact changes made to files
- **When**: Timestamp of each commit
- **Why**: Commit message explaining the change
- **Where**: Which branch the change was made on

#### Viewing Commit Details
```bash
# Show detailed commit information
git show <commit-hash>

# Show commit statistics
git show --stat <commit-hash>

# Show only the files changed
git show --name-only <commit-hash>
```

### 8. Common Workflow Patterns

#### Feature Development Workflow
```bash
# 1. Start from main branch
git checkout main
git pull origin main

# 2. Create feature branch
git switch -c feature/user-authentication

# 3. Make changes and commit
git add .
git commit -m "Add login form validation"

# 4. Push feature branch
git push -u origin feature/user-authentication

# 5. Create pull request (via GitHub/GitLab UI)
# 6. After approval, merge and delete branch
```

#### Hotfix Workflow
```bash
# 1. Create hotfix branch from main
git checkout main
git pull origin main
git switch -c hotfix/critical-bug-fix

# 2. Make fix and commit
git add .
git commit -m "Fix critical security vulnerability"

# 3. Push and create pull request
git push -u origin hotfix/critical-bug-fix
```

### 9. Best Practices for Daily Use

#### Before Starting Work
```bash
# Always start with latest changes
git checkout main
git pull origin main

# Check current status
git status
```

#### During Development
```bash
# Check status frequently
git status

# Stage changes incrementally
git add specific-file.txt

# Commit small, logical changes
git commit -m "Descriptive message"

# Push regularly to backup work
git push
```

#### Before Ending Work
```bash
# Check what you've changed
git status
git diff

# Commit any remaining changes
git add .
git commit -m "WIP: Feature in progress"

# Push to remote
git push
```

### 10. Troubleshooting Common Issues

#### "Your branch is ahead of origin/main"
```bash
# This means you have local commits not pushed
git push origin main
```

#### "Your branch is behind origin/main"
```bash
# This means remote has changes you don't have
git pull origin main
```

#### "Please tell me who you are"
```bash
# Git doesn't know your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

#### "fatal: not a git repository"
```bash
# You're not in a Git repository
# Either initialize one or navigate to an existing one
git init
# or
cd /path/to/git/repository
```

### 11. AI Assistant Integration

#### When to Ask AI for Help
- **Command syntax**: "What's the difference between git add . and git add -A?"
- **Workflow questions**: "How do I update my feature branch with latest changes from main?"
- **Error messages**: "Git says 'rejected' when I try to push - what does this mean?"
- **Best practices**: "What's the best way to write commit messages?"

#### Example Prompts
- "I made changes to a file but git status doesn't show it - why?"
- "How do I see what changes I made in the last commit?"
- "What's the difference between git pull and git fetch?"
- "How do I rename a branch in Git?"

### 12. Team Collaboration Considerations

#### Shared Repository Etiquette
- **Always pull before pushing**: `git pull` before `git push`
- **Use descriptive commit messages**: Help teammates understand changes
- **Push frequently**: Don't let local work get too far ahead
- **Communicate about branches**: Let team know what you're working on

#### Handling Conflicts
- **Pull regularly**: Reduces chance of conflicts
- **Communicate with team**: Know who's working on what
- **Use branches**: Isolate your work from others

## Key Takeaways

1. **Daily workflow is simple**: status → add → commit → push
2. **Good commit messages matter**: Help your future self and teammates
3. **Branches enable parallel work**: Use them for features and fixes
4. **Git history is auditable**: Every change is tracked and recoverable
5. **Pull before push**: Always sync with remote before pushing
6. **AI can help with syntax**: But understand the concepts first
7. **Practice makes perfect**: Use these commands daily to build muscle memory

