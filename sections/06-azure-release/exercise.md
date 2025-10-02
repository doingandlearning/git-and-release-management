# Exercise 6: Release Process in Azure

## Setup
For this exercise, we'll simulate an Azure DevOps environment and create a Python application that can be deployed to Azure.

```bash
# Create a new repository for Azure deployment
mkdir azure-deployment-exercise
cd azure-deployment-exercise
git init
git config user.name "Azure Developer"
git config user.email "azure-dev@example.com"

# Create initial project structure
echo "# Azure Deployment Exercise" > README.md
echo "A Python application for learning Azure deployment." >> README.md
mkdir src tests azure-pipelines
```

## Exercise 1: Creating a Python Application for Azure

### Part A: Build the Application
1. **Create a Flask web application**:
   ```bash
   echo "from flask import Flask, jsonify, request" > src/app.py
   echo "import os" >> src/app.py
   echo "import logging" >> src/app.py
   echo "" >> src/app.py
   echo "app = Flask(__name__)" >> src/app.py
   echo "" >> src/app.py
   echo "# Configure logging" >> src/app.py
   echo "logging.basicConfig(level=logging.INFO)" >> src/app.py
   echo "logger = logging.getLogger(__name__)" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/')" >> src/app.py
   echo "def home():" >> src/app.py
   echo "    return jsonify({" >> src/app.py
   echo "        'message': 'Hello from Azure!'," >> src/app.py
   echo "        'environment': os.getenv('ENVIRONMENT', 'development')," >> src/app.py
   echo "        'version': os.getenv('VERSION', '1.0.0')" >> src/app.py
   echo "    })" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/health')" >> src/app.py
   echo "def health():" >> src/app.py
   echo "    return jsonify({'status': 'healthy'})" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/api/calculate', methods=['POST'])" >> src/app.py
   echo "def calculate():" >> src/app.py
   echo "    data = request.get_json()" >> src/app.py
   echo "    if not data or 'operation' not in data:" >> src/app.py
   echo "        return jsonify({'error': 'Invalid input'}), 400" >> src/app.py
   echo "    " >> src/app.py
   echo "    operation = data['operation']" >> src/app.py
   echo "    a = data.get('a', 0)" >> src/app.py
   echo "    b = data.get('b', 0)" >> src/app.py
   echo "    " >> src/app.py
   echo "    try:" >> src/app.py
   echo "        if operation == 'add':" >> src/app.py
   echo "            result = a + b" >> src/app.py
   echo "        elif operation == 'subtract':" >> src/app.py
   echo "            result = a - b" >> src/app.py
   echo "        elif operation == 'multiply':" >> src/app.py
   echo "            result = a * b" >> src/app.py
   echo "        elif operation == 'divide':" >> src/app.py
   echo "            if b == 0:" >> src/app.py
   echo "                return jsonify({'error': 'Cannot divide by zero'}), 400" >> src/app.py
   echo "            result = a / b" >> src/app.py
   echo "        else:" >> src/app.py
   echo "            return jsonify({'error': 'Invalid operation'}), 400" >> src/app.py
   echo "        " >> src/app.py
   echo "        logger.info(f'Calculation: {a} {operation} {b} = {result}')" >> src/app.py
   echo "        return jsonify({'result': result})" >> src/app.py
   echo "    except Exception as e:" >> src/app.py
   echo "        logger.error(f'Calculation error: {str(e)}')" >> src/app.py
   echo "        return jsonify({'error': 'Calculation failed'}), 500" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    port = int(os.getenv('PORT', 5000))" >> src/app.py
   echo "    app.run(host='0.0.0.0', port=port, debug=False)" >> src/app.py
   ```

2. **Create requirements.txt**:
   ```bash
   echo "Flask==2.3.3" > requirements.txt
   echo "gunicorn==21.2.0" >> requirements.txt
   echo "python-dotenv==1.0.0" >> requirements.txt
   ```

