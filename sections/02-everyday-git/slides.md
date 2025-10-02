# Everyday Git Essentials

---

## Learning Objectives

* <span class="fragment">Get comfortable with the commands you'll use daily: clone, commit, branch, push, pull</span>
* <span class="fragment">See how Git provides an auditable history of who changed what, and when</span>
* <span class="fragment">Develop a shared, repeatable workflow that reduces risk</span>

<!-- This is hands-on from the start. Have everyone open a terminal and follow along. -->

---

## The Daily Git Workflow

**The Essential Commands You'll Use Every Day:**

* <span class="fragment">`git clone` - Get a copy of a repository</span>
* <span class="fragment">`git status` - See what's changed</span>
* <span class="fragment">`git add` - Stage changes for commit</span>
* <span class="fragment">`git commit` - Save changes with a message</span>
* <span class="fragment">`git push` - Send changes to remote repository</span>
* <span class="fragment">`git pull` - Get changes from remote repository</span>
* <span class="fragment">`git branch` - Work with branches</span>
* <span class="fragment">`git log` - See commit history</span>

<!-- Stop here and have everyone run `git --version` to confirm Git is installed. -->

---

## Hands-On: Let's Start!

**First, let's check your Git setup:**

```bash
# Check Git version
git --version

# Check your identity
git config --global user.name
git config --global user.email
```

**If you need to set up your identity:**

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

<!-- Everyone should run these commands now. Help anyone who needs to configure their identity. -->

---

## Repository Setup

### Creating a New Project

```bash
# Start a new project
git init my-first-project
cd my-first-project

# Check status
git status
```

### Getting an Existing Project

```bash
# Clone a repository
git clone https://github.com/octocat/Hello-World.git
cd Hello-World

# Check what we got
ls -la
```

<!-- Have everyone choose one approach and follow along. Create a simple file to work with. -->

---

## Hands-On: Your First Commit

**Let's create something to commit:**

```bash
# Create a simple file
echo "# My First Project" > README.md
echo "This is my first Git repository!" >> README.md

# Check what Git sees
git status
```

**Now let's commit it:**

```bash
# Stage the file
git add README.md

# Check status again
git status

# Commit with a message
git commit -m "Add initial README file"
```

<!-- Everyone should follow along and create their first commit. Check that everyone's status shows "nothing to commit, working tree clean". -->

---

## The Commit Workflow

**The 5-Step Process:**

1. <span class="fragment">**Check status** - `git status`</span>
2. <span class="fragment">**See changes** - `git diff`</span>
3. <span class="fragment">**Stage changes** - `git add filename` or `git add .`</span>
4. <span class="fragment">**Commit changes** - `git commit -m "message"`</span>
5. <span class="fragment">**Push to remote** - `git push`</span>

<!-- Emphasize this is the rhythm they'll use daily. Practice makes it automatic. -->

---

## Hands-On: Making Changes

**Let's modify our file:**

```bash
# Edit the README
echo "" >> README.md
echo "## Features" >> README.md
echo "- Learning Git" >> README.md
echo "- Version control" >> README.md

# Check what changed
git status
git diff
```

**Now commit the changes:**

```bash
git add README.md
git commit -m "Add features list to README"
```

<!-- Everyone should see the diff output and understand what changed. -->

---

## Commit Message Best Practices

**Good commit messages:**

* <span class="fragment">**Present tense**: "Add feature" not "Added feature"</span>
* <span class="fragment">**Imperative mood**: "Fix bug" not "Fixed bug"</span>
* <span class="fragment">**Be descriptive**: "Add user login validation" not "Update code"</span>
* <span class="fragment">**Keep it concise**: First line under 50 characters</span>
* <span class="fragment">**Add details if needed**: Use the body for complex changes</span>


---

**Examples:**
- ✅ "Add user authentication feature"
- ✅ "Fix critical security vulnerability"
- ❌ "Updated stuff"
- ❌ "Fixed things"

<!-- Show examples of good vs bad commit messages. Have them practice writing good ones. -->

---

## Hands-On: Viewing History

**Let's explore our commit history:**

```bash
# Basic log
git log

# One line per commit
git log --oneline

# Show changes in commits
git log -p

# Show graph of branches
git log --graph --oneline --all
```

**Check what changed in a specific commit:**

```bash
# Show details of last commit
git show HEAD

# Show only files changed
git show --name-only HEAD
```

<!-- Everyone should run these commands and see their commit history. Point out the commit hashes, authors, and dates. -->

---

## The Auditable History

**What Git tracks for every change:**

* <span class="fragment">**Who**: Author and committer information</span>
* <span class="fragment">**What**: Exact changes made to files</span>
* <span class="fragment">**When**: Timestamp of each commit</span>
* <span class="fragment">**Why**: Commit message explaining the change</span>
* <span class="fragment">**Where**: Which branch the change was made on</span>

---

**This creates a complete audit trail:**

* <span class="fragment">Every change is traceable</span>
* <span class="fragment">You can see who made what changes</span>
* <span class="fragment">You can understand why changes were made</span>
* <span class="fragment">You can revert any change</span>

<!-- This is the key value proposition of Git - complete accountability and traceability. -->

---

## Hands-On: Working with Remotes

**Let's set up a remote repository:**

```bash
# Check current remotes
git remote -v

# If you have a GitHub account, add a remote
# git remote add origin https://github.com/yourusername/your-repo.git

# Push your main branch
git push -u origin main
```

---

**Simulate working with others:**

