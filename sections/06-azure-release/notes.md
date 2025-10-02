# Section 6: Release Process in Azure

## Learning Objectives
- Connect Git workflow to deployment on your Windows-based Azure environment
- Demonstrate the path from commit → pull request → merge → release
- Show how disciplined Git use directly supports reliable deployments

## Key Talking Points

### 1. Azure DevOps and Git Integration

#### Azure DevOps Services
- **Azure Repos**: Git repository hosting
- **Azure Pipelines**: CI/CD automation
- **Azure Artifacts**: Package management
- **Azure Boards**: Work item tracking
- **Azure Test Plans**: Testing management

#### Git Integration Benefits
- **Source Control**: Centralized code repository
- **Version Control**: Track changes and releases
- **Collaboration**: Team development workflows
- **Automation**: Trigger builds and deployments
- **Auditability**: Complete change history

### 2. Azure Pipelines for Python Applications

#### Pipeline Structure
```yaml
# azure-pipelines.yml
trigger:
- main
- develop

pool:
  vmImage: 'ubuntu-latest'

variables:
  pythonVersion: '3.9'

stages:
- stage: Build
  displayName: 'Build and Test'
  jobs:
  - job: BuildJob
    displayName: 'Build Python App'
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '$(pythonVersion)'
    - script: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
      displayName: 'Install dependencies'
    - script: |
        python -m pytest tests/ --junitxml=junit/test-results.xml
      displayName: 'Run tests'
    - task: PublishTestResults@2
      inputs:
        testResultsFiles: '**/test-*.xml'
        testRunTitle: 'Python Tests'
```

#### Build Triggers
- **Push to main**: Automatic production builds
- **Push to develop**: Staging builds
- **Pull request**: Validation builds
- **Manual trigger**: On-demand builds
- **Scheduled**: Nightly builds

### 3. Release Management in Azure

#### Release Pipeline Stages
1. **Build Stage**: Compile and test
2. **Staging Stage**: Deploy to test environment
3. **Production Stage**: Deploy to live environment
4. **Rollback Stage**: Revert if issues occur

#### Environment Configuration
```yaml
# Release pipeline configuration
environments:
- environment: 'staging'
  displayName: 'Staging Environment'
  variables:
    - name: 'appUrl'
      value: 'https://staging.myapp.com'
    - name: 'dbConnection'
      value: 'staging-db-connection'

- environment: 'production'
  displayName: 'Production Environment'
  variables:
    - name: 'appUrl'
      value: 'https://myapp.com'
    - name: 'dbConnection'
      value: 'prod-db-connection'
```

### 4. Python-Specific Azure Configurations

#### Azure App Service for Python
```yaml
# Deploy to Azure App Service
- task: AzureWebApp@1
  inputs:
    azureSubscription: 'Azure Service Connection'
    appName: 'my-python-app'
    package: '$(Build.ArtifactStagingDirectory)/**/*.zip'
    deploymentMethod: 'zipDeploy'
```

#### Azure Functions for Python
```yaml
# Deploy to Azure Functions
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: 'Azure Service Connection'
    appName: 'my-python-function'
    package: '$(Build.ArtifactStagingDirectory)/**/*.zip'
    deploymentMethod: 'zipDeploy'
```

#### Azure Container Instances
```yaml
# Deploy to Azure Container Instances
- task: AzureContainerInstances@0
  inputs:
    azureSubscription: 'Azure Service Connection'
    containerName: 'my-python-container'
    imageName: 'myregistry.azurecr.io/myapp:$(Build.BuildId)'
    resourceGroup: 'my-resource-group'
    location: 'East US'
```

### 5. Git Workflow Integration

#### Branch Protection Rules
```yaml
# Branch protection for main
protection_rules:
  main:
    required_status_checks:
      strict: true
      contexts:
        - "ci/build"
        - "ci/test"
    enforce_admins: true
    required_pull_request_reviews:
      required_approving_review_count: 2
      dismiss_stale_reviews: true
    restrictions: null
```

#### Pull Request Validation
```yaml
# PR validation pipeline
pr_validation:
  trigger:
    branches:
      include:
        - main
        - develop
  pool:
    vmImage: 'ubuntu-latest'
  steps:
  - script: |
      python -m pytest tests/ --junitxml=junit/test-results.xml
    displayName: 'Run tests on PR'
  - script: |
      python -m flake8 src/
    displayName: 'Lint code'
  - script: |
      python -m black --check src/
    displayName: 'Check code formatting'
```