3. **Create tests**:
   ```bash
   echo "import pytest" > tests/test_app.py
   echo "import json" >> tests/test_app.py
   echo "from src.app import app" >> tests/test_app.py
   echo "" >> tests/test_app.py
   echo "@pytest.fixture" >> tests/test_app.py
   echo "def client():" >> tests/test_app.py
   echo "    app.config['TESTING'] = True" >> tests/test_app.py
   echo "    with app.test_client() as client:" >> tests/test_app.py
   echo "        yield client" >> tests/test_app.py
   echo "" >> tests/test_app.py
   echo "def test_home(client):" >> tests/test_app.py
   echo "    response = client.get('/')" >> tests/test_app.py
   echo "    assert response.status_code == 200" >> tests/test_app.py
   echo "    data = json.loads(response.data)" >> tests/test_app.py
   echo "    assert 'message' in data" >> tests/test_app.py
   echo "    assert data['message'] == 'Hello from Azure!'" >> tests/test_app.py
   echo "" >> tests/test_app.py
   echo "def test_health(client):" >> tests/test_app.py
   echo "    response = client.get('/health')" >> tests/test_app.py
   echo "    assert response.status_code == 200" >> tests/test_app.py
   echo "    data = json.loads(response.data)" >> tests/test_app.py
   echo "    assert data['status'] == 'healthy'" >> tests/test_app.py
   echo "" >> tests/test_app.py
   echo "def test_calculate_add(client):" >> tests/test_app.py
   echo "    response = client.post('/api/calculate'," >> tests/test_app.py
   echo "                         json={'operation': 'add', 'a': 2, 'b': 3})" >> tests/test_app.py
   echo "    assert response.status_code == 200" >> tests/test_app.py
   echo "    data = json.loads(response.data)" >> tests/test_app.py
   echo "    assert data['result'] == 5" >> tests/test_app.py
   echo "" >> tests/test_app.py
   echo "def test_calculate_divide_by_zero(client):" >> tests/test_app.py
   echo "    response = client.post('/api/calculate'," >> tests/test_app.py
   echo "                         json={'operation': 'divide', 'a': 5, 'b': 0})" >> tests/test_app.py
   echo "    assert response.status_code == 400" >> tests/test_app.py
   echo "    data = json.loads(response.data)" >> tests/test_app.py
   echo "    assert 'error' in data" >> tests/test_app.py
   ```

4. **Create startup script**:
   ```bash
   echo "#!/bin/bash" > start.sh
   echo "echo 'Starting Python application...'" >> start.sh
   echo "python src/app.py" >> start.sh
   chmod +x start.sh
   ```

5. **Commit the initial application**:
   ```bash
   git add .
   git commit -m "Initial Python Flask application for Azure deployment"
   ```

## Exercise 2: Creating Azure Pipeline Configuration