```bash
# Switch back to main
git switch main

# Pull any changes (simulate someone else's work)
git pull origin main

# Check status
git status
```

<!-- If possible, have everyone push to a real remote. Otherwise, explain the concept. -->

---

## Daily Workflow Patterns

### Before Starting Work

```bash
# Always start with latest changes
git switch main
git pull origin main

# Check current status
git status
```

---

### During Development

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

---

### Before Ending Work

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

<!-- This is the rhythm they should develop. Practice this workflow. -->

---

## Common Issues and Solutions

### "Your branch is ahead of origin/main"

```bash
# You have local commits not pushed
git push origin main
```

---

### "Your branch is behind origin/main"

```bash
# Remote has changes you don't have
git pull origin main
```

---

### "Please tell me who you are"

```bash
# Git doesn't know your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

---

### "fatal: not a git repository"

```bash
# You're not in a Git repository
git init
# or navigate to an existing one
cd /path/to/git/repository
```

<!-- These are the most common issues beginners face. Practice recognizing and solving them. -->

---

## Hands-On: Troubleshooting Practice

**Let's create some common scenarios:**

```bash
# Scenario 1: Check if you're in a git repo
pwd
git status

# Scenario 2: See what branch you're on
git branch
git status

# Scenario 3: Check your configuration
git config --list | grep user

# Scenario 4: See recent commits
git log --oneline -5
```

**Practice these diagnostic commands:**

* <span class="fragment">`git status` - Always start here</span>
* <span class="fragment">`git log --oneline` - See recent history</span>
* <span class="fragment">`git branch` - Check current branch</span>
* <span class="fragment">`git remote -v` - Check remotes</span>

<!-- Have everyone run these diagnostic commands and understand what they show. -->

---

## AI Assistant Integration

### When to Ask AI for Help

* <span class="fragment">**Command syntax**: "What's the difference between git add . and git add -A?"</span>
* <span class="fragment">**Workflow questions**: "How do I update my feature branch with latest changes from main?"</span>
* <span class="fragment">**Error messages**: "Git says 'rejected' when I try to push - what does this mean?"</span>
* <span class="fragment">**Best practices**: "What's the best way to write commit messages?"</span>

### Example Prompts

* <span class="fragment">"I made changes to a file but git status doesn't show it - why?"</span>
* <span class="fragment">"How do I see what changes I made in the last commit?"</span>
* <span class="fragment">"What's the difference between git pull and git fetch?"</span>
* <span class="fragment">"How do I rename a branch in Git?"</span>

<!-- Position AI as a learning tool, not a crutch. Always try to understand the concepts first. -->

---

## Team Collaboration

### Shared Repository Etiquette

* <span class="fragment">**Always pull before pushing**: `git pull` before `git push`</span>
* <span class="fragment">**Use descriptive commit messages**: Help teammates understand changes</span>
* <span class="fragment">**Push frequently**: Don't let local work get too far ahead</span>
* <span class="fragment">**Communicate about branches**: Let team know what you're working on</span>

### Handling Conflicts

* <span class="fragment">**Pull regularly**: Reduces chance of conflicts</span>
* <span class="fragment">**Communicate with team**: Know who's working on what</span>
* <span class="fragment">**Use branches**: Isolate your work from others</span>

<!-- These are the social aspects of Git that make or break team productivity. -->

---

## Hands-On: Final Practice

**Let's put it all together:**

```bash
# 1. Create a new feature
git switch -c feature/final-practice

# 2. Make multiple commits
echo "// Feature 1" > feature1.js
git add feature1.js
git commit -m "Add feature 1 implementation"

echo "// Feature 2" > feature2.js
git add feature2.js
git commit -m "Add feature 2 implementation"

# 3. Check your work
git log --oneline
git status

# 4. Push your work
git push -u origin feature/final-practice
```

**Review what you've learned:**

* <span class="fragment">Created and managed branches</span>
* <span class="fragment">Made multiple commits with good messages</span>
* <span class="fragment">Viewed commit history</span>
* <span class="fragment">Pushed work to remote</span>

<!-- This is the culmination - they should feel confident with the basic workflow. -->

---

## Key Takeaways

1. <span class="fragment">**Daily workflow is simple**: status → add → commit → push</span>
2. <span class="fragment">**Good commit messages matter**: Help your future self and teammates</span>
3. <span class="fragment">**Branches enable parallel work**: Use them for features and fixes</span>
4. <span class="fragment">**Git history is auditable**: Every change is tracked and recoverable</span>
5. <span class="fragment">**Pull before push**: Always sync with remote before pushing</span>
6. <span class="fragment">**AI can help with syntax**: But understand the concepts first</span>
7. <span class="fragment">**Practice makes perfect**: Use these commands daily to build muscle memory</span>

<!-- Close by emphasizing that this is just the beginning. They now have the foundation to build on. -->

---

## Next Steps

**What to practice this week:**

* <span class="fragment">Use Git for all your projects</span>
* <span class="fragment">Write descriptive commit messages</span>
* <span class="fragment">Create branches for new features</span>
* <span class="fragment">Push your work regularly</span>
* <span class="fragment">Explore `git log` and `git diff` options</span>

**Resources for continued learning:**

* <span class="fragment">Git documentation: `git help <command>`</span>
* <span class="fragment">Interactive tutorials: try.github.io</span>
* <span class="fragment">Ask AI assistants for specific help</span>
* <span class="fragment">Practice with real projects</span>

<!-- End on an encouraging note - they have the tools, now they need to use them. -->

