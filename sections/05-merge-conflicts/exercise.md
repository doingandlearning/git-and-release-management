# Exercise 5: Dealing with Merge Conflicts

## Setup
For this exercise, we'll create a scenario where merge conflicts are inevitable. This will help you practice resolving them in a controlled environment.

```bash
# Create a new repository for conflict practice
mkdir merge-conflict-exercise
cd merge-conflict-exercise
git init
git config user.name "Developer A"
git config user.email "dev-a@example.com"

# Create initial project
echo "# Merge Conflict Practice" > README.md
echo "This project helps you practice resolving merge conflicts." >> README.md
mkdir src tests
echo "def main():" > src/app.py
echo "    print('Hello, World!')" >> src/app.py
echo "" >> src/app.py
echo "if __name__ == '__main__':" >> src/app.py
echo "    main()" >> src/app.py

git add .
git commit -m "Initial project setup"
```

## Exercise 1: Creating Your First Merge Conflict

### Part A: Set Up the Conflict Scenario
1. **Create a feature branch**:
   ```bash
   git checkout -b feature/add-calculator
   ```

2. **Add calculator functionality**:
   ```bash
   echo "def add(a, b):" > src/calculator.py
   echo "    return a + b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def subtract(a, b):" >> src/calculator.py
   echo "    return a - b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def multiply(a, b):" >> src/calculator.py
   echo "    return a * b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def divide(a, b):" >> src/calculator.py
   echo "    if b == 0:" >> src/calculator.py
   echo "        raise ValueError('Cannot divide by zero')" >> src/calculator.py
   echo "    return a / b" >> src/calculator.py
   ```

3. **Update main app to use calculator**:
   ```bash
   echo "from src.calculator import add, subtract, multiply, divide" > src/app.py
   echo "" >> src/app.py
   echo "def main():" >> src/app.py
   echo "    print('Calculator Demo')" >> src/app.py
   echo "    print(f'2 + 3 = {add(2, 3)}')" >> src/app.py
   echo "    print(f'10 - 4 = {subtract(10, 4)}')" >> src/app.py
   echo "    print(f'3 * 4 = {multiply(3, 4)}')" >> src/app.py
   echo "    print(f'15 / 3 = {divide(15, 3)}')" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    main()" >> src/app.py
   ```

4. **Commit your changes**:
   ```bash
   git add .
   git commit -m "Add calculator functionality"
   ```

### Part B: Simulate Another Developer's Work
1. **Switch to main and create another branch**:
   ```bash
   git checkout main
   git checkout -b feature/add-logging
   ```

2. **Add logging functionality**:
   ```bash
   echo "import logging" > src/logger.py
   echo "import sys" >> src/logger.py
   echo "" >> src/logger.py
   echo "def setup_logging():" >> src/logger.py
   echo "    logging.basicConfig(" >> src/logger.py
   echo "        level=logging.INFO," >> src/logger.py
   echo "        format='%(asctime)s - %(levelname)s - %(message)s'," >> src/logger.py
   echo "        handlers=[logging.StreamHandler(sys.stdout)]" >> src/logger.py
   echo "    )" >> src/logger.py
   echo "    return logging.getLogger(__name__)" >> src/logger.py
   ```

3. **Update main app to use logging**:
   ```bash
   echo "import logging" > src/app.py
   echo "from src.logger import setup_logging" >> src/app.py
   echo "" >> src/app.py
   echo "def main():" >> src/app.py
   echo "    logger = setup_logging()" >> src/app.py
   echo "    logger.info('Starting application')" >> src/app.py
   echo "    print('Hello, World!')" >> src/app.py
   echo "    logger.info('Application completed')" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    main()" >> src/app.py
   ```

4. **Commit the logging changes**:
   ```bash
   git add .
   git commit -m "Add logging functionality"
   ```

### Part C: Create the Conflict
1. **Switch to main and merge the logging feature**:
   ```bash
   git checkout main
   git merge feature/add-logging
   ```

2. **Now try to merge the calculator feature**:
   ```bash
   git merge feature/add-calculator
   ```

3. **You should see a merge conflict!** Check the status:
   ```bash
   git status
   ```

4. **Look at the conflicted file**:
   ```bash
   cat src/app.py
   ```

## Exercise 2: Resolving Your First Conflict

### Part A: Understand the Conflict
1. **Examine the conflict markers**:
   ```bash
   cat src/app.py
   ```

