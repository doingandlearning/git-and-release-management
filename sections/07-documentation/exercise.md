# Exercise 7: Documenting the Process

## Setup
For this exercise, we'll create comprehensive documentation for your team's Git workflow and processes.

```bash
# Create a documentation repository
mkdir team-git-documentation
cd team-git-documentation
git init
git config user.name "Documentation Team"
git config user.email "docs@example.com"

# Create documentation structure
mkdir docs templates examples
echo "# Team Git Documentation" > README.md
echo "Comprehensive documentation for our team's Git workflow and processes." >> README.md
```

## Exercise 1: Creating Team Workflow Documentation

### Part A: Workflow Diagram
1. **Create a Mermaid workflow diagram**:
   ```bash
   echo "```mermaid" > docs/workflow-diagram.md
   echo "graph TD" >> docs/workflow-diagram.md
   echo "    A[Start Feature] --> B[Pull Latest Main]" >> docs/workflow-diagram.md
   echo "    B --> C[Create Feature Branch]" >> docs/workflow-diagram.md
   echo "    C --> D[Develop Feature]" >> docs/workflow-diagram.md
   echo "    D --> E[Commit Changes]" >> docs/workflow-diagram.md
   echo "    E --> F[Push to Remote]" >> docs/workflow-diagram.md
   echo "    F --> G[Create Pull Request]" >> docs/workflow-diagram.md
   echo "    G --> H[Code Review]" >> docs/workflow-diagram.md
   echo "    H --> I{Approved?}" >> docs/workflow-diagram.md
   echo "    I -->|No| J[Address Feedback] >> docs/workflow-diagram.md
   echo "    J --> E" >> docs/workflow-diagram.md
   echo "    I -->|Yes| K[Merge to Main]" >> docs/workflow-diagram.md
   echo "    K --> L[Deploy to Production]" >> docs/workflow-diagram.md
   echo "    L --> M[Delete Feature Branch]" >> docs/workflow-diagram.md
   echo "```" >> docs/workflow-diagram.md
   ```

2. **Create detailed workflow steps**:
   ```bash
   echo "# Team Git Workflow" > docs/workflow-steps.md
   echo "" >> docs/workflow-steps.md
   echo "## 1. Starting a New Feature" >> docs/workflow-steps.md
   echo "" >> docs/workflow-steps.md
   echo "### Prerequisites" >> docs/workflow-steps.md
   echo "- Ensure you have the latest changes from main" >> docs/workflow-steps.md
   echo "- Verify you're on the main branch" >> docs/workflow-steps.md
   echo "" >> docs/workflow-steps.md
   echo "### Steps" >> docs/workflow-steps.md
   echo "1. **Pull latest changes**: \`git pull origin main\`" >> docs/workflow-steps.md
   echo "2. **Create feature branch**: \`git checkout -b feature/description\`" >> docs/workflow-steps.md
   echo "3. **Push to remote**: \`git push -u origin feature/description\`" >> docs/workflow-steps.md
   echo "" >> docs/workflow-steps.md
   echo "### Example" >> docs/workflow-steps.md
   echo "\`\`\`bash" >> docs/workflow-steps.md
   echo "git checkout main" >> docs/workflow-steps.md
   echo "git pull origin main" >> docs/workflow-steps.md
   echo "git checkout -b feature/user-authentication" >> docs/workflow-steps.md
   echo "git push -u origin feature/user-authentication" >> docs/workflow-steps.md
   echo "\`\`\`" >> docs/workflow-steps.md
   ```

3. **Commit the workflow documentation**:
   ```bash
   git add docs/
   git commit -m "Add team workflow documentation"
   ```

## Exercise 2: Creating Cheat Sheets

### Part A: Git Commands Cheat Sheet
1. **Create a comprehensive cheat sheet**:
   ```bash
   echo "# Git Commands Cheat Sheet" > docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "## Repository Setup" >> docs/git-cheat-sheet.md
   echo "\`\`\`bash" >> docs/git-cheat-sheet.md
   echo "# Initialize new repository" >> docs/git-cheat-sheet.md
   echo "git init" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Clone existing repository" >> docs/git-cheat-sheet.md
   echo "git clone <url>" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Add remote repository" >> docs/git-cheat-sheet.md
   echo "git remote add origin <url>" >> docs/git-cheat-sheet.md
   echo "\`\`\`" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "## Daily Workflow" >> docs/git-cheat-sheet.md
   echo "\`\`\`bash" >> docs/git-cheat-sheet.md
   echo "# Check repository status" >> docs/git-cheat-sheet.md
   echo "git status" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Stage changes" >> docs/git-cheat-sheet.md
   echo "git add <file>          # Stage specific file" >> docs/git-cheat-sheet.md
   echo "git add .               # Stage all changes" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Commit changes" >> docs/git-cheat-sheet.md
   echo "git commit -m \"message\"" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Push to remote" >> docs/git-cheat-sheet.md
   echo "git push                # Push current branch" >> docs/git-cheat-sheet.md
   echo "git push origin <branch> # Push specific branch" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Pull latest changes" >> docs/git-cheat-sheet.md
   echo "git pull                # Pull current branch" >> docs/git-cheat-sheet.md
   echo "git pull origin <branch> # Pull specific branch" >> docs/git-cheat-sheet.md
   echo "\`\`\`" >> docs/git-cheat-sheet.md
   ```

2. **Add branching commands**:
   ```bash
   echo "" >> docs/git-cheat-sheet.md
   echo "## Branching" >> docs/git-cheat-sheet.md
   echo "\`\`\`bash" >> docs/git-cheat-sheet.md
   echo "# List branches" >> docs/git-cheat-sheet.md
   echo "git branch              # Local branches" >> docs/git-cheat-sheet.md
   echo "git branch -a           # All branches" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Create branch" >> docs/git-cheat-sheet.md
   echo "git branch <name>       # Create branch" >> docs/git-cheat-sheet.md
   echo "git checkout -b <name>  # Create and switch" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Switch branches" >> docs/git-cheat-sheet.md
   echo "git checkout <branch>   # Switch to branch" >> docs/git-cheat-sheet.md
   echo "git switch <branch>     # Modern way to switch" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Merge branches" >> docs/git-cheat-sheet.md
   echo "git merge <branch>      # Merge branch into current" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Delete branches" >> docs/git-cheat-sheet.md
   echo "git branch -d <name>    # Delete local branch" >> docs/git-cheat-sheet.md
   echo "git push --delete <name> # Delete remote branch" >> docs/git-cheat-sheet.md
   echo "\`\`\`" >> docs/git-cheat-sheet.md
   ```

3. **Add troubleshooting commands**:
   ```bash
   echo "" >> docs/git-cheat-sheet.md
   echo "## Troubleshooting" >> docs/git-cheat-sheet.md
   echo "\`\`\`bash" >> docs/git-cheat-sheet.md
   echo "# Undo changes" >> docs/git-cheat-sheet.md
   echo "git restore <file>      # Undo working directory changes" >> docs/git-cheat-sheet.md
   echo "git restore --staged <file> # Unstage file" >> docs/git-cheat-sheet.md
   echo "git reset HEAD~1        # Undo last commit" >> docs/git-cheat-sheet.md
   echo "git revert <commit>     # Create undo commit" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# View history" >> docs/git-cheat-sheet.md
   echo "git log                 # Show commit history" >> docs/git-cheat-sheet.md
   echo "git log --oneline       # Compact history" >> docs/git-cheat-sheet.md
   echo "git log --graph --all   # Show branch graph" >> docs/git-cheat-sheet.md
   echo "git show <commit>       # Show commit details" >> docs/git-cheat-sheet.md
   echo "" >> docs/git-cheat-sheet.md
   echo "# Find lost work" >> docs/git-cheat-sheet.md
   echo "git reflog              # Show all Git activity" >> docs/git-cheat-sheet.md
   echo "git checkout <commit>   # Go to specific commit" >> docs/git-cheat-sheet.md
   echo "\`\`\`" >> docs/git-cheat-sheet.md
   ```

4. **Commit the cheat sheet**:
   ```bash
   git add docs/git-cheat-sheet.md
   git commit -m "Add Git commands cheat sheet"
   ```

## Exercise 3: Creating Team Conventions

### Part A: Naming Conventions
1. **Create naming conventions document**:
   ```bash
   echo "# Team Conventions" > docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "## Branch Naming" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Format" >> docs/team-conventions.md
   echo "Use the format: \`type/description\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Types" >> docs/team-conventions.md
   echo "- **feature/**: New features or enhancements" >> docs/team-conventions.md
   echo "  - Example: \`feature/user-authentication\`" >> docs/team-conventions.md
   echo "  - Example: \`feature/payment-integration\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "- **bugfix/**: Bug fixes" >> docs/team-conventions.md
   echo "  - Example: \`bugfix/login-error\`" >> docs/team-conventions.md
   echo "  - Example: \`bugfix/memory-leak\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "- **hotfix/**: Critical production fixes" >> docs/team-conventions.md
   echo "  - Example: \`hotfix/security-patch\`" >> docs/team-conventions.md
   echo "  - Example: \`hotfix/database-connection\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "- **release/**: Release preparation" >> docs/team-conventions.md
   echo "  - Example: \`release/1.2.0\`" >> docs/team-conventions.md
   echo "  - Example: \`release/2.0.0\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Description Guidelines" >> docs/team-conventions.md
   echo "- Use lowercase letters" >> docs/team-conventions.md
   echo "- Use hyphens to separate words" >> docs/team-conventions.md
   echo "- Be descriptive but concise" >> docs/team-conventions.md
   echo "- Avoid special characters" >> docs/team-conventions.md
   ```

2. **Add commit message conventions**:
   ```bash
   echo "" >> docs/team-conventions.md
   echo "## Commit Messages" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Format" >> docs/team-conventions.md
   echo "Use the format: \`type(scope): description\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Types" >> docs/team-conventions.md
   echo "- **feat**: New features" >> docs/team-conventions.md
   echo "- **fix**: Bug fixes" >> docs/team-conventions.md
   echo "- **docs**: Documentation changes" >> docs/team-conventions.md
   echo "- **style**: Code formatting changes" >> docs/team-conventions.md
   echo "- **refactor**: Code refactoring" >> docs/team-conventions.md
   echo "- **test**: Test additions or changes" >> docs/team-conventions.md
   echo "- **chore**: Maintenance tasks" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Examples" >> docs/team-conventions.md
   echo "- \`feat(auth): add OAuth2 login\`" >> docs/team-conventions.md
   echo "- \`fix(api): handle null response from external service\`" >> docs/team-conventions.md
   echo "- \`docs(readme): update installation instructions\`" >> docs/team-conventions.md
   echo "- \`refactor(utils): simplify data processing logic\`" >> docs/team-conventions.md
   echo "- \`test(auth): add unit tests for login function\`" >> docs/team-conventions.md
   echo "" >> docs/team-conventions.md
   echo "### Guidelines" >> docs/team-conventions.md
   echo "- Use present tense (\"add\" not \"added\")" >> docs/team-conventions.md
   echo "- Use imperative mood (\"fix\" not \"fixes\")" >> docs/team-conventions.md
   echo "- Keep description under 50 characters" >> docs/team-conventions.md
   echo "- Capitalize first letter of description" >> docs/team-conventions.md
   echo "- Don't end with a period" >> docs/team-conventions.md
   ```

3. **Commit the conventions**:
   ```bash
   git add docs/team-conventions.md
   git commit -m "Add team naming and commit conventions"
   ```

## Exercise 4: Creating Troubleshooting Guides

### Part A: Common Issues Guide
1. **Create troubleshooting guide**:
   ```bash
   echo "# Troubleshooting Guide" > docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "## Common Git Issues" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### \"fatal: not a git repository\"" >> docs/troubleshooting.md
   echo "**Problem**: Git commands don't work" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "**Solution**: You're not in a Git repository" >> docs/troubleshooting.md
   echo "1. Check if you're in the right directory: \`pwd\`" >> docs/troubleshooting.md
   echo "2. Initialize repository: \`git init\`" >> docs/troubleshooting.md
   echo "3. Or navigate to existing repo: \`cd /path/to/repo\`" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### \"Your branch is ahead of origin/main\"" >> docs/troubleshooting.md
   echo "**Problem**: Local changes not pushed to remote" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "**Solution**: Push your changes" >> docs/troubleshooting.md
   echo "1. Push to remote: \`git push\`" >> docs/troubleshooting.md
   echo "2. If first time: \`git push -u origin main\`" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### \"Your branch is behind origin/main\"" >> docs/troubleshooting.md
   echo "**Problem**: Remote has changes you don't have" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "**Solution**: Pull latest changes" >> docs/troubleshooting.md
   echo "1. Pull changes: \`git pull origin main\`" >> docs/troubleshooting.md
   echo "2. If conflicts: resolve them and commit" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### \"Please tell me who you are\"" >> docs/troubleshooting.md
   echo "**Problem**: Git doesn't know your identity" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "**Solution**: Configure Git" >> docs/troubleshooting.md
   echo "1. Set name: \`git config --global user.name \"Your Name\`" >> docs/troubleshooting.md
   echo "2. Set email: \`git config --global user.email \"your.email@example.com\`" >> docs/troubleshooting.md
   echo "3. Verify: \`git config --list\`" >> docs/troubleshooting.md
   ```

2. **Add merge conflict troubleshooting**:
   ```bash
   echo "" >> docs/troubleshooting.md
   echo "## Merge Conflicts" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### How to Identify" >> docs/troubleshooting.md
   echo "1. Git will show: \`You have unmerged paths\`" >> docs/troubleshooting.md
   echo "2. Files will contain conflict markers:" >> docs/troubleshooting.md
   echo "   \`\`\`" >> docs/troubleshooting.md
   echo "   <<<<<<< HEAD" >> docs/troubleshooting.md
   echo "   Your changes" >> docs/troubleshooting.md
   echo "   =======" >> docs/troubleshooting.md
   echo "   Their changes" >> docs/troubleshooting.md
   echo "   >>>>>>> branch-name" >> docs/troubleshooting.md
   echo "   \`\`\`" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### How to Resolve" >> docs/troubleshooting.md
   echo "1. **Open conflicted files** in your editor" >> docs/troubleshooting.md
   echo "2. **Find conflict markers** (<<<<<<<, =======, >>>>>>>)" >> docs/troubleshooting.md
   echo "3. **Choose which changes to keep**" >> docs/troubleshooting.md
   echo "4. **Remove conflict markers**" >> docs/troubleshooting.md
   echo "5. **Save the file**" >> docs/troubleshooting.md
   echo "6. **Stage the resolved file**: \`git add filename\`" >> docs/troubleshooting.md
   echo "7. **Complete the merge**: \`git commit\`" >> docs/troubleshooting.md
   echo "" >> docs/troubleshooting.md
   echo "### Prevention" >> docs/troubleshooting.md
   echo "- Pull frequently: \`git pull origin main\`" >> docs/troubleshooting.md
   echo "- Communicate with team about changes" >> docs/troubleshooting.md
   echo "- Use small, focused commits" >> docs/troubleshooting.md
   echo "- Test changes before pushing" >> docs/troubleshooting.md
   ```

3. **Commit the troubleshooting guide**:
   ```bash
   git add docs/troubleshooting.md
   git commit -m "Add troubleshooting guide for common Git issues"
   ```

## Exercise 5: Creating Templates

### Part A: Pull Request Template
1. **Create PR template**:
   ```bash
   echo "## Description" > templates/pull-request-template.md
   echo "Brief description of changes made." >> templates/pull-request-template.md
   echo "" >> templates/pull-request-template.md
   echo "## Type of Change" >> templates/pull-request-template.md
   echo "- [ ] Bug fix (non-breaking change which fixes an issue)" >> templates/pull-request-template.md
   echo "- [ ] New feature (non-breaking change which adds functionality)" >> templates/pull-request-template.md
   echo "- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)" >> templates/pull-request-template.md
   echo "- [ ] Documentation update" >> templates/pull-request-template.md
   echo "" >> templates/pull-request-template.md
   echo "## How to Test" >> templates/pull-request-template.md
   echo "1. Step 1" >> templates/pull-request-template.md
   echo "2. Step 2" >> templates/pull-request-template.md
   echo "3. Step 3" >> templates/pull-request-template.md
   echo "" >> templates/pull-request-template.md
   echo "## Checklist" >> templates/pull-request-template.md
   echo "- [ ] My code follows the team's coding standards" >> templates/pull-request-template.md
   echo "- [ ] I have performed a self-review of my own code" >> templates/pull-request-template.md
   echo "- [ ] I have commented my code, particularly in hard-to-understand areas" >> templates/pull-request-template.md
   echo "- [ ] I have made corresponding changes to the documentation" >> templates/pull-request-template.md
   echo "- [ ] My changes generate no new warnings" >> templates/pull-request-template.md
   echo "- [ ] I have added tests that prove my fix is effective or that my feature works" >> templates/pull-request-template.md
   echo "- [ ] New and existing unit tests pass locally with my changes" >> templates/pull-request-template.md
   echo "- [ ] Any dependent changes have been merged and published" >> templates/pull-request-template.md
   echo "" >> templates/pull-request-template.md
   echo "## Screenshots (if applicable)" >> templates/pull-request-template.md
   echo "Add screenshots to help explain your changes." >> templates/pull-request-template.md
   echo "" >> templates/pull-request-template.md
   echo "## Related Issues" >> templates/pull-request-template.md
   echo "Link to related issues or tickets." >> templates/pull-request-template.md
   ```

2. **Create commit message template**:
   ```bash
   echo "# <type>(<scope>): <subject>" > templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# <body>" >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# <footer>" >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# Types:" >> templates/commit-message-template.txt
   echo "# feat:     A new feature" >> templates/commit-message-template.txt
   echo "# fix:      A bug fix" >> templates/commit-message-template.txt
   echo "# docs:     Documentation only changes" >> templates/commit-message-template.txt
   echo "# style:    Changes that do not affect the meaning of the code" >> templates/commit-message-template.txt
   echo "# refactor: A code change that neither fixes a bug nor adds a feature" >> templates/commit-message-template.txt
   echo "# test:     Adding missing tests or correcting existing tests" >> templates/commit-message-template.txt
   echo "# chore:    Changes to the build process or auxiliary tools" >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# Scope:" >> templates/commit-message-template.txt
   echo "# The scope should be the name of the npm package affected" >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# Subject:" >> templates/commit-message-template.txt
   echo "# The subject contains a succinct description of the change:" >> templates/commit-message-template.txt
   echo "# - Use the imperative, present tense: \"change\" not \"changed\" nor \"changes\"" >> templates/commit-message-template.txt
   echo "# - Don't capitalize the first letter" >> templates/commit-message-template.txt
   echo "# - No dot (.) at the end" >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# Body:" >> templates/commit-message-template.txt
   echo "# Just as in the subject, use the imperative, present tense: \"change\" not \"changed\" nor \"changes\"" >> templates/commit-message-template.txt
   echo "# The body should include the motivation for the change and contrast this with previous behavior." >> templates/commit-message-template.txt
   echo "" >> templates/commit-message-template.txt
   echo "# Footer:" >> templates/commit-message-template.txt
   echo "# The footer should contain any information about Breaking Changes and is also the place to" >> templates/commit-message-template.txt
   echo "# reference GitHub issues that this commit Closes." >> templates/commit-message-template.txt
   ```

3. **Commit the templates**:
   ```bash
   git add templates/
   git commit -m "Add PR and commit message templates"
   ```

## Exercise 6: Creating Examples

### Part A: Workflow Examples
1. **Create example scenarios**:
   ```bash
   echo "# Workflow Examples" > examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "## Example 1: Adding a New Feature" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "### Scenario" >> examples/workflow-examples.md
   echo "You need to add user authentication to the application." >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "### Steps" >> examples/workflow-examples.md
   echo "1. **Start from main branch**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout main" >> examples/workflow-examples.md
   echo "   git pull origin main" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "2. **Create feature branch**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout -b feature/user-authentication" >> examples/workflow-examples.md
   echo "   git push -u origin feature/user-authentication" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "3. **Develop the feature**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   # Make changes to files" >> examples/workflow-examples.md
   echo "   git add ." >> examples/workflow-examples.md
   echo "   git commit -m \"feat(auth): add login form\"" >> examples/workflow-examples.md
   echo "   git push" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "4. **Create pull request**" >> examples/workflow-examples.md
   echo "   - Go to GitHub/GitLab" >> examples/workflow-examples.md
   echo "   - Click \"New Pull Request\"" >> examples/workflow-examples.md
   echo "   - Select branches and write description" >> examples/workflow-examples.md
   echo "   - Assign reviewers" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "5. **Address feedback**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   # Make requested changes" >> examples/workflow-examples.md
   echo "   git add ." >> examples/workflow-examples.md
   echo "   git commit -m \"fix(auth): address review feedback\"" >> examples/workflow-examples.md
   echo "   git push" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "6. **Merge and cleanup**" >> examples/workflow-examples.md
   echo "   - Merge PR after approval" >> examples/workflow-examples.md
   echo "   - Delete feature branch" >> examples/workflow-examples.md
   echo "   - Switch back to main" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout main" >> examples/workflow-examples.md
   echo "   git pull origin main" >> examples/workflow-examples.md
   echo "   git branch -d feature/user-authentication" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   ```

2. **Add hotfix example**:
   ```bash
   echo "" >> examples/workflow-examples.md
   echo "## Example 2: Critical Hotfix" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "### Scenario" >> examples/workflow-examples.md
   echo "Production is down due to a critical bug. You need to fix it immediately." >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "### Steps" >> examples/workflow-examples.md
   echo "1. **Start from main branch**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout main" >> examples/workflow-examples.md
   echo "   git pull origin main" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "2. **Create hotfix branch**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout -b hotfix/critical-database-connection" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "3. **Make the fix**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   # Fix the critical issue" >> examples/workflow-examples.md
   echo "   git add ." >> examples/workflow-examples.md
   echo "   git commit -m \"fix(db): resolve critical connection issue\"" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "4. **Deploy immediately**" >> examples/workflow-examples.md
   echo "   - Merge directly to main" >> examples/workflow-examples.md
   echo "   - Deploy to production" >> examples/workflow-examples.md
   echo "   - Monitor for issues" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "5. **Merge back to develop**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git checkout develop" >> examples/workflow-examples.md
   echo "   git merge hotfix/critical-database-connection" >> examples/workflow-examples.md
   echo "   git push origin develop" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   echo "" >> examples/workflow-examples.md
   echo "6. **Cleanup**" >> examples/workflow-examples.md
   echo "   \`\`\`bash" >> examples/workflow-examples.md
   echo "   git branch -d hotfix/critical-database-connection" >> examples/workflow-examples.md
   echo "   \`\`\`" >> examples/workflow-examples.md
   ```

3. **Commit the examples**:
   ```bash
   git add examples/
   git commit -m "Add workflow examples for common scenarios"
   ```

## Exercise 7: Finalizing Documentation

### Part A: Create Index and Navigation
1. **Create main index**:
   ```bash
   echo "# Team Git Documentation" > README.md
   echo "" >> README.md
   echo "Welcome to our team's Git workflow documentation. This repository contains all the information you need to work effectively with Git in our team." >> README.md
   echo "" >> README.md
   echo "## Quick Start" >> README.md
   echo "" >> README.md
   echo "1. **New to Git?** Start with [Git Commands Cheat Sheet](docs/git-cheat-sheet.md)" >> README.md
   echo "2. **Need to start a feature?** Follow our [Workflow Steps](docs/workflow-steps.md)" >> README.md
   echo "3. **Having issues?** Check our [Troubleshooting Guide](docs/troubleshooting.md)" >> README.md
   echo "4. **Need examples?** See our [Workflow Examples](examples/workflow-examples.md)" >> README.md
   echo "" >> README.md
   echo "## Documentation Structure" >> README.md
   echo "" >> README.md
   echo "### Core Documentation" >> README.md
   echo "- [Workflow Diagram](docs/workflow-diagram.md) - Visual overview of our process" >> README.md
   echo "- [Workflow Steps](docs/workflow-steps.md) - Detailed step-by-step instructions" >> README.md
   echo "- [Team Conventions](docs/team-conventions.md) - Naming and commit standards" >> README.md
   echo "- [Git Commands Cheat Sheet](docs/git-cheat-sheet.md) - Quick reference for commands" >> README.md
   echo "- [Troubleshooting Guide](docs/troubleshooting.md) - Solutions to common problems" >> README.md
   echo "" >> README.md
   echo "### Templates" >> README.md
   echo "- [Pull Request Template](templates/pull-request-template.md) - Standard PR format" >> README.md
   echo "- [Commit Message Template](templates/commit-message-template.txt) - Standard commit format" >> README.md
   echo "" >> README.md
   echo "### Examples" >> README.md
   echo "- [Workflow Examples](examples/workflow-examples.md) - Real-world scenarios" >> README.md
   echo "" >> README.md
   echo "## Getting Help" >> README.md
   echo "" >> README.md
   echo "If you can't find what you're looking for:" >> README.md
   echo "1. Check the [Troubleshooting Guide](docs/troubleshooting.md)" >> README.md
   echo "2. Ask in the #git-help channel" >> README.md
   echo "3. Contact the team lead" >> README.md
   echo "4. Create an issue in this repository" >> README.md
   echo "" >> README.md
   echo "## Contributing" >> README.md
   echo "" >> README.md
   echo "This documentation is a living resource. If you find errors or have suggestions:" >> README.md
   echo "1. Create a pull request with your changes" >> README.md
   echo "2. Follow our [Team Conventions](docs/team-conventions.md)" >> README.md
   echo "3. Request review from the documentation team" >> README.md
   ```

2. **Create a simple navigation file**:
   ```bash
   echo "# Navigation" > docs/navigation.md
   echo "" >> docs/navigation.md
   echo "## Getting Started" >> docs/navigation.md
   echo "- [Workflow Overview](workflow-diagram.md)" >> docs/navigation.md
   echo "- [Step-by-Step Guide](workflow-steps.md)" >> docs/navigation.md
   echo "- [Quick Reference](git-cheat-sheet.md)" >> docs/navigation.md
   echo "" >> docs/navigation.md
   echo "## Team Standards" >> docs/navigation.md
   echo "- [Naming Conventions](team-conventions.md)" >> docs/navigation.md
   echo "- [Commit Standards](team-conventions.md#commit-messages)" >> docs/navigation.md
   echo "- [Branch Standards](team-conventions.md#branch-naming)" >> docs/navigation.md
   echo "" >> docs/navigation.md
   echo "## Problem Solving" >> docs/navigation.md
   echo "- [Troubleshooting Guide](troubleshooting.md)" >> docs/navigation.md
   echo "- [Common Issues](troubleshooting.md#common-git-issues)" >> docs/navigation.md
   echo "- [Merge Conflicts](troubleshooting.md#merge-conflicts)" >> docs/navigation.md
   echo "" >> docs/navigation.md
   echo "## Templates" >> docs/navigation.md
   echo "- [Pull Request Template](../templates/pull-request-template.md)" >> docs/navigation.md
   echo "- [Commit Message Template](../templates/commit-message-template.txt)" >> docs/navigation.md
   echo "" >> docs/navigation.md
   echo "## Examples" >> docs/navigation.md
   echo "- [Workflow Examples](../examples/workflow-examples.md)" >> docs/navigation.md
   echo "- [Feature Development](../examples/workflow-examples.md#example-1-adding-a-new-feature)" >> docs/navigation.md
   echo "- [Hotfix Process](../examples/workflow-examples.md#example-2-critical-hotfix)" >> docs/navigation.md
   ```

3. **Commit the final documentation**:
   ```bash
   git add README.md docs/navigation.md
   git commit -m "Add main index and navigation for documentation"
   ```

## Exercise 8: Testing Your Documentation

### Part A: Self-Test
1. **Test the workflow**:
   - Follow your own workflow steps
   - Try the examples
   - Check if all commands work

2. **Test the troubleshooting guide**:
   - Create some common issues
   - Follow the solutions
   - Verify they work

3. **Test the cheat sheet**:
   - Try the commands
   - Check if they're accurate
   - Add any missing commands

### Part B: Team Review
1. **Share with team members**:
   - Ask for feedback
   - Test with different experience levels
   - Gather suggestions for improvement

2. **Update based on feedback**:
   - Fix any errors
   - Add missing information
   - Improve clarity

3. **Finalize documentation**:
   - Make final edits
   - Ensure consistency
   - Test all links and commands

## Discussion Questions

1. **What makes documentation effective?**
   - How do you balance detail with simplicity?
   - What visual elements help most?

2. **How do you keep documentation current?**
   - What processes help maintain accuracy?
   - How do you handle outdated information?

3. **What role should AI play in documentation?**
   - Can AI help create better documentation?
   - How do you balance AI assistance with human expertise?

4. **How do you measure documentation success?**
   - What metrics indicate good documentation?
   - How do you know if it's helping the team?

5. **What's the best way to onboard new team members?**
   - How does documentation help with training?
   - What additional support is needed?

## Key Takeaways

- **Documentation enables consistency**: Everyone follows the same process
- **Start simple**: Begin with basic workflows and expand
- **Make it visual**: Diagrams and examples help understanding
- **Keep it current**: Update documentation when processes change
- **Test it**: Have others follow your documentation
- **Use AI as a starting point**: But always customize for your team
- **Regular maintenance**: Documentation is a living resource
- **Team ownership**: Everyone should contribute to and maintain docs




