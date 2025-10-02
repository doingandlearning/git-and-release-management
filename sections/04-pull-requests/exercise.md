# Exercise 4: Pull Requests and Code Review

## Setup
For this exercise, we'll simulate a team environment with multiple developers. We'll use GitHub CLI or create a local repository that simulates the PR workflow.

### Option A: Using GitHub CLI (if available)
```bash
# Check if GitHub CLI is installed
gh --version

# If not installed, you can still follow along with the concepts
```

### Option B: Local Simulation
```bash
# Create a team repository
mkdir team-project
cd team-project
git init
git config user.name "Team Lead"
git config user.email "team@example.com"

# Create initial project
echo "# Team Project" > README.md
echo "A collaborative project for learning pull requests." >> README.md
mkdir src tests
echo "def main():" > src/app.py
echo "    print('Hello, Team!')" >> src/app.py
echo "" >> src/app.py
echo "if __name__ == '__main__':" >> src/app.py
echo "    main()" >> src/app.py

git add .
git commit -m "Initial project setup"
```

## Exercise 1: Creating Your First Pull Request

### Part A: Set Up Feature Branch
1. **Create a feature branch**:
   ```bash
   git checkout -b feature/add-calculator
   ```

2. **Implement calculator functionality**:
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

3. **Add tests for calculator**:
   ```bash
   echo "import pytest" > tests/test_calculator.py
   echo "from src.calculator import add, subtract, multiply, divide" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_add():" >> tests/test_calculator.py
   echo "    assert add(2, 3) == 5" >> tests/test_calculator.py
   echo "    assert add(-1, 1) == 0" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_subtract():" >> tests/test_calculator.py
   echo "    assert subtract(5, 3) == 2" >> tests/test_calculator.py
   echo "    assert subtract(1, 1) == 0" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_multiply():" >> tests/test_calculator.py
   echo "    assert multiply(2, 3) == 6" >> tests/test_calculator.py
   echo "    assert multiply(-2, 3) == -6" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_divide():" >> tests/test_calculator.py
   echo "    assert divide(6, 2) == 3" >> tests/test_calculator.py
   echo "    assert divide(5, 2) == 2.5" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_divide_by_zero():" >> tests/test_calculator.py
   echo "    with pytest.raises(ValueError):" >> tests/test_calculator.py
   echo "        divide(5, 0)" >> tests/test_calculator.py
   ```

4. **Update main app to use calculator**:
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

5. **Commit your changes**:
   ```bash
   git add .
   git commit -m "Add calculator module with basic operations

   - Implement add, subtract, multiply, divide functions
   - Add comprehensive test coverage
   - Update main app to demonstrate calculator functionality
   - Handle division by zero with appropriate error"
   ```

### Part B: Create Pull Request Description
Write a PR description following this template:

```markdown
## What Changed
Added a calculator module with basic mathematical operations (add, subtract, multiply, divide) and integrated it into the main application.

## Why
The team needs basic mathematical functionality for future features. This provides a foundation for more complex calculations.

## How to Test
1. Run the main application: `python src/app.py`
2. Run the tests: `python -m pytest tests/test_calculator.py`
3. Verify all operations work correctly
4. Test error handling by trying to divide by zero

## Screenshots/Examples
```
Calculator Demo
2 + 3 = 5
10 - 4 = 6
3 * 4 = 12
15 / 3 = 5.0
```

## Related Issues
None - this is a new feature.

## Checklist
- [x] Code follows team standards
- [x] Tests added/updated
- [x] Documentation updated (README)
- [x] No breaking changes
- [x] Error handling implemented
```

## Exercise 2: Simulating Code Review

### Part A: Review Your Own Code
Before submitting a PR, review your own code:

1. **Check code quality**:
   - Are variable names descriptive?
   - Is the code readable and well-structured?
   - Are there any obvious bugs or issues?

2. **Check test coverage**:
   - Do tests cover all functions?
   - Are edge cases tested?
   - Are error conditions tested?