### 6. Deployment Strategies

#### Blue-Green Deployment
```yaml
# Blue-green deployment strategy
blue_green:
  stages:
  - stage: 'Blue'
    displayName: 'Blue Environment'
    jobs:
    - deployment: 'DeployBlue'
      environment: 'blue'
      strategy:
        runOnce:
          deploy:
            steps:
            - task: AzureWebApp@1
              inputs:
                appName: 'my-app-blue'
                package: '$(Build.ArtifactStagingDirectory)/**/*.zip'
  
  - stage: 'Green'
    displayName: 'Green Environment'
    jobs:
    - deployment: 'DeployGreen'
      environment: 'green'
      strategy:
        runOnce:
          deploy:
            steps:
            - task: AzureWebApp@1
              inputs:
                appName: 'my-app-green'
                package: '$(Build.ArtifactStagingDirectory)/**/*.zip'
```

#### Canary Deployment
```yaml
# Canary deployment strategy
canary:
  stages:
  - stage: 'Canary'
    displayName: 'Canary Deployment'
    jobs:
    - deployment: 'DeployCanary'
      environment: 'canary'
      strategy:
        canary:
          increments: [10, 20, 50, 100]
          preDeploy:
            steps:
            - script: |
                echo "Deploying to canary environment"
          deploy:
            steps:
            - task: AzureWebApp@1
              inputs:
                appName: 'my-app-canary'
                package: '$(Build.ArtifactStagingDirectory)/**/*.zip'
```

### 7. Environment-Specific Configurations

#### Configuration Management
```python
# config.py
import os
from dataclasses import dataclass

@dataclass
class Config:
    environment: str
    database_url: str
    api_key: str
    debug: bool

def get_config():
    env = os.getenv('ENVIRONMENT', 'development')
    
    if env == 'production':
        return Config(
            environment='production',
            database_url=os.getenv('PROD_DATABASE_URL'),
            api_key=os.getenv('PROD_API_KEY'),
            debug=False
        )
    elif env == 'staging':
        return Config(
            environment='staging',
            database_url=os.getenv('STAGING_DATABASE_URL'),
            api_key=os.getenv('STAGING_API_KEY'),
            debug=True
        )
    else:
        return Config(
            environment='development',
            database_url='sqlite:///dev.db',
            api_key='dev-key',
            debug=True
        )
```

#### Environment Variables
```yaml
# Azure Pipeline variables
variables:
  - name: 'ENVIRONMENT'
    value: 'staging'
  - name: 'DATABASE_URL'
    value: '$(staging-database-url)'
  - name: 'API_KEY'
    value: '$(staging-api-key)'
```

### 8. Monitoring and Logging

#### Application Insights Integration
```python
# app_insights.py
from opencensus.ext.azure.log_exporter import AzureLogHandler
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
import logging

def setup_logging():
    logger = logging.getLogger(__name__)
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=your-key'
    ))
    return logger
```

#### Health Checks
```python
# health_check.py
from flask import Flask, jsonify
import requests

app = Flask(__name__)

@app.route('/health')
def health_check():
    try:
        # Check database connection
        db_status = check_database()
        
        # Check external services
        api_status = check_external_api()
        
        return jsonify({
            'status': 'healthy',
            'database': db_status,
            'api': api_status
        })
    except Exception as e:
        return jsonify({
            'status': 'unhealthy',
            'error': str(e)
        }), 500
```

### 9. Rollback Strategies

#### Automated Rollback
```yaml
# Rollback pipeline
rollback:
  trigger:
    branches:
      include:
        - main
  pool:
    vmImage: 'ubuntu-latest'
  steps:
  - script: |
      # Get previous successful deployment
      az webapp deployment list --name my-app --resource-group my-rg
    displayName: 'Get deployment history'
  
  - script: |
      # Deploy previous version
      az webapp deployment source config-zip --name my-app --resource-group my-rg --src previous-version.zip
    displayName: 'Rollback to previous version'
```

#### Manual Rollback Process
1. **Identify the issue**: Monitor application health
2. **Find last known good version**: Check deployment history
3. **Deploy previous version**: Use Azure CLI or portal
4. **Verify rollback**: Test application functionality
5. **Investigate root cause**: Fix the issue in development

### 10. Security Considerations

