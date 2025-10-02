# Section 6: Release Process in Azure

---

## Learning Objectives
- <span class="fragment">Connect Git workflow to deployment on your Windows-based Azure environment</span>  
- <span class="fragment">Demonstrate the path from commit → pull request → merge → release</span>  
- <span class="fragment">Show how disciplined Git use directly supports reliable deployments</span>  

<!-- Position this as bridging Git concepts with real-world deployment pipelines. -->

---

## Azure DevOps and Git Integration

**Azure DevOps Services**
- <span class="fragment">Azure Repos: Git repository hosting</span>  
- <span class="fragment">Azure Pipelines: CI/CD automation</span>  
- <span class="fragment">Azure Artifacts: Package management</span>  
- <span class="fragment">Azure Boards: Work item tracking</span>  
- <span class="fragment">Azure Test Plans: Testing management</span>  

---

## Azure Pipelines for Python Applications

**Pipeline Structure Example**
```yaml
trigger:
- main
- develop

pool:
  vmImage: 'ubuntu-latest'
variables:
  pythonVersion: '3.9'
````

* <span class="fragment">Build stage: install dependencies</span>
* <span class="fragment">Run tests with Pytest</span>
* <span class="fragment">Publish test results</span>

---

**Build Triggers**

* <span class="fragment">Push to main → production build</span>
* <span class="fragment">Push to develop → staging build</span>
* <span class="fragment">Pull requests → validation build</span>
* <span class="fragment">Manual trigger or nightly schedule</span>

---

## Release Management in Azure

**Release Stages**

* <span class="fragment">Build → compile & test</span>
* <span class="fragment">Staging → deploy for testing</span>
* <span class="fragment">Production → live deployment</span>
* <span class="fragment">Rollback → revert if issues arise</span>

---

**Environment Config**

```yaml
environments:
- environment: 'staging'
  variables:
    - name: 'appUrl'
      value: 'https://staging.myapp.com'
- environment: 'production'
  variables:
    - name: 'appUrl'
      value: 'https://myapp.com'
```

---

## Python-Specific Azure Configurations

* <span class="fragment">Azure App Service for Python (zipDeploy)</span>
* <span class="fragment">Azure Functions (serverless Python apps)</span>
* <span class="fragment">Azure Container Instances (Docker images)</span>

```yaml
- task: AzureWebApp@1
  inputs:
    appName: 'my-python-app'
    package: '**/*.zip'
    deploymentMethod: 'zipDeploy'
```

---

## Git Workflow Integration

**Branch Protection**

* <span class="fragment">Require CI status checks</span>
* <span class="fragment">Two reviewers for PRs</span>
* <span class="fragment">Block direct pushes to main</span>

---

**Pull Request Validation**

* <span class="fragment">Run tests on PR</span>
* <span class="fragment">Lint with flake8</span>
* <span class="fragment">Check formatting with Black</span>

---

## Deployment Strategies

**Blue-Green Deployment**

* <span class="fragment">Deploy to “blue” environment</span>
* <span class="fragment">Switch traffic once healthy</span>
* <span class="fragment">Rollback by reverting traffic</span>

---

**Canary Deployment**

* <span class="fragment">Release to 10% users → 20% → 50% → 100%</span>
* <span class="fragment">Monitor at each step</span>
* <span class="fragment">Abort if errors detected</span>

---

## Environment-Specific Configurations

**Python Config Management**

```python
@dataclass
class Config:
    environment: str
    database_url: str
    debug: bool
```

* <span class="fragment">Use environment variables for secrets</span>
* <span class="fragment">Different configs for dev, staging, prod</span>
* <span class="fragment">Keep secrets out of source code</span>

---

## Monitoring and Logging

* <span class="fragment">Azure Application Insights integration</span>
* <span class="fragment">Add health check endpoints</span>
* <span class="fragment">Track API and DB connectivity</span>

```python
@app.route('/health')
def health_check():
    return {'status': 'healthy'}
```

---

## Rollback Strategies

**Automated Rollback**

* <span class="fragment">Detect failure → redeploy last good build</span>

**Manual Rollback**

* <span class="fragment">Identify issue</span>
* <span class="fragment">Deploy previous version</span>
* <span class="fragment">Verify recovery</span>

---

## Security Considerations

* <span class="fragment">Use Azure Key Vault for secrets</span>
* <span class="fragment">Scan dependencies with Safety</span>
* <span class="fragment">Analyse code with Bandit</span>
* <span class="fragment">Restrict pipeline permissions</span>

---

## Performance Optimisation

* <span class="fragment">Cache pip dependencies</span>
* <span class="fragment">Run tests with coverage reports</span>
* <span class="fragment">Deploy using pre-built packages</span>

```yaml
- task: Cache@2
  inputs:
    key: 'pip | requirements.txt'
    path: '$(PIP_CACHE_DIR)'
```

---

## Troubleshooting Common Issues

* <span class="fragment">Build failures → check logs</span>
* <span class="fragment">Deployment issues → tail logs</span>
* <span class="fragment">Environment misconfigurations → inspect app settings</span>

```bash
az webapp log tail --name my-app
```

---

## AI Assistant Integration

* <span class="fragment">Pipeline config help</span>
* <span class="fragment">Debugging deployment issues</span>
* <span class="fragment">Environment setup guidance</span>
* <span class="fragment">Security recommendations</span>

---

**Example Prompts**

* “How do I set up blue-green deployment in Azure Pipelines?”
* “My Azure Function deployment fails — help me debug.”

---

## Best Practices for Azure Releases

**Git Workflow**

* <span class="fragment">Feature branches</span>
* <span class="fragment">PR reviews</span>
* <span class="fragment">Automated tests</span>
* <span class="fragment">Tag releases</span>

---

**Deployment**

* <span class="fragment">Test in staging first</span>
* <span class="fragment">Blue-green or canary for safety</span>
* <span class="fragment">Monitor deployments</span>
* <span class="fragment">Always have rollback plan</span>

---

**Security**

* <span class="fragment">Key Vault for secrets</span>
* <span class="fragment">Dependency scanning</span>
* <span class="fragment">Audit changes</span>

---

## Key Takeaways

* <span class="fragment">Git enables automated CI/CD in Azure</span>
* <span class="fragment">Pipelines enforce discipline and reliability</span>
* <span class="fragment">Separate configs for dev/staging/prod</span>
* <span class="fragment">Monitoring & logging are essential</span>
* <span class="fragment">Security practices must be baked in</span>
* <span class="fragment">Rollback strategies prevent downtime</span>
* <span class="fragment">AI can assist, but concepts come first</span>
* <span class="fragment">Deploying often builds confidence</span>

<!-- Close by tying Git discipline to safe, repeatable Azure releases. -->
