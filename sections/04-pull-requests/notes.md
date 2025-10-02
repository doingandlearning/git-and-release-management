# Section 4: Pull Requests and Code Review

## Learning Objectives
- Understand how pull requests formalize review and approval
- Practice raising and reviewing pull requests in a real repository
- Make the process auditable and provide quality checks before release

## Key Talking Points

### 1. What Are Pull Requests?

#### Definition
- **Pull Request (PR)**: A request to merge changes from one branch into another
- **Code Review**: The process of examining code changes before they're merged
- **Audit Trail**: A record of who approved what changes and when

#### Why Pull Requests Matter
- **Quality Control**: Catch bugs and issues before they reach production
- **Knowledge Sharing**: Team members learn from each other's code
- **Consistency**: Ensure code follows team standards and patterns
- **Documentation**: PR descriptions document what changed and why
- **Collaboration**: Enable discussion and iteration on changes

### 2. The Pull Request Workflow

#### Creating a Pull Request
1. **Complete your feature**:
   ```bash
   git checkout feature/my-feature
   # ... make changes ...
   git add .
   git commit -m "Implement user authentication"
   git push origin feature/my-feature
   ```

2. **Create PR via GitHub/GitLab UI**:
   - Navigate to repository
   - Click "New Pull Request" or "Create Merge Request"
   - Select source and target branches
   - Write descriptive title and description
   - Assign reviewers
   - Submit

3. **Wait for review and address feedback**:
   - Respond to comments
   - Make requested changes
   - Push updates to the same branch

4. **Merge after approval**:
   - Reviewer approves
   - All checks pass
   - Merge via UI or command line

#### PR Best Practices
- **Small, focused PRs**: One feature or fix per PR
- **Clear descriptions**: What changed, why, and how to test
- **Link to issues**: Reference related tickets or bugs
- **Request specific reviewers**: Tag relevant team members
- **Keep PRs up to date**: Rebase on target branch regularly

### 3. Writing Effective PR Descriptions

#### PR Title Guidelines
- **Be descriptive**: "Add user authentication with OAuth2" not "Update code"
- **Use present tense**: "Add feature" not "Added feature"
- **Keep it concise**: Under 50 characters when possible
- **Include issue numbers**: "Add user authentication (fixes #123)"

#### PR Description Template
```markdown
## What Changed
Brief description of the changes made.

## Why
Explanation of why these changes were necessary.

## How to Test
Steps to verify the changes work correctly.

## Screenshots/Examples
Visual evidence of the changes (if applicable).

## Related Issues
Links to related tickets, bugs, or discussions.

## Checklist
- [ ] Code follows team standards
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No breaking changes
```

### 4. Code Review Process

#### For Reviewers
**What to Look For**:
- **Correctness**: Does the code work as intended?
- **Readability**: Is the code easy to understand?
- **Performance**: Are there any performance issues?
- **Security**: Are there any security vulnerabilities?
- **Testing**: Are there adequate tests?
- **Documentation**: Is the code well-documented?

**How to Review**:
- **Read the PR description first**: Understand the context
- **Look at the diff**: Focus on what actually changed
- **Test the changes**: If possible, run the code locally
- **Be constructive**: Suggest improvements, don't just criticize
- **Ask questions**: If something isn't clear, ask for clarification

#### For Authors
**Preparing for Review**:
- **Self-review first**: Look at your own changes critically
- **Write clear commit messages**: Help reviewers understand your changes
- **Add tests**: Ensure your changes are well-tested
- **Update documentation**: Keep docs in sync with code changes
- **Be responsive**: Address feedback promptly and professionally

### 5. Review Comments and Feedback

#### Types of Comments
- **Questions**: "Why did you choose this approach?"
- **Suggestions**: "Consider using a more descriptive variable name"
- **Issues**: "This might cause a memory leak"
- **Praise**: "Great solution! This is much cleaner"

#### Comment Best Practices
- **Be specific**: Point to exact lines or sections
- **Be constructive**: Suggest improvements, don't just criticize
- **Be respectful**: Remember there's a person behind the code
- **Be timely**: Don't let PRs sit for days without review
- **Be thorough**: Don't just skim, actually understand the changes

### 6. Handling Review Feedback

#### Responding to Comments
- **Acknowledge feedback**: "Good point, I'll fix that"
- **Ask for clarification**: "Could you explain what you mean by...?"
- **Discuss alternatives**: "I considered that approach, but..."
- **Show appreciation**: "Thanks for the suggestion!"

#### Making Changes
- **Address all feedback**: Don't ignore comments
- **Make incremental commits**: Show progress on feedback
- **Test your changes**: Ensure fixes don't break anything
- **Update the PR description**: If the scope changes significantly

### 7. PR Status and Checks

#### Common PR States
- **Draft**: Work in progress, not ready for review
- **Open**: Ready for review, waiting for approval
- **Approved**: Ready to merge, all checks passed
- **Merged**: Successfully integrated into target branch
- **Closed**: Rejected or abandoned