### Part A: Build Pipeline
1. **Create Azure Pipelines YAML**:
   ```bash
   echo "trigger:" > azure-pipelines.yml
   echo "  branches:" >> azure-pipelines.yml
   echo "    include:" >> azure-pipelines.yml
   echo "    - main" >> azure-pipelines.yml
   echo "    - develop" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "pool:" >> azure-pipelines.yml
   echo "  vmImage: 'ubuntu-latest'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "variables:" >> azure-pipelines.yml
   echo "  pythonVersion: '3.9'" >> azure-pipelines.yml
   echo "  buildConfiguration: 'Release'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "stages:" >> azure-pipelines.yml
   echo "- stage: Build" >> azure-pipelines.yml
   echo "  displayName: 'Build and Test'" >> azure-pipelines.yml
   echo "  jobs:" >> azure-pipelines.yml
   echo "  - job: BuildJob" >> azure-pipelines.yml
   echo "    displayName: 'Build Python App'" >> azure-pipelines.yml
   echo "    steps:" >> azure-pipelines.yml
   echo "    - task: UsePythonVersion@0" >> azure-pipelines.yml
   echo "      inputs:" >> azure-pipelines.yml
   echo "        versionSpec: '$(pythonVersion)'" >> azure-pipelines.yml
   echo "      displayName: 'Use Python $(pythonVersion)'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - script: |" >> azure-pipelines.yml
   echo "        python -m pip install --upgrade pip" >> azure-pipelines.yml
   echo "        pip install -r requirements.txt" >> azure-pipelines.yml
   echo "        pip install pytest pytest-cov" >> azure-pipelines.yml
   echo "      displayName: 'Install dependencies'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - script: |" >> azure-pipelines.yml
   echo "        python -m pytest tests/ --junitxml=junit/test-results.xml --cov=src --cov-report=xml" >> azure-pipelines.yml
   echo "      displayName: 'Run tests'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - task: PublishTestResults@2" >> azure-pipelines.yml
   echo "      inputs:" >> azure-pipelines.yml
   echo "        testResultsFiles: '**/test-*.xml'" >> azure-pipelines.yml
   echo "        testRunTitle: 'Python Tests'" >> azure-pipelines.yml
   echo "      displayName: 'Publish test results'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - task: PublishCodeCoverageResults@1" >> azure-pipelines.yml
   echo "      inputs:" >> azure-pipelines.yml
   echo "        codeCoverageTool: 'Cobertura'" >> azure-pipelines.yml
   echo "        summaryFileLocation: '$(System.DefaultWorkingDirectory)/**/coverage.xml'" >> azure-pipelines.yml
   echo "      displayName: 'Publish code coverage'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - task: ArchiveFiles@2" >> azure-pipelines.yml
   echo "      inputs:" >> azure-pipelines.yml
   echo "        rootFolderOrFile: '$(Build.SourcesDirectory)'" >> azure-pipelines.yml
   echo "        includeRootFolder: false" >> azure-pipelines.yml
   echo "        archiveType: 'zip'" >> azure-pipelines.yml
   echo "        archiveFile: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'" >> azure-pipelines.yml
   echo "      displayName: 'Archive application'" >> azure-pipelines.yml
   echo "" >> azure-pipelines.yml
   echo "    - task: PublishBuildArtifacts@1" >> azure-pipelines.yml
   echo "      inputs:" >> azure-pipelines.yml
   echo "        PathtoPublish: '$(Build.ArtifactStagingDirectory)'" >> azure-pipelines.yml
   echo "        ArtifactName: 'drop'" >> azure-pipelines.yml
   echo "      displayName: 'Publish build artifacts'" >> azure-pipelines.yml
   ```

2. **Create PR validation pipeline**:
   ```bash
   echo "trigger:" > pr-validation.yml
   echo "  branches:" >> pr-validation.yml
   echo "    include:" >> pr-validation.yml
   echo "    - main" >> pr-validation.yml
   echo "    - develop" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "pool:" >> pr-validation.yml
   echo "  vmImage: 'ubuntu-latest'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "steps:" >> pr-validation.yml
   echo "- task: UsePythonVersion@0" >> pr-validation.yml
   echo "  inputs:" >> pr-validation.yml
   echo "    versionSpec: '3.9'" >> pr-validation.yml
   echo "  displayName: 'Use Python 3.9'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "- script: |" >> pr-validation.yml
   echo "    python -m pip install --upgrade pip" >> pr-validation.yml
   echo "    pip install -r requirements.txt" >> pr-validation.yml
   echo "    pip install pytest flake8 black" >> pr-validation.yml
   echo "  displayName: 'Install dependencies'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "- script: |" >> pr-validation.yml
   echo "    python -m flake8 src/ --max-line-length=100" >> pr-validation.yml
   echo "  displayName: 'Lint code'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "- script: |" >> pr-validation.yml
   echo "    python -m black --check src/ --line-length=100" >> pr-validation.yml
   echo "  displayName: 'Check code formatting'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "- script: |" >> pr-validation.yml
   echo "    python -m pytest tests/ --junitxml=junit/test-results.xml" >> pr-validation.yml
   echo "  displayName: 'Run tests'" >> pr-validation.yml
   echo "" >> pr-validation.yml
   echo "- task: PublishTestResults@2" >> pr-validation.yml
   echo "  inputs:" >> pr-validation.yml
   echo "    testResultsFiles: '**/test-*.xml'" >> pr-validation.yml
   echo "    testRunTitle: 'PR Validation Tests'" >> pr-validation.yml
   echo "  displayName: 'Publish test results'" >> pr-validation.yml
   ```

