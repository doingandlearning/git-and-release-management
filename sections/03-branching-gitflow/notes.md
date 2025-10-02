# Section 3: Branching and GitFlow

## Learning Objectives
- Learn the GitFlow model for feature development, releases, and hotfixes
- Practice branching, merging, and tagging releases
- Provide structure so the team can work in parallel without overwriting each other's work

## Key Talking Points

### 1. Why Branching Matters

#### The Problem with Linear Development
- **Single branch**: Everyone works on the same code
- **Conflicts**: Changes overwrite each other
- **Risk**: Broken code affects everyone
- **Deployment**: Can't separate stable from experimental code

#### The Branching Solution
- **Parallel work**: Multiple people can work simultaneously
- **Isolation**: Changes don't affect others until merged
- **Experimentation**: Try new features safely
- **Stability**: Keep main branch always deployable

### 2. GitFlow Model Overview

#### Core Branches
- **main/master**: Production-ready code, always deployable
- **develop**: Integration branch for features, latest development
- **feature/***: Individual features being developed
- **release/***: Preparing releases for production
- **hotfix/***: Critical fixes for production

#### Branch Relationships
```
main (production)
  ↑
develop (integration)
  ↑
feature/user-auth (individual work)
```

### 3. Feature Development Workflow

#### Starting a Feature
```bash
# 1. Start from develop branch
git checkout develop
git pull origin develop

# 2. Create feature branch
git checkout -b feature/user-authentication

# 3. Work on feature
# ... make changes ...
git add .
git commit -m "Add login form validation"

# 4. Push feature branch
git push -u origin feature/user-authentication
```

#### Completing a Feature
```bash
# 1. Update feature with latest develop
git checkout develop
git pull origin develop
git checkout feature/user-authentication
git merge develop

# 2. Push updated feature
git push origin feature/user-authentication

# 3. Create Pull Request (via GitHub/GitLab UI)
# 4. After approval, merge via UI
# 5. Delete feature branch
git branch -d feature/user-authentication
```

### 4. Release Management

#### Creating a Release
```bash
# 1. Start from develop
git checkout develop
git pull origin develop

# 2. Create release branch
git checkout -b release/1.2.0

# 3. Update version numbers, changelog, etc.
# ... make release preparations ...

# 4. Commit release preparations
git add .
git commit -m "Prepare release 1.2.0"

# 5. Push release branch
git push -u origin release/1.2.0
```

#### Completing a Release
```bash
# 1. Merge to main
git checkout main
git merge release/1.2.0

# 2. Tag the release
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin v1.2.0

# 3. Merge back to develop
git checkout develop
git merge release/1.2.0

# 4. Delete release branch
git branch -d release/1.2.0
git push origin --delete release/1.2.0
```

### 5. Hotfix Workflow

#### Creating a Hotfix
```bash
# 1. Start from main
git checkout main
git pull origin main

# 2. Create hotfix branch
git checkout -b hotfix/critical-security-fix

# 3. Make the fix
# ... implement fix ...

# 4. Commit the fix
git add .
git commit -m "Fix critical security vulnerability"

# 5. Push hotfix
git push -u origin hotfix/critical-security-fix
```

#### Completing a Hotfix
```bash
# 1. Merge to main
git checkout main
git merge hotfix/critical-security-fix

# 2. Tag the hotfix
git tag -a v1.2.1 -m "Hotfix for critical security issue"
git push origin v1.2.1

# 3. Merge back to develop
git checkout develop
git merge hotfix/critical-security-fix

# 4. Delete hotfix branch
git branch -d hotfix/critical-security-fix
git push origin --delete hotfix/critical-security-fix
```

### 6. Branch Protection and Policies

#### Main Branch Protection
- **Require pull requests**: No direct pushes to main
- **Require reviews**: At least one approval
- **Require status checks**: CI/CD must pass
- **Require up-to-date branches**: Must be current with main

#### Branch Naming Conventions
- **Features**: `feature/description` (e.g., `feature/user-dashboard`)
- **Releases**: `release/version` (e.g., `release/1.2.0`)
- **Hotfixes**: `hotfix/description` (e.g., `hotfix/login-bug`)
- **Bug fixes**: `bugfix/description` (e.g., `bugfix/memory-leak`)

### 7. Merge Strategies

#### Fast-Forward Merge
```bash
# When no conflicts exist
git merge feature-branch
# Creates linear history
```

#### No-Fast-Forward Merge
```bash
# Always create merge commit
git merge --no-ff feature-branch
# Preserves branch history
```

#### Squash Merge
```bash
# Combine all commits into one
git merge --squash feature-branch
git commit -m "Add user authentication feature"
# Clean linear history
```

### 8. Tagging Releases

#### Creating Tags
```bash
# Lightweight tag
git tag v1.2.0

# Annotated tag (recommended)
git tag -a v1.2.0 -m "Release version 1.2.0"

# Tag specific commit
git tag -a v1.2.0 <commit-hash> -m "Release version 1.2.0"
```

#### Working with Tags
```bash
# List tags
git tag

# Show tag information
git show v1.2.0

# Push tags to remote
git push origin v1.2.0
git push origin --tags  # Push all tags

# Delete tag
git tag -d v1.2.0
git push origin --delete v1.2.0
```

### 9. Branch Management

#### Viewing Branches
```bash
# Local branches
git branch

# All branches (local and remote)
git branch -a

# Remote branches only
git branch -r

# Branch with last commit info
git branch -v
```

#### Cleaning Up Branches
```bash
# Delete local branch
git branch -d feature-branch

# Force delete local branch
git branch -D feature-branch

# Delete remote branch
git push origin --delete feature-branch

# Prune remote-tracking branches
git remote prune origin
```

### 10. Common Branching Patterns

#### Feature Branch Pattern
```bash
# Start feature
git checkout develop
git pull origin develop
git checkout -b feature/new-feature

# Work on feature
# ... make changes ...
git add .
git commit -m "Implement new feature"

# Update with latest develop
git checkout develop
git pull origin develop
git checkout feature/new-feature
git merge develop

# Complete feature
git push origin feature/new-feature
# Create PR, review, merge, delete branch
```

#### Bug Fix Pattern
```bash
# Start bug fix
git checkout develop
git pull origin develop
git checkout -b bugfix/fix-issue-123

# Fix the bug
# ... make changes ...
git add .
git commit -m "Fix issue #123: description"

# Complete bug fix
git push origin bugfix/fix-issue-123
# Create PR, review, merge, delete branch
```

### 11. Troubleshooting Branch Issues

#### "Your branch is ahead of origin/develop"
```bash
# Push your changes
git push origin feature-branch
```

#### "Your branch is behind origin/develop"
```bash
# Pull latest changes
git checkout develop
git pull origin develop
git checkout feature-branch
git merge develop
```

#### "Cannot delete branch, it is not fully merged"
```bash
# Check if branch is merged
git branch --merged

# Force delete if you're sure
git branch -D feature-branch
```

#### "Branch already exists"
```bash
# Switch to existing branch
git checkout existing-branch

# Or delete and recreate
git branch -D old-branch
git checkout -b new-branch
```

### 12. AI Assistant Integration

#### When to Ask AI for Help
- **Branch strategy**: "What's the best way to organize branches for a team of 5 developers?"
- **Merge conflicts**: "I have merge conflicts in my feature branch, how do I resolve them?"
- **GitFlow questions**: "How do I handle a hotfix when I have a release branch open?"
- **Branch cleanup**: "How do I clean up old branches that are no longer needed?"

#### Example Prompts
- "I need to create a hotfix but I have unmerged changes in my feature branch"
- "What's the difference between git merge and git rebase for feature branches?"
- "How do I update my feature branch with the latest changes from develop?"
- "I accidentally deleted a branch, can I recover it?"

### 13. Team Collaboration Best Practices

#### Branch Naming
- **Be descriptive**: `feature/user-dashboard` not `feature/update`
- **Use consistent format**: `type/description`
- **Include issue numbers**: `feature/123-user-login`

#### Commit Messages
- **Reference issues**: "Add user login (fixes #123)"
- **Be descriptive**: "Implement OAuth2 authentication"
- **Use present tense**: "Add feature" not "Added feature"

#### Pull Request Practices
- **Small, focused PRs**: One feature per PR
- **Clear descriptions**: What changed and why
- **Link to issues**: Reference related tickets
- **Request reviews**: Tag relevant team members

## Key Takeaways

1. **GitFlow provides structure**: Clear roles for different branch types
2. **Branches enable parallel work**: Multiple developers can work simultaneously
3. **Main branch stays stable**: Always deployable, protected by policies
4. **Feature branches are disposable**: Create, use, merge, delete
5. **Releases are managed**: Separate branches for release preparation
6. **Hotfixes are urgent**: Direct path from main to production
7. **Tags mark releases**: Easy to identify and deploy specific versions
8. **AI can help with strategy**: But understand the concepts first