#### Automated Checks
- **CI/CD Pipeline**: Automated tests, builds, and deployments
- **Code Quality**: Linting, formatting, and static analysis
- **Security Scanning**: Vulnerability detection
- **Coverage Reports**: Test coverage analysis
- **Performance Tests**: Automated performance validation

### 8. Merge Strategies

#### Merge Options
- **Merge Commit**: Creates a merge commit, preserves branch history
- **Squash and Merge**: Combines all commits into one, clean history
- **Rebase and Merge**: Replays commits on target branch, linear history

#### Choosing Merge Strategy
- **Merge Commit**: When you want to preserve branch context
- **Squash and Merge**: When you want clean, linear history
- **Rebase and Merge**: When you want linear history but keep individual commits

### 9. Branch Protection Rules

#### Common Protection Rules
- **Require PR**: No direct pushes to protected branches
- **Require Reviews**: At least one approval before merging
- **Require Status Checks**: CI/CD must pass before merging
- **Require Up-to-Date Branches**: Must be current with target branch
- **Restrict Push Access**: Only certain users can push to protected branches

#### Setting Up Protection
```bash
# Via GitHub CLI (example)
gh api repos/:owner/:repo/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":["ci"]}' \
  --field enforce_admins=true \
  --field required_pull_request_reviews='{"required_approving_review_count":1}' \
  --field restrictions=null
```

### 10. Advanced PR Techniques

#### Keeping PRs Up to Date
```bash
# Update feature branch with latest main
git checkout feature/my-feature
git fetch origin
git rebase origin/main
git push --force-with-lease origin feature/my-feature
```

#### Interactive Rebase for Clean History
```bash
# Clean up commit history before PR
git rebase -i HEAD~3  # Interactive rebase last 3 commits
# Choose: pick, reword, edit, squash, drop
```

#### Cherry-picking Changes
```bash
# Apply specific commits to another branch
git checkout target-branch
git cherry-pick <commit-hash>
```

### 11. PR Templates and Automation

#### PR Templates
Create `.github/pull_request_template.md`:
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Tests pass locally
- [ ] New tests added
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
```

#### Automated PR Actions
- **Auto-assign reviewers**: Based on file changes
- **Auto-label PRs**: Based on content or size
- **Auto-close issues**: When PR is merged
- **Auto-deploy**: To staging environment

### 12. Troubleshooting Common PR Issues

#### "This branch is out of date"
```bash
# Update with latest changes
git checkout feature-branch
git fetch origin
git rebase origin/main
git push --force-with-lease origin feature-branch
```

#### "Merge conflicts"
```bash
# Resolve conflicts locally
git checkout feature-branch
git fetch origin
git rebase origin/main
# Resolve conflicts in files
git add resolved-files
git rebase --continue
git push --force-with-lease origin feature-branch
```

#### "Required status checks are pending"
- Wait for CI/CD to complete
- Check if any checks failed
- Fix issues and push updates
- Re-run failed checks if possible

### 13. AI Assistant Integration

#### When to Ask AI for Help
- **PR descriptions**: "Help me write a clear PR description for this change"
- **Review feedback**: "How should I respond to this review comment?"
- **Merge conflicts**: "I have merge conflicts, how do I resolve them?"
- **Git commands**: "How do I update my PR with latest changes?"

#### Example Prompts
- "Help me write a PR description for adding user authentication"
- "I got feedback to 'use more descriptive variable names' - what does this mean?"
- "My PR is failing CI checks, how do I debug this?"
- "How do I squash commits in my PR to clean up the history?"

### 14. Team Collaboration Best Practices

#### Communication
- **Be responsive**: Respond to PRs within 24 hours
- **Be constructive**: Focus on improving the code, not criticizing the person
- **Be specific**: Point to exact issues and suggest solutions
- **Be patient**: Remember that everyone is learning

#### Process
- **Small PRs**: Easier to review and less likely to have conflicts
- **Clear descriptions**: Help reviewers understand the context
- **Regular updates**: Keep PRs current with target branch
- **Quick merges**: Don't let approved PRs sit for days

#### Tools
- **IDE integrations**: Use GitHub/GitLab extensions in your editor
- **CLI tools**: Use GitHub CLI or GitLab CLI for common operations
- **Notifications**: Set up alerts for PRs assigned to you
- **Dashboards**: Use project boards to track PR status

## Key Takeaways

1. **Pull requests enable collaboration**: Formal process for code review and approval
2. **Good descriptions matter**: Help reviewers understand your changes
3. **Review is a skill**: Practice giving and receiving constructive feedback
4. **Small PRs are better**: Easier to review and less likely to have issues
5. **Automation helps**: Use CI/CD and other tools to catch issues early
6. **Communication is key**: Be responsive and constructive in reviews
7. **AI can help with process**: But human judgment is still essential for code review

