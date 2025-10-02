# Section 5: Dealing with Merge Conflicts

## Learning Objectives
- Deliberately create and resolve merge conflicts step-by-step
- Learn strategies to prevent conflicts before they occur
- Build resilience for when collaboration inevitably causes overlaps

## Key Talking Points

### 1. What Are Merge Conflicts?

#### Definition
- **Merge Conflict**: When Git cannot automatically merge changes because the same lines were modified in different ways
- **Conflict Markers**: Special markers in files showing conflicting changes
- **Resolution**: The process of manually choosing which changes to keep

#### Why Conflicts Happen
- **Parallel Development**: Multiple people work on the same files
- **Different Changes**: Same lines modified in different ways
- **Git Can't Decide**: Automatic merge is impossible
- **Human Decision Required**: Someone must choose the correct version

### 2. Understanding Conflict Markers

#### Conflict Markers Explained
```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
```

- **`<<<<<<< HEAD`**: Start of your changes (current branch)
- **`=======`**: Separator between conflicting changes
- **`>>>>>>> branch-name`**: End of their changes (incoming branch)

#### Example Conflict
```python
def calculate_total(items):
<<<<<<< HEAD
    total = sum(item.price for item in items)
    return total * 1.1  # Add 10% tax
=======
    total = sum(item.price for item in items)
    return total * 1.15  # Add 15% tax
>>>>>>> feature/tax-update
```

### 3. Types of Merge Conflicts

#### Content Conflicts
- **Same lines modified**: Most common type
- **Different logic**: Same area, different implementation
- **Different values**: Same variable, different value

#### File Conflicts
- **File deleted**: One branch deletes, other modifies
- **File renamed**: Different names for same file
- **File moved**: Different locations for same file

#### Binary File Conflicts
- **Images, documents**: Can't merge automatically
- **Must choose**: One version or the other
- **No conflict markers**: Git shows both versions

### 4. Conflict Resolution Strategies

#### Strategy 1: Accept Current Changes (HEAD)
```bash
# Keep your changes, discard theirs
git checkout --ours filename
```

#### Strategy 2: Accept Incoming Changes
```bash
# Keep their changes, discard yours
git checkout --theirs filename
```

#### Strategy 3: Manual Resolution
1. **Edit the file** to resolve conflicts
2. **Remove conflict markers**
3. **Choose the correct code**
4. **Test the resolution**
5. **Stage the resolved file**

#### Strategy 4: Use Merge Tool
```bash
# Configure merge tool
git config --global merge.tool vimdiff
# or
git config --global merge.tool vscode

# Launch merge tool
git mergetool
```

### 5. Step-by-Step Conflict Resolution

#### Step 1: Identify the Conflict
```bash
# Check which files have conflicts
git status

# See the conflict details
git diff
```

#### Step 2: Open the Conflicted File
```bash
# Open in your preferred editor
code filename.py
# or
vim filename.py
```

#### Step 3: Resolve the Conflict
1. **Find conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`)
2. **Understand both changes**
3. **Choose the correct version**
4. **Remove conflict markers**
5. **Test the resolution**

#### Step 4: Stage the Resolution
```bash
# Add the resolved file
git add filename.py

# Check status
git status
```

#### Step 5: Complete the Merge
```bash
# Commit the merge
git commit -m "Resolve merge conflict in filename.py"

# Or if it's a merge commit
git commit  # Git will open editor with merge message
```

### 6. Prevention Strategies

#### Communication
- **Coordinate with team**: Know who's working on what
- **Use feature branches**: Isolate changes
- **Regular updates**: Pull frequently from main
- **Small, focused changes**: Reduce conflict probability

#### Git Workflow
- **Pull before push**: Always sync with remote
- **Rebase instead of merge**: Cleaner history
- **Use .gitignore**: Avoid unnecessary conflicts
- **Lock files when needed**: For binary files

#### Code Organization
- **Modular design**: Separate concerns
- **Different files**: Avoid same file modifications
- **Clear interfaces**: Well-defined APIs
- **Documentation**: Clear code structure

### 7. Advanced Conflict Resolution

#### Using Git Rebase
```bash
# Rebase feature branch on main
git checkout feature-branch
git rebase main

# Resolve conflicts during rebase
# Edit files, then:
git add resolved-file
git rebase --continue

# If you want to abort:
git rebase --abort
```

#### Using Git Merge Tool
```bash
# Configure merge tool
git config --global merge.tool vimdiff
git config --global mergetool.keepBackup false

# Use merge tool
git mergetool

# Clean up backup files
git clean -f
```

#### Three-Way Merge
```bash
# Show all three versions
git show :1:filename  # Common ancestor
git show :2:filename  # Your version
git show :3:filename  # Their version
```

### 8. Common Conflict Scenarios

#### Scenario 1: Same Function, Different Implementation
```python
# Your version
def calculate_tax(amount):
    return amount * 0.1