3. **Check documentation**:
   - Is the code self-documenting?
   - Are there any confusing parts that need comments?

### Part B: Simulate Reviewer Feedback
Imagine you're reviewing this PR. What feedback would you give?

**Example feedback you might give**:
- "Consider adding type hints to make the function signatures clearer"
- "The error message could be more descriptive"
- "Should we add more edge case tests for floating point arithmetic?"
- "Great job on the test coverage!"

### Part C: Address Feedback
Based on the feedback, make improvements:

1. **Add type hints**:
   ```bash
   echo "from typing import Union" > src/calculator.py
   echo "" >> src/calculator.py
   echo "def add(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    return a + b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def subtract(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    return a - b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def multiply(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    return a * b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def divide(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    if b == 0:" >> src/calculator.py
   echo "        raise ValueError('Cannot divide by zero - division by zero is undefined')" >> src/calculator.py
   echo "    return a / b" >> src/calculator.py
   ```

2. **Add more edge case tests**:
   ```bash
   echo "def test_float_arithmetic():" >> tests/test_calculator.py
   echo "    assert add(0.1, 0.2) == pytest.approx(0.3)" >> tests/test_calculator.py
   echo "    assert multiply(0.1, 3) == pytest.approx(0.3)" >> tests/test_calculator.py
   echo "" >> tests/test_calculator.py
   echo "def test_negative_numbers():" >> tests/test_calculator.py
   echo "    assert add(-5, 3) == -2" >> tests/test_calculator.py
   echo "    assert multiply(-2, -3) == 6" >> tests/test_calculator.py
   echo "    assert divide(-6, 2) == -3" >> tests/test_calculator.py
   ```

3. **Commit the improvements**:
   ```bash
   git add .
   git commit -m "Address review feedback

   - Add type hints to all functions
   - Improve error message clarity
   - Add edge case tests for floating point arithmetic
   - Add tests for negative number operations"
   ```

## Exercise 3: Handling Multiple PRs

### Part A: Create Another Feature Branch
1. **Switch to main and create another feature**:
   ```bash
   git checkout main
   git checkout -b feature/add-logging
   ```

2. **Implement logging functionality**:
   ```bash
   echo "import logging" > src/logger.py
   echo "import sys" >> src/logger.py
   echo "" >> src/logger.py
   echo "def setup_logging(level=logging.INFO):" >> src/logger.py
   echo "    logging.basicConfig(" >> src/logger.py
   echo "        level=level," >> src/logger.py
   echo "        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'," >> src/logger.py
   echo "        handlers=[" >> src/logger.py
   echo "            logging.StreamHandler(sys.stdout)," >> src/logger.py
   echo "            logging.FileHandler('app.log')" >> src/logger.py
   echo "        ]" >> src/logger.py
   echo "    )" >> src/logger.py
   echo "    return logging.getLogger(__name__)" >> src/logger.py
   ```

3. **Update main app to use logging**:
   ```bash
   echo "from src.calculator import add, subtract, multiply, divide" > src/app.py
   echo "from src.logger import setup_logging" >> src/app.py
   echo "" >> src/app.py
   echo "def main():" >> src/app.py
   echo "    logger = setup_logging()" >> src/app.py
   echo "    logger.info('Starting calculator demo')" >> src/app.py
   echo "    " >> src/app.py
   echo "    print('Calculator Demo')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = add(2, 3)" >> src/app.py
   echo "    print(f'2 + 3 = {result}')" >> src/app.py
   echo "    logger.info(f'Addition: 2 + 3 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = subtract(10, 4)" >> src/app.py
   echo "    print(f'10 - 4 = {result}')" >> src/app.py
   echo "    logger.info(f'Subtraction: 10 - 4 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = multiply(3, 4)" >> src/app.py
   echo "    print(f'3 * 4 = {result}')" >> src/app.py
   echo "    logger.info(f'Multiplication: 3 * 4 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = divide(15, 3)" >> src/app.py
   echo "    print(f'15 / 3 = {result}')" >> src/app.py
   echo "    logger.info(f'Division: 15 / 3 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    logger.info('Calculator demo completed')" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    main()" >> src/app.py
   ```

