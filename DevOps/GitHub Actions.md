# GitHub Actions Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Workflow Syntax](#workflow-syntax)
- [Events & Triggers](#events--triggers)
- [Jobs](#jobs)
- [Steps](#steps)
- [Actions](#actions)
- [Contexts & Expressions](#contexts--expressions)
- [Secrets & Variables](#secrets--variables)
- [Matrix Strategy](#matrix-strategy)
- [Caching](#caching)
- [Artifacts](#artifacts)
- [Environments](#environments)
- [Common Workflows](#common-workflows)
- [Best Practices](#best-practices)

---

## Basics

### Workflow Structure
```yaml
# .github/workflows/main.yml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run a one-line script
        run: echo Hello, world!
```

---

## Workflow Syntax

### Basic Workflow
```yaml
name: CI/CD Pipeline

# Controls when workflow runs
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

# Environment variables
env:
  NODE_VERSION: 18

jobs:
  build:
    name: Build Application
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
```

---

## Events & Triggers

### Push & Pull Request
```yaml
on:
  push:
    branches:
      - main
      - develop
    tags:
      - v*
    paths:
      - 'src/**'
      - '!**.md'
  
  pull_request:
    branches:
      - main
    types:
      - opened
      - synchronize
      - reopened
```

### Schedule (Cron)
```yaml
on:
  schedule:
    # Runs at 00:00 UTC every day
    - cron: '0 0 * * *'
    # Runs every Monday at 9:00 AM
    - cron: '0 9 * * 1'
```

### Manual Trigger
```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to deploy'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
      debug:
        description: 'Enable debug mode'
        required: false
        type: boolean
```

### Multiple Events
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:
```

### Repository Events
```yaml
on:
  issues:
    types: [opened, edited, deleted]
  
  issue_comment:
    types: [created, edited]
  
  release:
    types: [published]
  
  workflow_run:
    workflows: [CI]
    types: [completed]
```

---

## Jobs

### Basic Job
```yaml
jobs:
  build:
    name: Build Application
    runs-on: ubuntu-latest
    timeout-minutes: 10
    
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
```

### Job Dependencies
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building..."
  
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing..."
  
  deploy:
    needs: [build, test]
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying..."
```

### Conditional Jobs
```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to production"
  
  deploy-staging:
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to staging"
```

### Job Outputs
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.get_version.outputs.version }}
    steps:
      - name: Get version
        id: get_version
        run: echo "version=1.0.0" >> $GITHUB_OUTPUT
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying version ${{ needs.build.outputs.version }}"
```

### Multiple Runners
```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

---

## Steps

### Basic Steps
```yaml
steps:
  # Checkout code
  - name: Checkout
    uses: actions/checkout@v4
  
  # Run command
  - name: Run script
    run: npm test
  
  # Multi-line command
  - name: Multi-line script
    run: |
      echo "Line 1"
      echo "Line 2"
      npm run build
  
  # Working directory
  - name: Run in directory
    run: npm install
    working-directory: ./frontend
  
  # Continue on error
  - name: Might fail
    run: npm test
    continue-on-error: true
  
  # Timeout
  - name: Long task
    run: npm run long-task
    timeout-minutes: 30
```

### Conditional Steps
```yaml
steps:
  - name: Deploy to production
    if: github.ref == 'refs/heads/main'
    run: npm run deploy
  
  - name: Deploy to staging
    if: github.ref == 'refs/heads/develop'
    run: npm run deploy:staging
  
  - name: Run on success
    if: success()
    run: echo "Previous steps succeeded"
  
  - name: Run on failure
    if: failure()
    run: echo "Previous steps failed"
  
  - name: Always run
    if: always()
    run: echo "This always runs"
```

### Step Outputs
```yaml
steps:
  - name: Set output
    id: vars
    run: |
      echo "sha_short=$(git rev-parse --short HEAD)" >> $GITHUB_OUTPUT
      echo "branch=${GITHUB_REF#refs/heads/}" >> $GITHUB_OUTPUT
  
  - name: Use output
    run: |
      echo "Short SHA: ${{ steps.vars.outputs.sha_short }}"
      echo "Branch: ${{ steps.vars.outputs.branch }}"
```

---

## Actions

### Using Actions
```yaml
steps:
  # Checkout code
  - name: Checkout
    uses: actions/checkout@v4
  
  # Setup Node.js
  - name: Setup Node
    uses: actions/setup-node@v4
    with:
      node-version: '18'
      cache: 'npm'
  
  # Setup Python
  - name: Setup Python
    uses: actions/setup-python@v5
    with:
      python-version: '3.11'
  
  # Cache dependencies
  - name: Cache
    uses: actions/cache@v4
    with:
      path: ~/.npm
      key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
  
  # Upload artifacts
  - name: Upload artifact
    uses: actions/upload-artifact@v4
    with:
      name: build-files
      path: dist/
  
  # Download artifacts
  - name: Download artifact
    uses: actions/download-artifact@v4
    with:
      name: build-files
      path: dist/
```

### Popular Actions
```yaml
steps:
  # Checkout with submodules
  - uses: actions/checkout@v4
    with:
      submodules: recursive
  
  # Docker build and push
  - uses: docker/build-push-action@v5
    with:
      context: .
      push: true
      tags: user/app:latest
  
  # Deploy to GitHub Pages
  - uses: peaceiris/actions-gh-pages@v3
    with:
      github_token: ${{ secrets.GITHUB_TOKEN }}
      publish_dir: ./public
  
  # Slack notification
  - uses: slackapi/slack-github-action@v1
    with:
      payload: |
        {
          "text": "Deployment completed"
        }
    env:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

---

## Contexts & Expressions

### GitHub Context
```yaml
steps:
  - name: Print context
    run: |
      echo "Repository: ${{ github.repository }}"
      echo "Ref: ${{ github.ref }}"
      echo "SHA: ${{ github.sha }}"
      echo "Actor: ${{ github.actor }}"
      echo "Event: ${{ github.event_name }}"
      echo "Run ID: ${{ github.run_id }}"
      echo "Run Number: ${{ github.run_number }}"
```

### Environment Variables
```yaml
env:
  GLOBAL_VAR: global value

jobs:
  build:
    env:
      JOB_VAR: job value
    steps:
      - name: Use variables
        env:
          STEP_VAR: step value
        run: |
          echo "Global: $GLOBAL_VAR"
          echo "Job: $JOB_VAR"
          echo "Step: $STEP_VAR"
          echo "GitHub: ${{ github.repository }}"
```

### Expressions
```yaml
steps:
  # String operations
  - name: String functions
    run: |
      echo "${{ contains('hello world', 'hello') }}"
      echo "${{ startsWith('hello', 'he') }}"
      echo "${{ endsWith('hello', 'lo') }}"
      echo "${{ format('Hello {0}', 'World') }}"
  
  # Conditional
  - name: Conditional
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    run: echo "Push to main"
  
  # Boolean operations
  - name: Boolean
    if: success() && !cancelled()
    run: echo "Success and not cancelled"
```

---

## Secrets & Variables

### Using Secrets
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
        run: |
          echo "Deploying with API key"
          ./deploy.sh
```

### Repository Variables
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Use variables
        env:
          APP_NAME: ${{ vars.APP_NAME }}
          ENVIRONMENT: ${{ vars.ENVIRONMENT }}
        run: |
          echo "Building $APP_NAME for $ENVIRONMENT"
```

---

## Matrix Strategy

### Basic Matrix
```yaml
jobs:
  test:
    strategy:
      matrix:
        node-version: [16, 18, 20]
        os: [ubuntu-latest, windows-latest]
    
    runs-on: ${{ matrix.os }}
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

### Matrix with Include
```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        node: [16, 18]
        include:
          - os: ubuntu-latest
            node: 20
            experimental: true
          - os: macos-latest
            node: 18
    
    runs-on: ${{ matrix.os }}
    continue-on-error: ${{ matrix.experimental || false }}
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
```

### Matrix with Exclude
```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [16, 18, 20]
        exclude:
          - os: macos-latest
            node: 16
          - os: windows-latest
            node: 20
    
    runs-on: ${{ matrix.os }}
```

---

## Caching

### NPM Cache
```yaml
steps:
  - uses: actions/checkout@v4
  
  - uses: actions/setup-node@v4
    with:
      node-version: '18'
      cache: 'npm'
  
  - run: npm ci
```

### Custom Cache
```yaml
steps:
  - uses: actions/cache@v4
    with:
      path: |
        ~/.npm
        ~/.cache
      key: ${{ runner.os }}-cache-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-cache-
```

### Python Cache
```yaml
steps:
  - uses: actions/setup-python@v5
    with:
      python-version: '3.11'
      cache: 'pip'
  
  - run: pip install -r requirements.txt
```

---

## Artifacts

### Upload Artifacts
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build
      
      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: |
            dist/
            build/
          retention-days: 5
```

### Download Artifacts
```yaml
jobs:
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: build-files
          path: ./dist
      
      - run: ls -la ./dist
```

---

## Environments

### Using Environments
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://example.com
    steps:
      - name: Deploy
        run: ./deploy.sh
```

### Environment Secrets
```yaml
jobs:
  deploy-staging:
    environment: staging
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: ./deploy.sh
  
  deploy-production:
    environment: production
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: ./deploy.sh
```

---

## Common Workflows

### Node.js CI/CD
```yaml
name: Node.js CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16, 18, 20]
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Build
        run: npm run build
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        if: matrix.node-version == 18
  
  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run build
      
      - name: Deploy
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: npm run deploy
```

### Docker Build & Push
```yaml
name: Docker Build

on:
  push:
    branches: [main]
    tags: [v*]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: user/app
          tags: |
            type=ref,event=branch
            type=semver,pattern={{version}}
            type=sha
      
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### Deploy to AWS
```yaml
name: Deploy to AWS

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2
      
      - name: Build and push to ECR
        run: |
          docker build -t myapp .
          docker tag myapp:latest $ECR_REGISTRY/myapp:latest
          docker push $ECR_REGISTRY/myapp:latest
      
      - name: Deploy to ECS
        run: |
          aws ecs update-service \
            --cluster my-cluster \
            --service my-service \
            --force-new-deployment
```

---

## Best Practices

### Workflow Organization
```yaml
# ✅ Good: Clear job names
jobs:
  build:
    name: Build Application
  test:
    name: Run Tests
  deploy:
    name: Deploy to Production

# ✅ Good: Use descriptive step names
steps:
  - name: Checkout source code
  - name: Install dependencies with npm
  - name: Run unit tests
  - name: Build production bundle

# ✅ Good: Group related steps
steps:
  - name: Setup
    run: |
      npm ci
      npm run build
```

### Security
```yaml
# ✅ Good: Use secrets for sensitive data
env:
  API_KEY: ${{ secrets.API_KEY }}

# ❌ Bad: Hardcode secrets
env:
  API_KEY: abc123secret

# ✅ Good: Pin action versions
- uses: actions/checkout@v4.1.0

# ❌ Bad: Use latest or branch
- uses: actions/checkout@main

# ✅ Good: Limit permissions
permissions:
  contents: read
  pull-requests: write
```

### Performance
```yaml
# ✅ Good: Use caching
- uses: actions/setup-node@v4
  with:
    cache: 'npm'

# ✅ Good: Run jobs in parallel
jobs:
  test:
    # independent job
  lint:
    # independent job
  
# ✅ Good: Use matrix for multiple versions
strategy:
  matrix:
    node: [16, 18, 20]

# ✅ Good: Fail fast
strategy:
  fail-fast: true
  matrix:
    os: [ubuntu, windows, macos]
```

### Maintenance
```yaml
# ✅ Good: Add timeout
jobs:
  build:
    timeout-minutes: 10

# ✅ Good: Continue on error for optional steps
- name: Optional step
  continue-on-error: true

# ✅ Good: Use concurrency control
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

# ✅ Good: Add conditions
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
```

---

**Pro Tip:** Use matrix builds for testing across multiple versions, cache dependencies to speed up workflows, pin action versions for reproducibility, and use environments with protection rules for production deployments!