3. **Commit the pipeline configurations**:
   ```bash
   git add azure-pipelines.yml pr-validation.yml
   git commit -m "Add Azure Pipelines configuration for CI/CD"
   ```

## Exercise 3: Environment Configuration

### Part A: Environment-Specific Settings
1. **Create environment configuration**:
   ```bash
   echo "import os" > src/config.py
   echo "from dataclasses import dataclass" >> src/config.py
   echo "" >> src/config.py
   echo "@dataclass" >> src/config.py
   echo "class Config:" >> src/config.py
   echo "    environment: str" >> src/config.py
   echo "    debug: bool" >> src/config.py
   echo "    port: int" >> src/config.py
   echo "    database_url: str" >> src/config.py
   echo "    api_key: str" >> src/config.py
   echo "" >> src/config.py
   echo "def get_config():" >> src/config.py
   echo "    env = os.getenv('ENVIRONMENT', 'development')" >> src/config.py
   echo "    " >> src/config.py
   echo "    if env == 'production':" >> src/config.py
   echo "        return Config(" >> src/config.py
   echo "            environment='production'," >> src/config.py
   echo "            debug=False," >> src/config.py
   echo "            port=int(os.getenv('PORT', 80))," >> src/config.py
   echo "            database_url=os.getenv('PROD_DATABASE_URL', '')," >> src/config.py
   echo "            api_key=os.getenv('PROD_API_KEY', '')" >> src/config.py
   echo "        )" >> src/config.py
   echo "    elif env == 'staging':" >> src/config.py
   echo "        return Config(" >> src/config.py
   echo "            environment='staging'," >> src/config.py
   echo "            debug=True," >> src/config.py
   echo "            port=int(os.getenv('PORT', 5000))," >> src/config.py
   echo "            database_url=os.getenv('STAGING_DATABASE_URL', 'sqlite:///staging.db')," >> src/config.py
   echo "            api_key=os.getenv('STAGING_API_KEY', 'staging-key')" >> src/config.py
   echo "        )" >> src/config.py
   echo "    else:" >> src/config.py
   echo "        return Config(" >> src/config.py
   echo "            environment='development'," >> src/config.py
   echo "            debug=True," >> src/config.py
   echo "            port=int(os.getenv('PORT', 5000))," >> src/config.py
   echo "            database_url='sqlite:///dev.db'," >> src/config.py
   echo "            api_key='dev-key'" >> src/config.py
   echo "        )" >> src/config.py
   ```

