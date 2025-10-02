# Exercise 2: Everyday Git Essentials

## Setup
For this exercise, we'll work with a simulated team project. You can either:
1. Use the existing `git-safety-exercise` directory, or
2. Create a new directory: `mkdir git-daily-exercise && cd git-daily-exercise && git init`

## Exercise 1: Basic Repository Operations

### Part A: Repository Setup
1. **Initialize a new repository** (if not using existing one):
   ```bash
   git init
   ```

2. **Configure Git** (if not already done):
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

3. **Check your configuration**:
   ```bash
   git config --list
   ```

4. **Create a project structure**:
   ```bash
   mkdir src tests docs
   echo "# My Project" > README.md
   echo "print('Hello, World!')" > src/main.py
   echo "def test_main():" > tests/test_main.py
   echo "    assert True" >> tests/test_main.py
   ```

### Part B: First Commit
1. **Check the status**:
   ```bash
   git status
   ```
   **What do you see?** All files should be untracked

2. **Add files to staging**:
   ```bash
   git add README.md src/main.py tests/test_main.py
   ```

3. **Check status again**:
   ```bash
   git status
   ```
   **What changed?**

4. **Make your first commit**:
   ```bash
   git commit -m "Initial project setup with basic structure"
   ```

5. **Check the commit history**:
   ```bash
   git log --oneline
   ```

## Exercise 2: Daily Workflow Simulation

### Part A: Making Changes
1. **Edit the main.py file**:
   ```bash
   echo "def greet(name):" >> src/main.py
   echo "    return f'Hello, {name}!'" >> src/main.py
   echo "" >> src/main.py
   echo "if __name__ == '__main__':" >> src/main.py
   echo "    print(greet('World'))" >> src/main.py
   ```

2. **Check what changed**:
   ```bash
   git status
   git diff
   ```

3. **Stage and commit the changes**:
   ```bash
   git add src/main.py
   git commit -m "Add greet function to main.py"
   ```

### Part B: Adding Documentation
1. **Create a documentation file**:
   ```bash
   echo "# API Documentation" > docs/api.md
   echo "" >> docs/api.md
   echo "## Functions" >> docs/api.md
   echo "" >> docs/api.md
   echo "### greet(name)" >> docs/api.md
   echo "Returns a greeting message for the given name." >> docs/api.md
   ```

2. **Check status and add documentation**:
   ```bash
   git status
   git add docs/api.md
   git commit -m "Add API documentation"
   ```

### Part C: Updating Tests
1. **Update the test file**:
   ```bash
   echo "from src.main import greet" > tests/test_main.py
   echo "" >> tests/test_main.py
   echo "def test_greet():" >> tests/test_main.py
   echo "    assert greet('Alice') == 'Hello, Alice!'" >> tests/test_main.py
   echo "    assert greet('Bob') == 'Hello, Bob!'" >> tests/test_main.py
   ```

2. **Stage and commit the test updates**:
   ```bash
   git add tests/test_main.py
   git commit -m "Update tests for greet function"
   ```


## Exercise 3: Viewing History and Changes

### Part A: Exploring Git Log
1. **View commit history**:
   ```bash
   git log --oneline
   ```

2. **View detailed commit history**:
   ```bash
   git log
   ```

3. **View commit history with changes**:
   ```bash
   git log -p
   ```

4. **View commit history in graph format**:
   ```bash
   git log --graph --oneline --all
   ```

### Part B: Using Git Diff
1. **Make a change to a file**:
   ```bash
   echo "# Updated Project" > README.md
   echo "This project now includes input validation." >> README.md
   ```

2. **Check the difference**:
   ```bash
   git diff README.md
   ```

3. **Stage the change**:
   ```bash
   git add README.md
   ```

4. **Check staged changes**:
   ```bash
   git diff --staged README.md
   ```

5. **Commit the change**:
   ```bash
   git commit -m "Update README with validation info"
   ```

## Exercise 4: Simulating Team Collaboration

### Part A: Simulating Remote Repository
1. **Create a "remote" repository** (simulate with another directory):
   ```bash
   cd ..
   git clone --bare git-daily-exercise git-daily-exercise-remote.git
   cd git-daily-exercise
   ```

2. **Add the remote**:
   ```bash
   git remote add origin ../git-daily-exercise-remote.git
   ```

3. **Push your branches**:
   ```bash
   git push -u origin main
   git push -u origin feature/add-validation
   ```

### Part B: Simulating Another Developer's Work
1. **Create a new directory for another developer**:
   ```bash
   cd ..
   git clone git-daily-exercise-remote.git developer2-work
   cd developer2-work
   ```

2. **Make changes as "developer 2"**:
   ```bash
   echo "def goodbye(name):" >> src/main.py
   echo "    return f'Goodbye, {name}!'" >> src/main.py
   git add src/main.py
   git commit -m "Add goodbye function"
   git push origin main
   ```

3. **Switch back to your original directory**:
   ```bash
   cd ../git-daily-exercise
   ```

4. **Pull the changes**:
   ```bash
   git pull origin main
   ```

5. **Check what changed**:
   ```bash
   git log --oneline
   cat src/main.py
   ```

## Exercise 5: Troubleshooting Practice

### Part A: Common Issues
1. **Try to push without pulling first** (simulate conflict):
   ```bash
   # Make a change
   echo "Local change" > local-change.txt
   git add local-change.txt
   git commit -m "Local change"
   
   # Try to push (this might work or show an error)
   git push origin main
   ```

2. **If you get an error about being behind**:
   ```bash
   git pull origin main
   git push origin main
   ```

### Part B: Checking Repository Status
1. **Check current status**:
   ```bash
   git status
   ```

2. **Check remote status**:
   ```bash
   git remote -v
   ```

3. **Check branch information**:
   ```bash
   git branch -a
   ```

## Discussion Questions

1. **What's the difference between `git add .` and `git add filename`?**
   - When would you use each one?

2. **Why is it important to write good commit messages?**
   - What makes a commit message "good"?

3. **What's the difference between `git pull` and `git fetch`?**
   - When would you use each one?

4. **Why do we use branches for features?**
   - What problems does this solve?

5. **When might you ask an AI assistant for help with Git commands?**
   - What information would you need to provide?

## Key Takeaways

- **Daily workflow**: status → add → commit → push
- **Good commit messages help everyone**: Be descriptive and consistent
- **Pull before push**: Always sync with remote before pushing
- **Git history is auditable**: Every change is tracked and recoverable
- **Practice builds confidence**: Use these commands regularly to build muscle memory