4. **Add tests for logging**:
   ```bash
   echo "import pytest" > tests/test_logger.py
   echo "import logging" >> tests/test_logger.py
   echo "from src.logger import setup_logging" >> tests/test_logger.py
   echo "" >> tests/test_logger.py
   echo "def test_setup_logging():" >> tests/test_logger.py
   echo "    logger = setup_logging()" >> tests/test_logger.py
   echo "    assert isinstance(logger, logging.Logger)" >> tests/test_logger.py
   echo "    assert logger.level == logging.INFO" >> tests/test_logger.py
   echo "" >> tests/test_logger.py
   echo "def test_logger_handlers():" >> tests/test_logger.py
   echo "    logger = setup_logging()" >> tests/test_logger.py
   echo "    assert len(logger.handlers) == 2" >> tests/test_logger.py
   ```

5. **Commit the logging feature**:
   ```bash
   git add .
   git commit -m "Add logging functionality

   - Implement structured logging with file and console output
   - Add logging to calculator operations
   - Include comprehensive tests for logging module
   - Update main app to demonstrate logging"
   ```

### Part B: Simulate PR Conflicts
1. **Switch back to calculator branch**:
   ```bash
   git checkout feature/add-calculator
   ```

2. **Make a change that might conflict**:
   ```bash
   echo "from src.calculator import add, subtract, multiply, divide" > src/app.py
   echo "from src.logger import setup_logging" >> src/app.py
   echo "" >> src/app.py
   echo "def main():" >> src/app.py
   echo "    logger = setup_logging()" >> src/app.py
   echo "    logger.info('Starting calculator demo')" >> src/app.py
   echo "    " >> src/app.py
   echo "    print('Calculator Demo')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = add(2, 3)" >> src/app.py
   echo "    print(f'2 + 3 = {result}')" >> src/app.py
   echo "    logger.info(f'Addition: 2 + 3 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = subtract(10, 4)" >> src/app.py
   echo "    print(f'10 - 4 = {result}')" >> src/app.py
   echo "    logger.info(f'Subtraction: 10 - 4 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = multiply(3, 4)" >> src/app.py
   echo "    print(f'3 * 4 = {result}')" >> src/app.py
   echo "    logger.info(f'Multiplication: 3 * 4 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    result = divide(15, 3)" >> src/app.py
   echo "    print(f'15 / 3 = {result}')" >> src/app.py
   echo "    logger.info(f'Division: 15 / 3 = {result}')" >> src/app.py
   echo "    " >> src/app.py
   echo "    logger.info('Calculator demo completed')" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    main()" >> src/app.py
   ```

3. **Commit the integration**:
   ```bash
   git add .
   git commit -m "Integrate logging with calculator demo

   - Add logging to calculator operations
   - Demonstrate integration between modules
   - Maintain existing functionality while adding logging"
   ```

## Exercise 4: PR Review Simulation

### Part A: Review the Calculator PR
Imagine you're reviewing the calculator PR. Write down:

1. **What you like about the code**:
   - Clear function names
   - Good test coverage
   - Proper error handling
   - Type hints

2. **What could be improved**:
   - Consider adding more edge cases
   - Maybe add input validation
   - Consider adding more mathematical operations

3. **Questions you would ask**:
   - "Should we add more mathematical operations like power or square root?"
   - "What about handling very large numbers?"
   - "Should we add a command-line interface?"

### Part B: Review the Logging PR
Review the logging PR:

1. **What you like**:
   - Good separation of concerns
   - Comprehensive logging setup
   - Tests for the logging module

2. **What could be improved**:
   - Maybe add log rotation
   - Consider different log levels for different operations
   - Add configuration options

3. **Questions you would ask**:
   - "Should we make the log level configurable?"
   - "What about log rotation for the file handler?"
   - "Should we add structured logging with JSON format?"

## Exercise 5: Merging PRs