2. **You should see something like**:
   ```python
   <<<<<<< HEAD
   import logging
   from src.logger import setup_logging
   
   def main():
       logger = setup_logging()
       logger.info('Starting application')
       print('Hello, World!')
       logger.info('Application completed')
   =======
   from src.calculator import add, subtract, multiply, divide
   
   def main():
       print('Calculator Demo')
       print(f'2 + 3 = {add(2, 3)}')
       print(f'10 - 4 = {subtract(10, 4)}')
       print(f'3 * 4 = {multiply(3, 4)}')
       print(f'15 / 3 = {divide(15, 3)}')
   >>>>>>> feature/add-calculator
   
   if __name__ == '__main__':
       main()
   ```

3. **Understand what each section represents**:
   - **HEAD section**: The logging version (current branch)
   - **Feature section**: The calculator version (incoming branch)

### Part B: Resolve the Conflict
1. **Edit the file to combine both features**:
   ```bash
   # Open in your preferred editor
   code src/app.py
   # or
   vim src/app.py
   ```

2. **Create a combined version**:
   ```python
   import logging
   from src.logger import setup_logging
   from src.calculator import add, subtract, multiply, divide
   
   def main():
       logger = setup_logging()
       logger.info('Starting calculator demo')
       
       print('Calculator Demo')
       result = add(2, 3)
       print(f'2 + 3 = {result}')
       logger.info(f'Addition: 2 + 3 = {result}')
       
       result = subtract(10, 4)
       print(f'10 - 4 = {result}')
       logger.info(f'Subtraction: 10 - 4 = {result}')
       
       result = multiply(3, 4)
       print(f'3 * 4 = {result}')
       logger.info(f'Multiplication: 3 * 4 = {result}')
       
       result = divide(15, 3)
       print(f'15 / 3 = {result}')
       logger.info(f'Division: 15 / 3 = {result}')
       
       logger.info('Calculator demo completed')
   
   if __name__ == '__main__':
       main()
   ```