#### Secret Management
```yaml
# Azure Key Vault integration
- task: AzureKeyVault@2
  inputs:
    azureSubscription: 'Azure Service Connection'
    KeyVaultName: 'my-keyvault'
    SecretsFilter: '*'
    RunAsPreJob: false
```

#### Security Scanning
```yaml
# Security scanning in pipeline
- script: |
    pip install safety
    safety check --json --output safety-report.json
  displayName: 'Security vulnerability scan'
  
- script: |
    pip install bandit
    bandit -r src/ -f json -o bandit-report.json
  displayName: 'Code security analysis'
```

### 11. Performance Optimization

#### Build Optimization
```yaml
# Optimized build pipeline
build_optimization:
  steps:
  - task: Cache@2
    inputs:
      key: 'pip | requirements.txt'
      path: '$(PIP_CACHE_DIR)'
    displayName: 'Cache pip dependencies'
  
  - script: |
      pip install --cache-dir $(PIP_CACHE_DIR) -r requirements.txt
    displayName: 'Install dependencies'
  
  - script: |
      python -m pytest tests/ --junitxml=junit/test-results.xml --cov=src --cov-report=xml
    displayName: 'Run tests with coverage'
```

#### Deployment Optimization
```yaml
# Optimized deployment
deployment_optimization:
  steps:
  - task: ArchiveFiles@2
    inputs:
      rootFolderOrFile: '$(Build.SourcesDirectory)'
      includeRootFolder: false
      archiveType: 'zip'
      archiveFile: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'
    displayName: 'Create deployment package'
  
  - task: AzureWebApp@1
    inputs:
      azureSubscription: 'Azure Service Connection'
      appName: 'my-python-app'
      package: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'
      deploymentMethod: 'zipDeploy'
    displayName: 'Deploy to Azure App Service'
```

### 12. Troubleshooting Common Issues

#### Build Failures
```bash
# Check build logs
az pipelines runs list --project my-project --top 10

# Get specific build details
az pipelines runs show --id 123 --project my-project

# Download build logs
az pipelines runs artifact download --id 123 --artifact-name logs --project my-project
```

#### Deployment Issues
```bash
# Check deployment status
az webapp deployment list --name my-app --resource-group my-rg

# View application logs
az webapp log tail --name my-app --resource-group my-rg

# Restart application
az webapp restart --name my-app --resource-group my-rg
```

#### Environment Issues
```bash
# Check environment variables
az webapp config appsettings list --name my-app --resource-group my-rg

# Update environment variables
az webapp config appsettings set --name my-app --resource-group my-rg --settings "ENVIRONMENT=production"
```

### 13. AI Assistant Integration

#### When to Ask AI for Help
- **Pipeline configuration**: "How do I configure Azure Pipelines for Python?"
- **Deployment issues**: "My Azure deployment is failing, how do I debug it?"
- **Environment setup**: "How do I set up different environments in Azure?"
- **Security questions**: "How do I securely manage secrets in Azure?"

#### Example Prompts
- "I need to set up a CI/CD pipeline for my Python Flask app in Azure DevOps"
- "My Azure App Service deployment is failing with a 500 error, how do I troubleshoot?"
- "How do I configure blue-green deployment for my Python application in Azure?"
- "What's the best way to manage environment variables for different Azure environments?"

### 14. Best Practices for Azure Releases

#### Git Workflow Integration
- **Use feature branches**: Isolate changes before merging
- **Require pull requests**: Enforce code review process
- **Automate testing**: Run tests on every PR
- **Tag releases**: Mark stable versions for deployment

#### Deployment Practices
- **Test in staging**: Always test before production
- **Use blue-green**: Minimize downtime during deployments
- **Monitor deployments**: Watch for issues after deployment
- **Have rollback plan**: Know how to revert if needed

#### Security Practices
- **Use Key Vault**: Store secrets securely
- **Scan dependencies**: Check for vulnerabilities
- **Limit permissions**: Use least privilege access
- **Audit changes**: Track all configuration changes

## Key Takeaways

1. **Git enables automation**: Every commit can trigger builds and deployments
2. **Azure Pipelines provide CI/CD**: Automated testing, building, and deployment
3. **Environment management is crucial**: Separate configs for dev, staging, and prod
4. **Monitoring is essential**: Track application health and performance
5. **Security matters**: Use Azure Key Vault and security scanning
6. **Rollback strategies are important**: Always have a plan to revert
7. **AI can help with configuration**: But understand the concepts first
8. **Practice makes perfect**: The more you deploy, the better you get