# Their version
def calculate_tax(amount):
    if amount > 1000:
        return amount * 0.15
    return amount * 0.1

# Resolution: Combine both
def calculate_tax(amount):
    if amount > 1000:
        return amount * 0.15
    return amount * 0.1
```

#### Scenario 2: Different Variable Names
```python
# Your version
def process_data(user_input):
    result = user_input.upper()
    return result

# Their version
def process_data(data):
    output = data.upper()
    return output

# Resolution: Choose one naming convention
def process_data(user_input):
    result = user_input.upper()
    return result
```

#### Scenario 3: File Deletion Conflict
```bash
# One branch deletes file, other modifies
# Resolution: Decide if file is needed
git rm filename  # Keep deletion
# or
git add filename  # Keep modification
```

### 9. Conflict Resolution Tools

#### Built-in Tools
- **Git diff**: Show differences
- **Git status**: Show conflicted files
- **Git log**: Show commit history
- **Git show**: Show specific commits

#### External Tools
- **VS Code**: Built-in merge conflict resolution
- **Vim**: Built-in merge tool
- **Beyond Compare**: Professional merge tool
- **KDiff3**: Cross-platform merge tool

#### IDE Integration
- **Visual Studio Code**: Excellent conflict resolution UI
- **IntelliJ IDEA**: Built-in merge tool
- **Eclipse**: Git integration
- **Sublime Text**: Merge conflict packages

### 10. Troubleshooting Common Issues

#### "Cannot resolve conflicts"
```bash
# Check if you're in the middle of a merge
git status

# If you see "You have unmerged paths"
git add resolved-files
git commit
```

#### "Merge conflict in binary file"
```bash
# Choose one version
git checkout --ours binary-file
# or
git checkout --theirs binary-file
git add binary-file
git commit
```

#### "Conflicts after rebase"
```bash
# Continue rebase after resolving
git add resolved-files
git rebase --continue

# Or abort if you want to start over
git rebase --abort
```

#### "Too many conflicts"
```bash
# Abort the merge/rebase
git merge --abort
# or
git rebase --abort

# Try a different approach
git pull --rebase origin main
```

### 11. Best Practices for Conflict Resolution

#### Before Starting
- **Understand the changes**: Read both versions carefully
- **Test both versions**: See which one works
- **Communicate with team**: Understand the intent
- **Backup your work**: Create a branch before resolving

#### During Resolution
- **Take your time**: Don't rush the resolution
- **Test frequently**: Ensure code still works
- **Document decisions**: Comment on why you chose certain changes
- **Ask for help**: When in doubt, ask the team

#### After Resolution
- **Test thoroughly**: Run all tests
- **Review the resolution**: Make sure it makes sense
- **Commit clearly**: Write descriptive commit messages
- **Push and communicate**: Let team know conflicts are resolved

### 12. AI Assistant Integration

#### When to Ask AI for Help
- **Complex conflicts**: "I have a merge conflict in a complex function, how do I resolve it?"
- **Understanding changes**: "What does this conflict mean? What are the differences?"
- **Resolution strategies**: "What's the best way to resolve this type of conflict?"
- **Git commands**: "How do I abort a merge and start over?"

#### Example Prompts
- "I have a merge conflict in my Python file. Here's the conflict: [paste conflict]. How should I resolve it?"
- "Git is showing merge conflicts but I don't understand what changed. Can you help me understand the differences?"
- "I accidentally started a merge and want to abort it. How do I do that?"
- "What's the difference between git merge and git rebase for resolving conflicts?"

### 13. Team Collaboration for Conflicts

#### Communication
- **Notify team**: Let others know about conflicts
- **Discuss resolution**: Get input on complex conflicts
- **Share context**: Explain why changes were made
- **Document decisions**: Record resolution rationale

#### Process
- **Assign ownership**: Who resolves which conflicts
- **Review resolutions**: Have someone else check the fix
- **Test together**: Ensure resolution works for everyone
- **Learn from conflicts**: Improve process to prevent future ones

#### Tools
- **Screen sharing**: Resolve conflicts together
- **Code review**: Have others review resolutions
- **Documentation**: Record common conflict patterns
- **Training**: Teach team conflict resolution skills

## Key Takeaways

1. **Conflicts are normal**: Part of collaborative development
2. **Understand before resolving**: Read both versions carefully
3. **Test your resolution**: Ensure code still works
4. **Communicate with team**: Get help when needed
5. **Prevention is better**: Use good practices to avoid conflicts
6. **Tools can help**: Use merge tools and IDEs
7. **AI can assist**: But human judgment is still essential
8. **Practice makes perfect**: The more you resolve, the better you get




