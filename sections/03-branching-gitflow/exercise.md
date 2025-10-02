# Exercise 3: Branching and GitFlow

## Setup
For this exercise, we'll simulate a team project with multiple developers. Create a new repository:

```bash
mkdir gitflow-exercise
cd gitflow-exercise
git init
git config user.name "Team Lead"
git config user.email "team@example.com"
```

## Exercise 1: Setting Up GitFlow Structure

### Part A: Initial Repository Setup
1. **Create initial project structure**:
   ```bash
   echo "# My Application" > README.md
   echo "Version: 1.0.0" >> README.md
   echo "" >> README.md
   echo "A sample application for GitFlow demonstration." >> README.md
   
   mkdir src tests
   echo "def main():" > src/app.py
   echo "    print('Hello, World!')" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    main()" >> src/app.py
   
   echo "def test_main():" > tests/test_app.py
   echo "    assert True" >> tests/test_app.py
   ```

2. **Create initial commit on main**:
   ```bash
   git add .
   git commit -m "Initial project setup"
   ```

3. **Create develop branch**:
   ```bash
   git checkout -b develop
   git checkout main
   ```

4. **Set up remote repository** (simulate with bare repo):
   ```bash
   cd ..
   git clone --bare gitflow-exercise gitflow-exercise-remote.git
   cd gitflow-exercise
   git remote add origin ../gitflow-exercise-remote.git
   git push -u origin main
   git push -u origin develop
   ```

### Part B: Verify Branch Structure
1. **Check all branches**:
   ```bash
   git branch -a
   ```

2. **Check branch relationships**:
   ```bash
   git log --graph --oneline --all
   ```

## Exercise 2: Feature Development Workflow

