# CI/CD Project README

## Overview

This is a CI/CD project designed to provide a seamless development and deployment workflow for a full-stack application. The project uses a Git-based branching model with automated testing and deployment pipelines set up using GitHub Actions.

## Workflow Overview

### 1. **Feature Development and Pull Requests**
   - When adding new features or making changes to the application, developers should work on their respective branches.
   - All new changes should be merged into the **`develop`** branch before being moved to **`main`** for deployment.

### 2. **GitHub Actions for Testing**
   - Every time a **Pull Request (PR)** is created to the **`develop`** branch, GitHub Actions will trigger Cypress component tests to ensure the new code does not break existing functionality.
   - You can view the test results directly on GitHub Actions. If the tests pass, you can merge the PR into the **`develop`** branch.

### 3. **Merging to `main` Branch**
   - Once the code has been successfully merged into **`develop`**, it is ready for deployment.
   - Pushing the code from the **`develop`** branch to the **`main`** branch triggers another GitHub Action, which will automatically deploy the application to **Render**.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Branching Strategy](#branching-strategy)
3. [GitHub Actions](#github-actions)
4. [Testing](#testing)
5. [Deployment](#deployment)
6. [Contributing](#contributing)

---

## Prerequisites

Before starting, ensure you have the following tools installed:

- **Git**: To manage your code versions.
- **Node.js**: To install and run Cypress tests locally (optional).
- **Render Account**: For deployment.
- **GitHub Account**: To access repositories, create pull requests, and monitor CI/CD pipelines.

## Branching Strategy

### `develop` Branch
- All new feature development and code changes should be pushed to feature branches.
- Pull Requests (PRs) to **`develop`** should always be reviewed and passed with successful tests before merging.

### `main` Branch
- This is the production-ready branch that will automatically trigger the deployment process.
- Only merge code into **`main`** when it is ready for deployment.

## GitHub Actions

### Cypress Component Tests
- When a **Pull Request** is opened or updated to the **`develop`** branch, a GitHub Action will automatically run to execute the Cypress component tests.
- These tests check for any regressions or issues with the new changes.
- You can view the status of the tests on the **Actions** tab of the GitHub repository.

### Deploy to Render
- Once the code has been successfully merged into **`develop`** and pushed to **`main`**, another GitHub Action is triggered that automatically deploys the application to **Render**.

### GitHub Actions Configuration Example

```yaml
name: CI/CD Pipeline

on:
  pull_request:
    branches:
      - develop
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Run Cypress tests
        run: npm run test:cypress

  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy to Render
        run: |
          # Deploy code to Render
          curl -X POST https://api.render.com/deploy \
            -H "Authorization: Bearer ${{ secrets.RENDER_API_KEY }}" \
            -d "service_id=YOUR_SERVICE_ID"
```

## Testing

### Cypress Component Tests
- Cypress tests will be executed automatically when a pull request is created against the **`develop`** branch.
- You can manually run the Cypress tests on your local machine using the following commands:
  ```bash
  npm install
  npm run test:cypress
  ```

### GitHub Actions Test Results
- After running Cypress tests, GitHub Actions will display the results.
- If all tests pass, you can proceed with merging the PR into the **`develop`** branch.

## Deployment

### Automatic Deployment
- Once code is merged into **`develop`**, and subsequently pushed to **`main`**, the deployment to **Render** is triggered automatically.
- Ensure you have set up the **Render** API key in the GitHub Secrets to authenticate the deployment action.
  
### Deployment Process

1. Merge code from feature branch into **`develop`**.
2. If all tests pass, push the changes from **`develop`** to **`main`**.
3. GitHub Action automatically deploys the application to **Render** once the push to **`main`** is detected.

## Contributing

We welcome contributions to this repository. To contribute:

1. Fork the repository and create your feature branch.
2. Ensure that your code passes Cypress tests before submitting a Pull Request to the **`develop`** branch.
3. Follow the process described above for testing, review, and deployment.

