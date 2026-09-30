# CI/CD — Continuous Integration & Continuous Delivery/Deployment

## What is CI/CD?

**CI/CD** is a set of practices that automate how code goes from a developer's machine to production. Instead of building, testing, and releasing software by hand, a pipeline does it automatically every time code changes.

- **CI (Continuous Integration)** — developers merge code into a shared repository frequently (several times a day). Each merge automatically triggers a build and runs tests, so bugs are caught early.
- **CD (Continuous Delivery)** — every change that passes the tests is automatically prepared for release. A human approves the final push to production.
- **CD (Continuous Deployment)** — goes one step further: every change that passes all tests is deployed to production automatically, with no manual approval.

```
Code  →  Build  →  Test  →  Package  →  Deploy  →  Monitor
 └──── CI ─────────────┘   └───────── CD ─────────────┘
```

## Why use it?

| Without CI/CD | With CI/CD |
|---|---|
| Manual builds and deployments | Fully automated pipeline |
| Bugs found late | Bugs found within minutes of a commit |
| "Works on my machine" | Consistent, reproducible environments |
| Rare, risky releases | Small, frequent, low-risk releases |
| Slow feedback | Fast feedback for developers |

## Stages of a typical pipeline

1. **Source** — a `git push` or pull request triggers the pipeline.
2. **Build** — compile code, install dependencies, build a Docker image.
3. **Test** — unit tests, integration tests, linting, security scans.
4. **Package / Artifact** — store the build output (Docker image, JAR, zip) in a registry.
5. **Deploy** — release to staging, then production (servers, containers, Kubernetes, cloud).
6. **Monitor** — track logs, metrics, and errors; roll back if something breaks.

## Popular CI/CD tools

| Tool | Notes |
|---|---|
| **GitHub Actions** | Built into GitHub; YAML workflows in `.github/workflows/` |
| **Jenkins** | Open-source, self-hosted, highly extensible (uses `Jenkinsfile`) |
| **GitLab CI/CD** | Built into GitLab (`.gitlab-ci.yml`) |
| **CircleCI** | Cloud-based, fast setup |
| **AWS CodePipeline** | Native AWS integration |
| **Argo CD** | GitOps-style continuous delivery for Kubernetes |

## Example: GitHub Actions workflow

Save as `.github/workflows/ci.yml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build project
        run: npm run build

      - name: Build Docker image
        run: docker build -t my-app:${{ github.sha }} .
```

## Example: Jenkinsfile

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'docker build -t my-app:${BUILD_NUMBER} .' }
        }
        stage('Test') {
            steps { sh 'docker run --rm my-app:${BUILD_NUMBER} npm test' }
        }
        stage('Deploy') {
            when { branch 'main' }
            steps { sh 'ansible-playbook -i inventory deploy.yml' }
        }
    }

    post {
        failure { echo 'Pipeline failed!' }
    }
}
```

## Best practices

- Commit small and often; keep the main branch always deployable.
- Keep builds fast — run quick tests first, slow ones later.
- Never hardcode secrets; use GitHub Secrets, Jenkins credentials, or a vault.
- Build once, deploy the same artifact to every environment.
- Use infrastructure as code (Terraform, Ansible) so environments are reproducible.
- Automate rollbacks and add monitoring/alerts after deployment.
- Fail fast: a broken pipeline should block the merge.

## Key terms

- **Pipeline** — the automated sequence of stages.
- **Artifact** — the output of a build (image, binary, package).
- **Runner / Agent** — the machine that executes pipeline jobs.
- **Trigger** — the event that starts a pipeline (push, PR, schedule).
- **Rollback** — reverting to the previous working version.
- **GitOps** — using Git as the single source of truth for infrastructure and deployments.

## Summary

CI/CD turns software delivery into a repeatable, automated process: **integrate often, test automatically, and release confidently.**