### Part A: Developer 1 - User Authentication Feature
1. **Switch to develop and create feature branch**:
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/user-authentication
   ```

2. **Implement authentication feature**:
   ```bash
   echo "def authenticate(username, password):" > src/auth.py
   echo "    # Simple authentication logic" >> src/auth.py
   echo "    return username == 'admin' and password == 'password'" >> src/auth.py
   echo "" >> src/auth.py
   echo "def login(username, password):" >> src/auth.py
   echo "    if authenticate(username, password):" >> src/auth.py
   echo "        return f'Welcome, {username}!'" >> src/auth.py
   echo "    else:" >> src/auth.py
   echo "        return 'Invalid credentials'" >> src/auth.py
   ```

3. **Add tests for authentication**:
   ```bash
   echo "from src.auth import authenticate, login" > tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_authenticate_valid_credentials():" >> tests/test_auth.py
   echo "    assert authenticate('admin', 'password') == True" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_authenticate_invalid_credentials():" >> tests/test_auth.py
   echo "    assert authenticate('admin', 'wrong') == False" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_login_success():" >> tests/test_auth.py
   echo "    assert login('admin', 'password') == 'Welcome, admin!'" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_login_failure():" >> tests/test_auth.py
   echo "    assert login('admin', 'wrong') == 'Invalid credentials'" >> tests/test_auth.py
   ```

4. **Commit the feature**:
   ```bash
   git add .
   git commit -m "Add user authentication feature with tests"
   ```

5. **Push feature branch**:
   ```bash
   git push -u origin feature/user-authentication
   ```

### Part B: Developer 2 - Data Storage Feature
1. **Switch to develop and create another feature branch**:
   ```bash
   git checkout develop
   git checkout -b feature/data-storage
   ```

2. **Implement data storage feature**:
   ```bash
   echo "import json" > src/storage.py
   echo "" >> src/storage.py
   echo "def save_data(data, filename):" >> src/storage.py
   echo "    with open(filename, 'w') as f:" >> src/storage.py
   echo "        json.dump(data, f)" >> src/storage.py
   echo "" >> src/storage.py
   echo "def load_data(filename):" >> src/storage.py
   echo "    try:" >> src/storage.py
   echo "        with open(filename, 'w') as f:" >> src/storage.py
   echo "            return json.load(f)" >> src/storage.py
   echo "    except FileNotFoundError:" >> src/storage.py
   echo "        return {}" >> src/storage.py
   ```

3. **Add tests for storage**:
   ```bash
   echo "from src.storage import save_data, load_data" > tests/test_storage.py
   echo "import tempfile" >> tests/test_storage.py
   echo "import os" >> tests/test_storage.py
   echo "" >> tests/test_storage.py
   echo "def test_save_and_load_data():" >> tests/test_storage.py
   echo "    test_data = {'key': 'value', 'number': 42}" >> tests/test_storage.py
   echo "    with tempfile.NamedTemporaryFile(mode='w', delete=False) as f:" >> tests/test_storage.py
   echo "        temp_file = f.name" >> tests/test_storage.py
   echo "    try:" >> tests/test_storage.py
   echo "        save_data(test_data, temp_file)" >> tests/test_storage.py
   echo "        loaded_data = load_data(temp_file)" >> tests/test_storage.py
   echo "        assert loaded_data == test_data" >> tests/test_storage.py
   echo "    finally:" >> tests/test_storage.py
   echo "        os.unlink(temp_file)" >> tests/test_storage.py
   ```

4. **Commit the feature**:
   ```bash
   git add .
   git commit -m "Add data storage functionality with JSON support"
   ```

5. **Push feature branch**:
   ```bash
   git push -u origin feature/data-storage
   ```

## Exercise 3: Merging Features

### Part A: Merge First Feature
1. **Switch to develop**:
   ```bash
   git checkout develop
   ```

2. **Merge user authentication feature**:
   ```bash
   git merge feature/user-authentication
   ```

3. **Push updated develop**:
   ```bash
   git push origin develop
   ```

4. **Delete feature branch**:
   ```bash
   git branch -d feature/user-authentication
   git push origin --delete feature/user-authentication
   ```

### Part B: Merge Second Feature
1. **Merge data storage feature**:
   ```bash
   git merge feature/data-storage
   ```

2. **Push updated develop**:
   ```bash
   git push origin develop
   ```

3. **Delete feature branch**:
   ```bash
   git branch -d feature/data-storage
   git push origin --delete feature/data-storage
   ```

### Part C: Verify Merged Features
1. **Check the current state**:
   ```bash
   git log --graph --oneline --all
   ls -la src/
   ls -la tests/
   ```

2. **Test the integrated features**:
   ```bash
   python3 -c "from src.auth import login; print(login('admin', 'password'))"
   python3 -c "from src.storage import save_data; save_data({'test': 'data'}, 'test.json')"
   ```

## Exercise 4: Release Management

### Part A: Create Release Branch
1. **Create release branch from develop**:
   ```bash
   git checkout develop
   git checkout -b release/1.1.0
   ```

2. **Update version information**:
   ```bash
   echo "# My Application" > README.md
   echo "Version: 1.1.0" >> README.md
   echo "" >> README.md
   echo "A sample application for GitFlow demonstration." >> README.md
   echo "" >> README.md
   echo "## Features" >> README.md
   echo "- User authentication" >> README.md
   echo "- Data storage with JSON support" >> README.md
   ```

3. **Create changelog**:
   ```bash
   echo "# Changelog" > CHANGELOG.md
   echo "" >> CHANGELOG.md
   echo "## [1.1.0] - $(date +%Y-%m-%d)" >> CHANGELOG.md
   echo "" >> CHANGELOG.md
   echo "### Added" >> CHANGELOG.md
   echo "- User authentication system" >> CHANGELOG.md
   echo "- Data storage functionality" >> CHANGELOG.md
   echo "- Comprehensive test coverage" >> CHANGELOG.md
   ```

4. **Commit release preparations**:
   ```bash
   git add .
   git commit -m "Prepare release 1.1.0"
   ```

5. **Push release branch**:
   ```bash
   git push -u origin release/1.1.0
   ```

### Part B: Complete Release
1. **Merge to main**:
   ```bash
   git checkout main
   git merge release/1.1.0
   ```

2. **Tag the release**:
   ```bash
   git tag -a v1.1.0 -m "Release version 1.1.0"
   git push origin v1.1.0
   ```

3. **Merge back to develop**:
   ```bash
   git checkout develop
   git merge release/1.1.0
   git push origin develop
   ```

4. **Delete release branch**:
   ```bash
   git branch -d release/1.1.0
   git push origin --delete release/1.1.0
   ```

## Exercise 5: Hotfix Workflow

### Part A: Create Hotfix
1. **Create hotfix branch from main**:
   ```bash
   git checkout main
   git checkout -b hotfix/security-patch
   ```

2. **Implement security fix**:
   ```bash
   echo "import hashlib" > src/auth.py
   echo "" >> src/auth.py
   echo "def hash_password(password):" >> src/auth.py
   echo "    return hashlib.sha256(password.encode()).hexdigest()" >> src/auth.py
   echo "" >> src/auth.py
   echo "def authenticate(username, password):" >> src/auth.py
   echo "    # More secure authentication with hashed passwords" >> src/auth.py
   echo "    stored_password = hash_password('password')" >> src/auth.py
   echo "    return username == 'admin' and hash_password(password) == stored_password" >> src/auth.py
   echo "" >> src/auth.py
   echo "def login(username, password):" >> src/auth.py
   echo "    if authenticate(username, password):" >> src/auth.py
   echo "        return f'Welcome, {username}!'" >> src/auth.py
   echo "    else:" >> src/auth.py
   echo "        return 'Invalid credentials'" >> src/auth.py
   ```

3. **Update tests for security fix**:
   ```bash
   echo "from src.auth import authenticate, login, hash_password" > tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_hash_password():" >> tests/test_auth.py
   echo "    hash1 = hash_password('password')" >> tests/test_auth.py
   echo "    hash2 = hash_password('password')" >> tests/test_auth.py
   echo "    assert hash1 == hash2" >> tests/test_auth.py
   echo "    assert hash1 != 'password'" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_authenticate_valid_credentials():" >> tests/test_auth.py
   echo "    assert authenticate('admin', 'password') == True" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_authenticate_invalid_credentials():" >> tests/test_auth.py
   echo "    assert authenticate('admin', 'wrong') == False" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_login_success():" >> tests/test_auth.py
   echo "    assert login('admin', 'password') == 'Welcome, admin!'" >> tests/test_auth.py
   echo "" >> tests/test_auth.py
   echo "def test_login_failure():" >> tests/test_auth.py
   echo "    assert login('admin', 'wrong') == 'Invalid credentials'" >> tests/test_auth.py
   ```

4. **Commit the hotfix**:
   ```bash
   git add .
   git commit -m "Fix security vulnerability: hash passwords"
   ```

5. **Push hotfix branch**:
   ```bash
   git push -u origin hotfix/security-patch
   ```

### Part B: Complete Hotfix
1. **Merge to main**:
   ```bash
   git checkout main
   git merge hotfix/security-patch
   ```

2. **Tag the hotfix**:
   ```bash
   git tag -a v1.1.1 -m "Hotfix: Security patch for password hashing"
   git push origin v1.1.1
   ```

3. **Merge back to develop**:
   ```bash
   git checkout develop
   git merge hotfix/security-patch
   git push origin develop
   ```

4. **Delete hotfix branch**:
   ```bash
   git branch -d hotfix/security-patch
   git push origin --delete hotfix/security-patch
   ```

## Exercise 6: Branch Management

### Part A: Viewing Branch Information
1. **List all branches**:
   ```bash
   git branch -a
   ```

2. **Show branch relationships**:
   ```bash
   git log --graph --oneline --all
   ```

3. **Show tags**:
   ```bash
   git tag
   ```

4. **Show tag details**:
   ```bash
   git show v1.1.0
   git show v1.1.1
   ```

### Part B: Branch Cleanup
1. **Check merged branches**:
   ```bash
   git branch --merged
   ```

2. **Check unmerged branches**:
   ```bash
   git branch --no-merged
   ```

3. **Clean up remote references**:
   ```bash
   git remote prune origin
   ```

## Exercise 7: Simulating Team Conflicts

### Part A: Create Conflicting Changes
1. **Create a new feature branch**:
   ```bash
   git checkout develop
   git checkout -b feature/conflicting-feature
   ```

2. **Make changes to a file**:
   ```bash
   echo "def new_function():" >> src/app.py
   echo "    print('This is a new function')" >> src/app.py
   git add src/app.py
   git commit -m "Add new function to app.py"
   ```

3. **Switch to develop and make conflicting changes**:
   ```bash
   git checkout develop
   echo "def another_function():" >> src/app.py
   echo "    print('This is another function')" >> src/app.py
   git add src/app.py
   git commit -m "Add another function to app.py"
   git push origin develop
   ```

4. **Try to merge the feature branch**:
   ```bash
   git merge feature/conflicting-feature
   ```

5. **Resolve the conflict** (if any):
   ```bash
   # Edit src/app.py to resolve conflicts
   # Then:
   git add src/app.py
   git commit -m "Resolve merge conflict in app.py"
   ```

## Discussion Questions

1. **What are the benefits of using GitFlow?**
   - How does it help with team collaboration?
   - What problems does it solve?

2. **When would you use a hotfix vs. a regular feature branch?**
   - What are the trade-offs?

3. **Why merge hotfixes back to develop?**
   - What happens if you don't?

4. **What's the difference between tags and branches?**
   - When would you use each one?

5. **How does branching help with parallel development?**
   - What challenges does it solve?

## Key Takeaways

- **GitFlow provides structure**: Clear roles for different branch types
- **Feature branches enable parallel work**: Multiple developers can work simultaneously
- **Releases are managed systematically**: Separate branches for preparation
- **Hotfixes provide emergency fixes**: Direct path from main to production
- **Tags mark important versions**: Easy to identify and deploy specific releases
- **Branch cleanup is important**: Keep repository organized and clean
- **Conflicts are manageable**: Git provides tools to resolve them