2. **Update the main app to use configuration**:
   ```bash
   echo "from flask import Flask, jsonify, request" > src/app.py
   echo "import logging" >> src/app.py
   echo "from src.config import get_config" >> src/app.py
   echo "" >> src/app.py
   echo "app = Flask(__name__)" >> src/app.py
   echo "config = get_config()" >> src/app.py
   echo "" >> src/app.py
   echo "# Configure logging" >> src/app.py
   echo "logging.basicConfig(level=logging.INFO)" >> src/app.py
   echo "logger = logging.getLogger(__name__)" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/')" >> src/app.py
   echo "def home():" >> src/app.py
   echo "    return jsonify({" >> src/app.py
   echo "        'message': 'Hello from Azure!'," >> src/app.py
   echo "        'environment': config.environment," >> src/app.py
   echo "        'debug': config.debug," >> src/app.py
   echo "        'port': config.port" >> src/app.py
   echo "    })" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/health')" >> src/app.py
   echo "def health():" >> src/app.py
   echo "    return jsonify({" >> src/app.py
   echo "        'status': 'healthy'," >> src/app.py
   echo "        'environment': config.environment" >> src/app.py
   echo "    })" >> src/app.py
   echo "" >> src/app.py
   echo "@app.route('/api/calculate', methods=['POST'])" >> src/app.py
   echo "def calculate():" >> src/app.py
   echo "    data = request.get_json()" >> src/app.py
   echo "    if not data or 'operation' not in data:" >> src/app.py
   echo "        return jsonify({'error': 'Invalid input'}), 400" >> src/app.py
   echo "    " >> src/app.py
   echo "    operation = data['operation']" >> src/app.py
   echo "    a = data.get('a', 0)" >> src/app.py
   echo "    b = data.get('b', 0)" >> src/app.py
   echo "    " >> src/app.py
   echo "    try:" >> src/app.py
   echo "        if operation == 'add':" >> src/app.py
   echo "            result = a + b" >> src/app.py
   echo "        elif operation == 'subtract':" >> src/app.py
   echo "            result = a - b" >> src/app.py
   echo "        elif operation == 'multiply':" >> src/app.py
   echo "            result = a * b" >> src/app.py
   echo "        elif operation == 'divide':" >> src/app.py
   echo "            if b == 0:" >> src/app.py
   echo "                return jsonify({'error': 'Cannot divide by zero'}), 400" >> src/app.py
   echo "            result = a / b" >> src/app.py
   echo "        else:" >> src/app.py
   echo "            return jsonify({'error': 'Invalid operation'}), 400" >> src/app.py
   echo "        " >> src/app.py
   echo "        logger.info(f'Calculation: {a} {operation} {b} = {result}')" >> src/app.py
   echo "        return jsonify({'result': result})" >> src/app.py
   echo "    except Exception as e:" >> src/app.py
   echo "        logger.error(f'Calculation error: {str(e)}')" >> src/app.py
   echo "        return jsonify({'error': 'Calculation failed'}), 500" >> src/app.py
   echo "" >> src/app.py
   echo "if __name__ == '__main__':" >> src/app.py
   echo "    app.run(host='0.0.0.0', port=config.port, debug=config.debug)" >> src/app.py
   ```

3. **Create environment-specific files**:
   ```bash
   echo "ENVIRONMENT=development" > .env.development
   echo "PORT=5000" >> .env.development
   echo "DEBUG=true" >> .env.development
   echo "DATABASE_URL=sqlite:///dev.db" >> .env.development
   echo "API_KEY=dev-key" >> .env.development
   
   echo "ENVIRONMENT=staging" > .env.staging
   echo "PORT=5000" >> .env.staging
   echo "DEBUG=true" >> .env.staging
   echo "DATABASE_URL=sqlite:///staging.db" >> .env.staging
   echo "API_KEY=staging-key" >> .env.staging
   
   echo "ENVIRONMENT=production" > .env.production
   echo "PORT=80" >> .env.production
   echo "DEBUG=false" >> .env.production
   echo "DATABASE_URL=" >> .env.production
   echo "API_KEY=" >> .env.production
   ```

4. **Commit the environment configuration**:
   ```bash
   git add src/config.py src/app.py .env.*
   git commit -m "Add environment-specific configuration"
   ```

## Exercise 4: Simulating Azure Deployment

### Part A: Local Testing
1. **Test the application locally**:
   ```bash
   # Test development environment
   export ENVIRONMENT=development
   python src/app.py &
   sleep 2
   curl http://localhost:5000/
   curl http://localhost:5000/health
   curl -X POST http://localhost:5000/api/calculate -H "Content-Type: application/json" -d '{"operation": "add", "a": 5, "b": 3}'
   pkill -f "python src/app.py"
   ```

2. **Test staging environment**:
   ```bash
   # Test staging environment
   export ENVIRONMENT=staging
   python src/app.py &
   sleep 2
   curl http://localhost:5000/
   curl http://localhost:5000/health
   pkill -f "python src/app.py"
   ```