3. **Remove the conflict markers** (make sure they're completely gone)

4. **Save the file**

### Part C: Complete the Merge
1. **Stage the resolved file**:
   ```bash
   git add src/app.py
   ```

2. **Check the status**:
   ```bash
   git status
   ```

3. **Complete the merge**:
   ```bash
   git commit -m "Merge calculator feature with logging

   - Combine calculator functionality with logging
   - Add logging to all calculator operations
   - Resolve merge conflict in src/app.py"
   ```

4. **Test the merged code**:
   ```bash
   python src/app.py
   ```

## Exercise 3: Different Types of Conflicts

### Part A: Content Conflict
1. **Create a new branch**:
   ```bash
   git checkout -b feature/update-calculator
   ```

2. **Modify the calculator function**:
   ```bash
   echo "def add(a, b):" > src/calculator.py
   echo "    \"\"\"Add two numbers.\"\"\"" >> src/calculator.py
   echo "    return a + b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def subtract(a, b):" >> src/calculator.py
   echo "    \"\"\"Subtract b from a.\"\"\"" >> src/calculator.py
   echo "    return a - b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def multiply(a, b):" >> src/calculator.py
   echo "    \"\"\"Multiply two numbers.\"\"\"" >> src/calculator.py
   echo "    return a * b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def divide(a, b):" >> src/calculator.py
   echo "    \"\"\"Divide a by b.\"\"\"" >> src/calculator.py
   echo "    if b == 0:" >> src/calculator.py
   echo "        raise ValueError('Cannot divide by zero')" >> src/calculator.py
   echo "    return a / b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def power(a, b):" >> src/calculator.py
   echo "    \"\"\"Raise a to the power of b.\"\"\"" >> src/calculator.py
   echo "    return a ** b" >> src/calculator.py
   ```

3. **Commit the changes**:
   ```bash
   git add src/calculator.py
   git commit -m "Add docstrings and power function to calculator"
   ```

4. **Switch to main and make conflicting changes**:
   ```bash
   git checkout main
   echo "def add(a, b):" > src/calculator.py
   echo "    # Add two numbers" >> src/calculator.py
   echo "    return a + b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def subtract(a, b):" >> src/calculator.py
   echo "    # Subtract b from a" >> src/calculator.py
   echo "    return a - b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def multiply(a, b):" >> src/calculator.py
   echo "    # Multiply two numbers" >> src/calculator.py
   echo "    return a * b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def divide(a, b):" >> src/calculator.py
   echo "    # Divide a by b" >> src/calculator.py
   echo "    if b == 0:" >> src/calculator.py
   echo "        raise ValueError('Cannot divide by zero')" >> src/calculator.py
   echo "    return a / b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def square(a):" >> src/calculator.py
   echo "    # Square a number" >> src/calculator.py
   echo "    return a * a" >> src/calculator.py
   ```

5. **Commit the main changes**:
   ```bash
   git add src/calculator.py
   git commit -m "Add comments and square function to calculator"
   ```

6. **Try to merge the feature branch**:
   ```bash
   git merge feature/update-calculator
   ```

7. **Resolve the conflict** by combining both approaches:
   - Keep the docstrings from the feature branch
   - Keep both the power and square functions
   - Choose the best documentation style

### Part B: File Deletion Conflict
1. **Create a branch that deletes a file**:
   ```bash
   git checkout -b feature/remove-logger
   git rm src/logger.py
   git commit -m "Remove logger module"
   ```

2. **Switch to main and modify the same file**:
   ```bash
   git checkout main
   echo "import logging" > src/logger.py
   echo "import sys" >> src/logger.py
   echo "" >> src/logger.py
   echo "def setup_logging():" >> src/logger.py
   echo "    logging.basicConfig(" >> src/logger.py
   echo "        level=logging.INFO," >> src/logger.py
   echo "        format='%(asctime)s - %(levelname)s - %(message)s'," >> src/logger.py
   echo "        handlers=[logging.StreamHandler(sys.stdout)]" >> src/logger.py
   echo "    )" >> src/logger.py
   echo "    return logging.getLogger(__name__)" >> src/logger.py
   echo "" >> src/logger.py
   echo "def log_calculation(operation, a, b, result):" >> src/logger.py
   echo "    logger = logging.getLogger(__name__)" >> src/logger.py
   echo "    logger.info(f'{operation}: {a} and {b} = {result}')" >> src/logger.py
   ```

3. **Commit the changes**:
   ```bash
   git add src/logger.py
   git commit -m "Enhance logger with calculation logging"
   ```

4. **Try to merge the deletion branch**:
   ```bash
   git merge feature/remove-logger
   ```

5. **Resolve the conflict** by choosing to keep the file:
   ```bash
   git add src/logger.py
   git commit -m "Keep enhanced logger module"
   ```

## Exercise 4: Using Merge Tools

### Part A: Configure a Merge Tool
1. **Configure VS Code as merge tool** (if available):
   ```bash
   git config --global merge.tool vscode
   git config --global mergetool.vscode.cmd 'code --wait $MERGED'
   ```

2. **Or configure vimdiff**:
   ```bash
   git config --global merge.tool vimdiff
   ```

3. **Create another conflict**:
   ```bash
   git checkout -b feature/conflict-tool-test
   echo "def main():" > src/app.py
   echo "    print('Feature branch version')" >> src/app.py
   echo "    print('This is different from main')" >> src/app.py
   git add src/app.py
   git commit -m "Update app.py in feature branch"
   
   git checkout main
   echo "def main():" > src/app.py
   echo "    print('Main branch version')" >> src/app.py
   echo "    print('This is also different')" >> src/app.py
   git add src/app.py
   git commit -m "Update app.py in main branch"
   ```

4. **Use the merge tool**:
   ```bash
   git merge feature/conflict-tool-test
   git mergetool
   ```

5. **Resolve the conflict using the tool**

### Part B: Manual Resolution Without Tools
1. **Create another conflict**:
   ```bash
   git checkout -b feature/manual-resolution
   echo "def calculate_tax(amount):" > src/tax.py
   echo "    return amount * 0.1" >> src/tax.py
   git add src/tax.py
   git commit -m "Add basic tax calculation"
   
   git checkout main
   echo "def calculate_tax(amount):" > src/tax.py
   echo "    if amount > 1000:" >> src/tax.py
   echo "        return amount * 0.15" >> src/tax.py
   echo "    return amount * 0.1" >> src/tax.py
   git add src/tax.py
   git commit -m "Add progressive tax calculation"
   ```

2. **Merge and resolve manually**:
   ```bash
   git merge feature/manual-resolution
   # Edit src/tax.py to combine both approaches
   # Choose the progressive tax approach
   git add src/tax.py
   git commit -m "Use progressive tax calculation"
   ```

## Exercise 5: Advanced Conflict Resolution

### Part A: Using Git Rebase
1. **Create a feature branch**:
   ```bash
   git checkout -b feature/rebase-conflict
   echo "def process_data(data):" > src/processor.py
   echo "    return data.upper()" >> src/processor.py
   git add src/processor.py
   git commit -m "Add data processor"
   ```

2. **Make changes to main**:
   ```bash
   git checkout main
   echo "def process_data(input_data):" > src/processor.py
   echo "    return input_data.upper()" >> src/processor.py
   git add src/processor.py
   git commit -m "Add data processor with different parameter name"
   ```

3. **Rebase the feature branch**:
   ```bash
   git checkout feature/rebase-conflict
   git rebase main
   ```

4. **Resolve the conflict during rebase**:
   - Choose the better parameter name
   - Complete the rebase

### Part B: Aborting Operations
1. **Create a complex conflict**:
   ```bash
   git checkout -b feature/abort-test
   echo "def complex_function():" > src/complex.py
   echo "    # This is a complex function" >> src/complex.py
   echo "    pass" >> src/complex.py
   git add src/complex.py
   git commit -m "Add complex function"
   
   git checkout main
   echo "def complex_function():" > src/complex.py
   echo "    # This is a different complex function" >> src/complex.py
   echo "    pass" >> src/complex.py
   git add src/complex.py
   git commit -m "Add different complex function"
   ```

2. **Start a merge and abort it**:
   ```bash
   git merge feature/abort-test
   # You'll see conflicts
   git merge --abort
   ```

3. **Start a rebase and abort it**:
   ```bash
   git checkout feature/abort-test
   git rebase main
   # You'll see conflicts
   git rebase --abort
   ```

## Exercise 6: Conflict Prevention Strategies

### Part A: Communication
1. **Create a team coordination file**:
   ```bash
   echo "# Team Coordination" > TEAM.md
   echo "" >> TEAM.md
   echo "## Current Work" >> TEAM.md
   echo "- Developer A: Working on calculator module" >> TEAM.md
   echo "- Developer B: Working on logging module" >> TEAM.md
   echo "- Developer C: Working on tax calculations" >> TEAM.md
   echo "" >> TEAM.md
   echo "## File Ownership" >> TEAM.md
   echo "- src/calculator.py: Developer A" >> TEAM.md
   echo "- src/logger.py: Developer B" >> TEAM.md
   echo "- src/tax.py: Developer C" >> TEAM.md
   ```

2. **Commit the coordination file**:
   ```bash
   git add TEAM.md
   git commit -m "Add team coordination file"
   ```

### Part B: Modular Design
1. **Create separate modules to avoid conflicts**:
   ```bash
   mkdir src/modules
   echo "def utility_function():" > src/modules/utils.py
   echo "    pass" >> src/modules/utils.py
   echo "" >> src/modules/utils.py
   echo "def helper_function():" > src/modules/helpers.py
   echo "    pass" >> src/modules/helpers.py
   ```

2. **Commit the modular structure**:
   ```bash
   git add src/modules/
   git commit -m "Add modular structure to avoid conflicts"
   ```

### Part C: Regular Updates
1. **Create a script to pull latest changes**:
   ```bash
   echo "#!/bin/bash" > update.sh
   echo "echo 'Updating from main...'" >> update.sh
   echo "git fetch origin" >> update.sh
   echo "git rebase origin/main" >> update.sh
   echo "echo 'Update complete!'" >> update.sh
   chmod +x update.sh
   ```

2. **Commit the update script**:
   ```bash
   git add update.sh
   git commit -m "Add update script for regular syncing"
   ```

## Discussion Questions

1. **What's the best strategy for resolving conflicts?**
   - When should you choose one version over another?
   - When should you combine both versions?

2. **How can you prevent conflicts before they happen?**
   - What communication strategies work best?
   - How can code organization help?

3. **What tools do you find most helpful for conflict resolution?**
   - Do you prefer merge tools or manual resolution?
   - What about your IDE's conflict resolution features?

4. **How do you handle conflicts in a team environment?**
   - When should you ask for help?
   - How do you communicate about conflicts?

5. **What's the difference between merge and rebase for conflicts?**
   - When would you use each approach?
   - What are the trade-offs?

## Key Takeaways

- **Conflicts are normal**: Part of collaborative development
- **Understand before resolving**: Read both versions carefully
- **Test your resolution**: Ensure code still works
- **Use tools when helpful**: Merge tools can make resolution easier
- **Prevention is better**: Good communication and organization help
- **Practice makes perfect**: The more you resolve, the better you get
- **Don't panic**: Conflicts can always be resolved
- **Ask for help**: When in doubt, consult your team




