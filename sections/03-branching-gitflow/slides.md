# Branching and GitFlow

---

## Learning Objectives

* <span class="fragment">Learn the GitFlow model for feature development, releases, and hotfixes</span>
* <span class="fragment">Practice branching, merging, and tagging releases</span>
* <span class="fragment">Provide structure so the team can work in parallel without overwriting each other's work</span>

<!-- This builds on the basic Git knowledge from the previous section. Emphasize that this is about team collaboration and structure. -->

---

## Why Branching Matters

### The Problem with Linear Development

* <span class="fragment">**Single branch**: Everyone works on the same code</span>
* <span class="fragment">**Conflicts**: Changes overwrite each other</span>
* <span class="fragment">**Risk**: Broken code affects everyone</span>
* <span class="fragment">**Deployment**: Can't separate stable from experimental code</span>


---

### The Branching Solution

* <span class="fragment">**Parallel work**: Multiple people can work simultaneously</span>
* <span class="fragment">**Isolation**: Changes don't affect others until merged</span>
* <span class="fragment">**Experimentation**: Try new features safely</span>
* <span class="fragment">**Stability**: Keep main branch always deployable</span>

<!-- Use real examples: "Imagine 5 developers all editing the same file at once..." -->

---

## GitFlow Model Overview

### Core Branches