### Part B: Create Deployment Scripts
1. **Create deployment script for Azure App Service**:
   ```bash
   echo "#!/bin/bash" > deploy-azure.sh
   echo "echo 'Deploying to Azure App Service...'" >> deploy-azure.sh
   echo "" >> deploy-azure.sh
   echo "# Set environment variables" >> deploy-azure.sh
   echo "export ENVIRONMENT=production" >> deploy-azure.sh
   echo "export PORT=80" >> deploy-azure.sh
   echo "export DEBUG=false" >> deploy-azure.sh
   echo "" >> deploy-azure.sh
   echo "# Install dependencies" >> deploy-azure.sh
   echo "pip install -r requirements.txt" >> deploy-azure.sh
   echo "" >> deploy-azure.sh
   echo "# Run the application" >> deploy-azure.sh
   echo "gunicorn --bind 0.0.0.0:80 src.app:app" >> deploy-azure.sh
   chmod +x deploy-azure.sh
   ```

2. **Create Docker configuration**:
   ```bash
   echo "FROM python:3.9-slim" > Dockerfile
   echo "" >> Dockerfile
   echo "WORKDIR /app" >> Dockerfile
   echo "" >> Dockerfile
   echo "COPY requirements.txt ." >> Dockerfile
   echo "RUN pip install --no-cache-dir -r requirements.txt" >> Dockerfile
   echo "" >> Dockerfile
   echo "COPY src/ ./src/" >> Dockerfile
   echo "" >> Dockerfile
   echo "EXPOSE 80" >> Dockerfile
   echo "" >> Dockerfile
   echo "ENV ENVIRONMENT=production" >> Dockerfile
   echo "ENV PORT=80" >> Dockerfile
   echo "ENV DEBUG=false" >> Dockerfile
   echo "" >> Dockerfile
   echo "CMD [\"gunicorn\", \"--bind\", \"0.0.0.0:80\", \"src.app:app\"]" >> Dockerfile
   ```

3. **Create Azure Container Registry configuration**:
   ```bash
   echo "FROM python:3.9-slim" > Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "WORKDIR /app" >> Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "COPY requirements.txt ." >> Dockerfile.acr
   echo "RUN pip install --no-cache-dir -r requirements.txt" >> Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "COPY src/ ./src/" >> Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "EXPOSE 80" >> Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "ENV ENVIRONMENT=production" >> Dockerfile.acr
   echo "ENV PORT=80" >> Dockerfile.acr
   echo "ENV DEBUG=false" >> Dockerfile.acr
   echo "" >> Dockerfile.acr
   echo "CMD [\"gunicorn\", \"--bind\", \"0.0.0.0:80\", \"src.app:app\"]" >> Dockerfile.acr
   ```

4. **Commit the deployment configurations**:
   ```bash
   git add deploy-azure.sh Dockerfile Dockerfile.acr
   git commit -m "Add Azure deployment configurations"
   ```

## Exercise 5: Release Pipeline Simulation