### Part A: Merge Calculator PR
1. **Switch to main**:
   ```bash
   git checkout main
   ```

2. **Merge calculator feature**:
   ```bash
   git merge feature/add-calculator
   ```

3. **Test the merged code**:
   ```bash
   python src/app.py
   python -m pytest tests/test_calculator.py
   ```

### Part B: Merge Logging PR
1. **Merge logging feature**:
   ```bash
   git merge feature/add-logging
   ```

2. **Test the integrated code**:
   ```bash
   python src/app.py
   python -m pytest tests/
   ```

3. **Check the log file**:
   ```bash
   cat app.log
   ```

### Part C: Clean Up Branches
1. **Delete feature branches**:
   ```bash
   git branch -d feature/add-calculator
   git branch -d feature/add-logging
   ```

2. **Check final state**:
   ```bash
   git log --oneline
   git branch
   ```

## Exercise 6: Advanced PR Techniques

### Part A: Interactive Rebase
1. **Create a messy commit history**:
   ```bash
   git checkout -b feature/messy-commits
   echo "def function1():" > src/messy.py
   echo "    pass" >> src/messy.py
   git add src/messy.py
   git commit -m "Add function1"
   
   echo "def function2():" >> src/messy.py
   echo "    pass" >> src/messy.py
   git add src/messy.py
   git commit -m "Add function2"
   
   echo "def function3():" >> src/messy.py
   echo "    pass" >> src/messy.py
   git add src/messy.py
   git commit -m "Add function3"
   ```

2. **Clean up with interactive rebase**:
   ```bash
   git rebase -i HEAD~3
   # Choose: pick, reword, edit, squash, drop
   # Try squashing the last two commits into one
   ```

### Part B: Cherry-picking
1. **Create a hotfix branch**:
   ```bash
   git checkout main
   git checkout -b hotfix/fix-calculator
   ```

2. **Make a quick fix**:
   ```bash
   echo "def add(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" > src/calculator.py
   echo "    \"\"\"Add two numbers.\"\"\"" >> src/calculator.py
   echo "    return a + b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def subtract(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    \"\"\"Subtract b from a.\"\"\"" >> src/calculator.py
   echo "    return a - b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def multiply(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    \"\"\"Multiply two numbers.\"\"\"" >> src/calculator.py
   echo "    return a * b" >> src/calculator.py
   echo "" >> src/calculator.py
   echo "def divide(a: Union[int, float], b: Union[int, float]) -> Union[int, float]:" >> src/calculator.py
   echo "    \"\"\"Divide a by b.\"\"\"" >> src/calculator.py
   echo "    if b == 0:" >> src/calculator.py
   echo "        raise ValueError('Cannot divide by zero - division by zero is undefined')" >> src/calculator.py
   echo "    return a / b" >> src/calculator.py
   
   git add src/calculator.py
   git commit -m "Add docstrings to calculator functions"
   ```

3. **Cherry-pick to main**:
   ```bash
   git checkout main
   git cherry-pick hotfix/fix-calculator
   ```

## Discussion Questions

1. **What makes a good PR description?**
   - How detailed should it be?
   - What information is most important?

2. **How do you handle conflicting feedback from multiple reviewers?**
   - What if reviewers disagree?
   - How do you prioritize feedback?

3. **When should you reject a PR?**
   - What are the criteria for rejection?
   - How do you communicate rejection constructively?

4. **How do you handle PRs that are too large?**
   - What's the maximum size for a PR?
   - How do you break down large changes?

5. **What role should AI play in code review?**
   - Can AI replace human reviewers?
   - How can AI assist reviewers?

## Key Takeaways

- **PRs enable collaboration**: Formal process for code review and approval
- **Good descriptions help reviewers**: Clear context makes reviews more effective
- **Review is a skill**: Practice giving and receiving constructive feedback
- **Small PRs are better**: Easier to review and less likely to have issues
- **Communication is key**: Be responsive and constructive in reviews
- **Process matters**: Consistent workflow helps the whole team
- **AI can assist**: But human judgment is still essential for code review