* <span class="fragment">**main/master**: Production-ready code, always deployable</span>
* <span class="fragment">**develop**: Integration branch for features, latest development</span>
* <span class="fragment">**feature/***: Individual features being developed</span>
* <span class="fragment">**release/***: Preparing releases for production</span>
* <span class="fragment">**hotfix/***: Critical fixes for production</span>

---

### Branch Relationships

```
main (production)
  ↑
develop (integration)
  ↑
feature/user-auth (individual work)
```

<!-- Draw this on the board or use the diagram. Show how branches flow into each other. -->

---

## Hands-On: Setting Up GitFlow

**Let's create a proper GitFlow structure:**

```bash
# 1. Start with a clean repository
git init gitflow-demo
cd gitflow-demo

# 2. Create initial commit
echo "# GitFlow Demo Project" > README.md
git add README.md
git commit -m "Initial commit"

# 3. Create develop branch
git branch develop
git checkout develop

# 4. Check our branch structure
git branch -a
```

---

**Verify your setup:**

```bash
# You should see both main and develop branches
git log --oneline --graph --all
```

<!-- Everyone should follow along and create the basic GitFlow structure. -->

---

## Feature Development Workflow

### Starting a Feature

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
```

---

### Completing a Feature

```bash
# 1. Update feature with latest develop
git checkout develop
git pull origin develop
git checkout feature/user-authentication
git merge develop

# 2. Push updated feature
git push -u origin feature/user-authentication

# 3. Create Pull Request (via GitHub/GitLab UI)
# 4. After approval, merge via UI
# 5. Delete feature branch
git branch -d feature/user-authentication
```

<!-- Explain that this is the daily workflow for developers. -->

---

## Hands-On: Feature Development

**Let's create a complete feature:**

```bash
# 1. Create feature branch
git checkout develop
git checkout -b feature/user-dashboard

# 2. Add feature files
echo "# User Dashboard" > dashboard.html
echo "console.log('Dashboard loaded');" > dashboard.js
echo "body { font-family: Arial; }" > dashboard.css

# 3. Commit the feature
git add .
git commit -m "Add user dashboard with HTML, CSS, and JS"

# 4. Check our work
git log --oneline
git status
```

---

**Now let's merge it back:**

```bash
# 5. Switch to develop and merge
git checkout develop
git merge feature/user-dashboard

# 6. Clean up
git branch -d feature/user-dashboard
```

<!-- Everyone should complete this full feature cycle. -->

---

## Release Management

### Creating a Release

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
```

---

### Completing a Release

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

<!-- Show the Release branches diagram here to visualize the process. -->

---

![Release Branches](./ReleaseBranches.svg)

<!-- This diagram shows the complete release workflow with all the branches and merges. -->

---

## Hands-On: Release Management

**Let's create a release:**

```bash
# 1. Create release branch
git checkout develop
git checkout -b release/1.0.0

# 2. Prepare release (update version, changelog)
echo "## Version 1.0.0" > CHANGELOG.md
echo "- Added user dashboard" >> CHANGELOG.md
echo "- Initial release" >> CHANGELOG.md

# 3. Commit release preparation
git add CHANGELOG.md
git commit -m "Prepare release 1.0.0"

# 4. Merge to main
git checkout main
git merge release/1.0.0

# 5. Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"

# 6. Merge back to develop
git checkout develop
git merge release/1.0.0

# 7. Clean up
git branch -d release/1.0.0
```

<!-- Everyone should complete this release cycle and see how it affects both main and develop. -->

---

## Hotfix Workflow

### Creating a Hotfix

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
```

---


### Completing a Hotfix

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

<!-- Emphasize that hotfixes bypass the normal development process for urgent fixes. -->

---

![Hotfix Branches](HotfixBranches.svg)

<!-- This diagram shows how hotfixes work - they branch directly from main and merge back to both main and develop. -->

---

## Hands-On: Hotfix Workflow

**Let's create a critical hotfix:**

```bash
# 1. Create hotfix branch from main
git checkout main
git checkout -b hotfix/security-patch

# 2. Make the fix
echo "// Security fix: validate user input" > security.js
echo "function validateInput(input) {" >> security.js
echo "  return input.replace(/<script>/g, '');" >> security.js
echo "}" >> security.js

# 3. Commit the hotfix
git add security.js
git commit -m "Fix XSS vulnerability in user input"

# 4. Merge to main
git checkout main
git merge hotfix/security-patch

# 5. Tag the hotfix
git tag -a v1.0.1 -m "Security hotfix v1.0.1"

# 6. Merge back to develop
git checkout develop
git merge hotfix/security-patch

# 7. Clean up
git branch -d hotfix/security-patch
```

<!-- Everyone should see how hotfixes work differently from regular features. -->

---

## Branch Protection and Policies

### Main Branch Protection

* <span class="fragment">**Require pull requests**: No direct pushes to main</span>
* <span class="fragment">**Require reviews**: At least one approval</span>
* <span class="fragment">**Require status checks**: CI/CD must pass</span>
* <span class="fragment">**Require up-to-date branches**: Must be current with main</span>

---

### Branch Naming Conventions

* <span class="fragment">**Features**: `feature/description` (e.g., `feature/user-dashboard`)</span>
* <span class="fragment">**Releases**: `release/version` (e.g., `release/1.2.0`)</span>
* <span class="fragment">**Hotfixes**: `hotfix/description` (e.g., `hotfix/login-bug`)</span>
* <span class="fragment">**Bug fixes**: `bugfix/description` (e.g., `bugfix/memory-leak`)</span>

<!-- These are the rules that keep teams organized and prevent chaos. -->

---

## Branch Management

### Viewing Branches

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

---

### Cleaning Up Branches

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

---

**Branch hygiene:**

* <span class="fragment">**Delete merged branches**: Keep repository clean</span>
* <span class="fragment">**Regular cleanup**: Remove old feature branches</span>
* <span class="fragment">**Consistent naming**: Follow team conventions</span>
* <span class="fragment">**Documentation**: Comment complex branches</span>

<!-- Clean branches make the repository easier to navigate. -->

---

## Hands-On: Troubleshooting Practice

**Let's create and solve common problems:**

```bash
# 1. Create a branch with uncommitted changes
git checkout develop
git checkout -b feature/problem-branch
echo "// Work in progress" > wip.js
git add wip.js

# 2. Try to switch branches (this will fail)
git checkout develop

# 3. Commit or stash the changes
git commit -m "WIP: work in progress"

# 4. Now switch branches
git checkout develop

# 5. Create a merge conflict
git checkout feature/problem-branch
echo "// Conflicting change" > conflict.js
git add conflict.js
git commit -m "Add conflicting file"

git checkout develop
echo "// Different change" > conflict.js
git add conflict.js
git commit -m "Add different conflicting file"

# 6. Try to merge (this will create a conflict)
git merge feature/problem-branch

# 7. Resolve the conflict and complete merge
# Edit conflict.js to resolve the conflict
git add conflict.js
git commit -m "Resolve merge conflict"
```

<!-- Everyone should experience these common problems and learn how to solve them. -->

---

## AI Assistant Integration

### When to Ask AI for Help

* <span class="fragment">**Branch strategy**: "What's the best way to organize branches for a team of 5 developers?"</span>
* <span class="fragment">**GitFlow questions**: "How do I handle a hotfix when I have a release branch open?"</span>
* <span class="fragment">**Branch cleanup**: "How do I clean up old branches that are no longer needed?"</span>

---

### Example Prompts

* <span class="fragment">"I need to create a hotfix but I have unmerged changes in my feature branch"</span>
* <span class="fragment">"How do I update my feature branch with the latest changes from develop?"</span>
* <span class="fragment">"I accidentally deleted a branch, can I recover it?"</span>

<!-- AI can help with complex branching scenarios, but understand the concepts first. -->

---

## Team Collaboration Best Practices

### Branch Naming

* <span class="fragment">**Be descriptive**: `feature/user-dashboard` not `feature/update`</span>
* <span class="fragment">**Use consistent format**: `type/description`</span>
* <span class="fragment">**Include issue numbers**: `feature/123-user-login`</span>

---

### Commit Messages

* <span class="fragment">**Reference issues**: "Add user login (fixes #123)"</span>
* <span class="fragment">**Be descriptive**: "Implement OAuth2 authentication"</span>
* <span class="fragment">**Use present tense**: "Add feature" not "Added feature"</span>

---


## Key Takeaways

1. <span class="fragment">**GitFlow provides structure**: Clear roles for different branch types</span>
2. <span class="fragment">**Branches enable parallel work**: Multiple developers can work simultaneously</span>
3. <span class="fragment">**Main branch stays stable**: Always deployable, protected by policies</span>
4. <span class="fragment">**Feature branches are disposable**: Create, use, merge, delete</span>
5. <span class="fragment">**Releases are managed**: Separate branches for release preparation</span>
6. <span class="fragment">**Hotfixes are urgent**: Direct path from main to production</span>

<!-- This is the foundation for professional Git workflows. -->