### Part A: Create Release Pipeline
1. **Create release pipeline YAML**:
   ```bash
   echo "trigger:" > release-pipeline.yml
   echo "  branches:" >> release-pipeline.yml
   echo "    include:" >> release-pipeline.yml
   echo "    - main" >> release-pipeline.yml
   echo "" >> release-pipeline.yml
   echo "pool:" >> release-pipeline.yml
   echo "  vmImage: 'ubuntu-latest'" >> release-pipeline.yml
   echo "" >> release-pipeline.yml
   echo "stages:" >> release-pipeline.yml
   echo "- stage: Staging" >> release-pipeline.yml
   echo "  displayName: 'Deploy to Staging'" >> release-pipeline.yml
   echo "  jobs:" >> release-pipeline.yml
   echo "  - deployment: DeployStaging" >> release-pipeline.yml
   echo "    displayName: 'Deploy to Staging Environment'" >> release-pipeline.yml
   echo "    environment: 'staging'" >> release-pipeline.yml
   echo "    strategy:" >> release-pipeline.yml
   echo "      runOnce:" >> release-pipeline.yml
   echo "        deploy:" >> release-pipeline.yml
   echo "          steps:" >> release-pipeline.yml
   echo "          - task: AzureWebApp@1" >> release-pipeline.yml
   echo "            inputs:" >> release-pipeline.yml
   echo "              azureSubscription: 'Azure Service Connection'" >> release-pipeline.yml
   echo "              appName: 'my-python-app-staging'" >> release-pipeline.yml
   echo "              package: '$(Pipeline.Workspace)/drop/$(Build.BuildId).zip'" >> release-pipeline.yml
   echo "              deploymentMethod: 'zipDeploy'" >> release-pipeline.yml
   echo "            displayName: 'Deploy to Staging'" >> release-pipeline.yml
   echo "" >> release-pipeline.yml
   echo "- stage: Production" >> release-pipeline.yml
   echo "  displayName: 'Deploy to Production'" >> release-pipeline.yml
   echo "  dependsOn: Staging" >> release-pipeline.yml
   echo "  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))" >> release-pipeline.yml
   echo "  jobs:" >> release-pipeline.yml
   echo "  - deployment: DeployProduction" >> release-pipeline.yml
   echo "    displayName: 'Deploy to Production Environment'" >> release-pipeline.yml
   echo "    environment: 'production'" >> release-pipeline.yml
   echo "    strategy:" >> release-pipeline.yml
   echo "      runOnce:" >> release-pipeline.yml
   echo "        deploy:" >> release-pipeline.yml
   echo "          steps:" >> release-pipeline.yml
   echo "          - task: AzureWebApp@1" >> release-pipeline.yml
   echo "            inputs:" >> release-pipeline.yml
   echo "              azureSubscription: 'Azure Service Connection'" >> release-pipeline.yml
   echo "              appName: 'my-python-app-production'" >> release-pipeline.yml
   echo "              package: '$(Pipeline.Workspace)/drop/$(Build.BuildId).zip'" >> release-pipeline.yml
   echo "              deploymentMethod: 'zipDeploy'" >> release-pipeline.yml
   echo "            displayName: 'Deploy to Production'" >> release-pipeline.yml
   ```

2. **Create rollback pipeline**:
   ```bash
   echo "trigger:" > rollback-pipeline.yml
   echo "  branches:" >> rollback-pipeline.yml
   echo "    include:" >> rollback-pipeline.yml
   echo "    - main" >> rollback-pipeline.yml
   echo "" >> rollback-pipeline.yml
   echo "pool:" >> rollback-pipeline.yml
   echo "  vmImage: 'ubuntu-latest'" >> rollback-pipeline.yml
   echo "" >> rollback-pipeline.yml
   echo "steps:" >> rollback-pipeline.yml
   echo "- script: |" >> rollback-pipeline.yml
   echo "    echo 'Rolling back to previous version...'" >> rollback-pipeline.yml
   echo "    # Get previous successful deployment" >> rollback-pipeline.yml
   echo "    az webapp deployment list --name my-python-app-production --resource-group my-rg" >> rollback-pipeline.yml
   echo "  displayName: 'Get deployment history'" >> rollback-pipeline.yml
   echo "" >> rollback-pipeline.yml
   echo "- script: |" >> rollback-pipeline.yml
   echo "    # Deploy previous version" >> rollback-pipeline.yml
   echo "    az webapp deployment source config-zip --name my-python-app-production --resource-group my-rg --src previous-version.zip" >> rollback-pipeline.yml
   echo "  displayName: 'Rollback to previous version'" >> rollback-pipeline.yml
   ```

3. **Commit the release configurations**:
   ```bash
   git add release-pipeline.yml rollback-pipeline.yml
   git commit -m "Add release and rollback pipeline configurations"
   ```

## Exercise 6: Monitoring and Health Checks

### Part A: Add Monitoring
1. **Create monitoring script**:
   ```bash
   echo "#!/bin/bash" > monitor.sh
   echo "echo 'Monitoring application health...'" >> monitor.sh
   echo "" >> monitor.sh
   echo "# Check if application is running" >> monitor.sh
   echo "if curl -f http://localhost:80/health > /dev/null 2>&1; then" >> monitor.sh
   echo "    echo 'Application is healthy'" >> monitor.sh
   echo "    exit 0" >> monitor.sh
   echo "else" >> monitor.sh
   echo "    echo 'Application is unhealthy'" >> monitor.sh
   echo "    exit 1" >> monitor.sh
   echo "fi" >> monitor.sh
   chmod +x monitor.sh
   ```

2. **Create health check endpoint**:
   ```bash
   echo "from flask import jsonify" > src/health.py
   echo "import time" >> src/health.py
   echo "import psutil" >> src/health.py
   echo "" >> src/health.py
   echo "def get_health_status():" >> src/health.py
   echo "    try:" >> src/health.py
   echo "        # Check system resources" >> src/health.py
   echo "        cpu_percent = psutil.cpu_percent(interval=1)" >> src/health.py
   echo "        memory = psutil.virtual_memory()" >> src/health.py
   echo "        " >> src/health.py
   echo "        # Check if resources are within acceptable limits" >> src/health.py
   echo "        if cpu_percent > 90 or memory.percent > 90:" >> src/health.py
   echo "            return {" >> src/health.py
   echo "                'status': 'unhealthy'," >> src/health.py
   echo "                'cpu_percent': cpu_percent," >> src/health.py
   echo "                'memory_percent': memory.percent" >> src/health.py
   echo "            }" >> src/health.py
   echo "        " >> src/health.py
   echo "        return {" >> src/health.py
   echo "            'status': 'healthy'," >> src/health.py
   echo "            'cpu_percent': cpu_percent," >> src/health.py
   echo "            'memory_percent': memory.percent" >> src/health.py
   echo "        }" >> src/health.py
   echo "    except Exception as e:" >> src/health.py
   echo "        return {" >> src/health.py
   echo "            'status': 'unhealthy'," >> src/health.py
   echo "            'error': str(e)" >> src/health.py
   echo "        }" >> src/health.py
   ```

3. **Update requirements.txt**:
   ```bash
   echo "Flask==2.3.3" > requirements.txt
   echo "gunicorn==21.2.0" >> requirements.txt
   echo "python-dotenv==1.0.0" >> requirements.txt
   echo "psutil==5.9.5" >> requirements.txt
   ```

4. **Commit the monitoring additions**:
   ```bash
   git add monitor.sh src/health.py requirements.txt
   git commit -m "Add monitoring and health check functionality"
   ```

## Discussion Questions

1. **What are the benefits of using Azure Pipelines for Python applications?**
   - How does it integrate with Git workflows?
   - What automation does it provide?

2. **How do you handle different environments in Azure?**
   - What configuration management strategies work best?
   - How do you ensure consistency across environments?

3. **What deployment strategies are most effective for Python applications?**
   - When would you use blue-green vs. canary deployment?
   - How do you minimize downtime during deployments?

4. **How do you monitor and troubleshoot Azure deployments?**
   - What tools and techniques are most helpful?
   - How do you handle rollbacks when things go wrong?

5. **What role should AI play in Azure deployment and monitoring?**
   - Can AI help with configuration and troubleshooting?
   - How do you balance automation with human oversight?

## Key Takeaways

- **Git enables automation**: Every commit can trigger builds and deployments
- **Azure Pipelines provide CI/CD**: Automated testing, building, and deployment
- **Environment management is crucial**: Separate configs for dev, staging, and prod
- **Monitoring is essential**: Track application health and performance
- **Security matters**: Use Azure Key Vault and security scanning
- **Rollback strategies are important**: Always have a plan to revert
- **AI can help with configuration**: But understand the concepts first
- **Practice makes perfect**: The more you deploy, the better you get




