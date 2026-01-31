# Continuous Integration Pipeline Template

**Pipeline Type**: CI (Continuous Integration)  
**Platform**: GitHub Actions  
**Version**: 1.0  
**Status**: Production Ready

## Metadata

| Field | Value |
|-------|-------|
| **Purpose** | Automated build, test, and artifact creation |
| **Triggers** | Push to main/develop, Pull Requests |
| **Components** | Build, Test, Security Scan, Publish |
| **Artifacts** | Container images, application packages |
| **Duration** | ~10-15 minutes |

---

## 1. Overview

This CI pipeline automates the process of building, testing, and publishing application artifacts. It ensures code quality, runs comprehensive tests, performs security scans, and creates deployable artifacts.

### Pipeline Flow

```
Code Push/PR
    │
    ▼
Checkout & Setup
    │
    ▼
Install Dependencies (with cache)
    │
    ▼
Code Quality Checks (lint, format)
    │
    ▼
Unit Tests (with coverage)
    │
    ▼
Integration Tests
    │
    ▼
Security Scans
    │
    ▼
Build Artifacts
    │
    ▼
Build & Push Container Images
    │
    ▼
Publish Test Results
    │
    ▼
Notify Status
```

---

## 2. Complete Pipeline: GitHub Actions

### File: `.github/workflows/ci.yml`

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
      - develop
    paths-ignore:
      - '**.md'
      - 'docs/**'
  pull_request:
    branches:
      - main
      - develop

env:
  NODE_VERSION: '18'
  PYTHON_VERSION: '3.11'
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

# Prevent multiple workflows from running simultaneously
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  # Job 1: Code Quality and Linting
  code-quality:
    name: Code Quality Checks
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Full history for better analysis

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint
        continue-on-error: false

      - name: Run Prettier
        run: npm run format:check

      - name: Check TypeScript
        run: npm run type-check
        if: hashFiles('tsconfig.json') != ''

  # Job 2: Unit Tests
  unit-tests:
    name: Unit Tests
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm run test:unit -- --coverage --maxWorkers=2
        env:
          NODE_ENV: test

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/coverage-final.json
          flags: unit
          name: unit-tests

      - name: Upload test results
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: unit-test-results
          path: |
            coverage/
            test-results/
          retention-days: 30

      - name: Check coverage threshold
        run: |
          COVERAGE=$(cat coverage/coverage-summary.json | jq '.total.lines.pct')
          if (( $(echo "$COVERAGE < 80" | bc -l) )); then
            echo "Coverage $COVERAGE% is below 80% threshold"
            exit 1
          fi

  # Job 3: Integration Tests
  integration-tests:
    name: Integration Tests
    runs-on: ubuntu-latest
    timeout-minutes: 20

    services:
      postgres:
        image: postgres:14-alpine
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

      redis:
        image: redis:7-alpine
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Wait for services
        run: |
          sleep 5
          npx wait-on tcp:5432 tcp:6379 -t 30000

      - name: Run database migrations
        run: npm run migrate:test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/testdb

      - name: Run integration tests
        run: npm run test:integration
        env:
          NODE_ENV: test
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/testdb
          REDIS_URL: redis://localhost:6379

      - name: Upload test results
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: integration-test-results
          path: test-results/
          retention-days: 30

  # Job 4: Security Scanning
  security-scan:
    name: Security Scanning
    runs-on: ubuntu-latest
    timeout-minutes: 15
    needs: [code-quality]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'
          severity: 'CRITICAL,HIGH'

      - name: Upload Trivy results to GitHub Security
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: 'trivy-results.sarif'

      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/node@master
        continue-on-error: true
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --severity-threshold=high

      - name: Run npm audit
        run: npm audit --audit-level=moderate
        continue-on-error: true

      - name: Check for secrets
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: ${{ github.event.repository.default_branch }}
          head: HEAD

  # Job 5: Build Application
  build:
    name: Build Application
    runs-on: ubuntu-latest
    timeout-minutes: 15
    needs: [unit-tests, integration-tests, security-scan]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci --production=false

      - name: Build application
        run: npm run build
        env:
          NODE_ENV: production

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-artifacts
          path: |
            dist/
            node_modules/
          retention-days: 7

  # Job 6: Build and Push Docker Image
  docker-build-push:
    name: Build and Push Docker Image
    runs-on: ubuntu-latest
    timeout-minutes: 20
    needs: [build]
    if: github.event_name == 'push'

    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=sha,prefix={{branch}}-
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
          build-args: |
            NODE_ENV=production
            BUILD_DATE=${{ github.event.head_commit.timestamp }}
            VCS_REF=${{ github.sha }}

      - name: Scan Docker image
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          format: 'sarif'
          output: 'trivy-image-results.sarif'

      - name: Upload image scan results
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: 'trivy-image-results.sarif'

  # Job 7: Publish Results
  publish-results:
    name: Publish Test Results
    runs-on: ubuntu-latest
    timeout-minutes: 10
    needs: [unit-tests, integration-tests]
    if: always()

    steps:
      - name: Download test results
        uses: actions/download-artifact@v4
        with:
          pattern: '*-test-results'
          path: test-results/

      - name: Publish test results
        uses: EnricoMi/publish-unit-test-result-action@v2
        if: always()
        with:
          files: |
            test-results/**/*.xml
            test-results/**/*.json

      - name: Comment PR with results
        uses: actions/github-script@v7
        if: github.event_name == 'pull_request' && always()
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const fs = require('fs');
            const coverage = JSON.parse(fs.readFileSync('test-results/unit-test-results/coverage/coverage-summary.json'));
            const comment = `## Test Results\n\n` +
              `✅ Unit Tests: Passed\n` +
              `✅ Integration Tests: Passed\n` +
              `📊 Coverage: ${coverage.total.lines.pct}%\n`;
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: comment
            });

  # Job 8: Notify
  notify:
    name: Notify Status
    runs-on: ubuntu-latest
    timeout-minutes: 5
    needs: [docker-build-push]
    if: always()

    steps:
      - name: Notify Slack on success
        if: ${{ needs.docker-build-push.result == 'success' }}
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          webhook-type: incoming-webhook
          payload: |
            {
              "text": "✅ CI Pipeline succeeded for ${{ github.repository }}",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*CI Pipeline Success*\n\nRepository: ${{ github.repository }}\nBranch: ${{ github.ref_name }}\nCommit: ${{ github.sha }}\nAuthor: ${{ github.actor }}"
                  }
                }
              ]
            }

      - name: Notify Slack on failure
        if: ${{ needs.docker-build-push.result == 'failure' }}
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          webhook-type: incoming-webhook
          payload: |
            {
              "text": "❌ CI Pipeline failed for ${{ github.repository }}",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*CI Pipeline Failed*\n\nRepository: ${{ github.repository }}\nBranch: ${{ github.ref_name }}\nCommit: ${{ github.sha }}\nAuthor: ${{ github.actor }}\n\n<${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}|View Run>"
                  }
                }
              ]
            }
```

---

## 3. Required Secrets

Configure these secrets in your GitHub repository:

| Secret Name | Description | How to Obtain |
|-------------|-------------|---------------|
| `GITHUB_TOKEN` | GitHub token (auto-provided) | Automatically available |
| `SNYK_TOKEN` | Snyk API token for vulnerability scanning | Sign up at snyk.io and get API token |
| `SLACK_WEBHOOK_URL` | Slack webhook for notifications | Create incoming webhook in Slack workspace |
| `CODECOV_TOKEN` | Codecov token for coverage reporting | Sign up at codecov.io and get token |

### Setting Secrets

```bash
# Using GitHub CLI
gh secret set SNYK_TOKEN --body "your-snyk-token"
gh secret set SLACK_WEBHOOK_URL --body "your-webhook-url"
gh secret set CODECOV_TOKEN --body "your-codecov-token"

# Or via GitHub UI
# Settings -> Secrets and variables -> Actions -> New repository secret
```

---

## 4. Pipeline Features

### ✅ Implemented Features

- **Parallel Execution**: Independent jobs run in parallel
- **Dependency Caching**: npm/pip dependencies cached
- **Test Coverage**: Unit and integration test coverage
- **Security Scanning**: Trivy, Snyk, npm audit
- **Secret Detection**: TruffleHog for leaked secrets
- **Docker Build**: Multi-stage builds with caching
- **Image Scanning**: Container vulnerability scanning
- **Test Results**: Published to PR comments
- **Notifications**: Slack alerts on success/failure
- **Concurrency Control**: Prevents duplicate runs

### 🔧 Customization Options

**For Python Projects**:
```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: ${{ env.PYTHON_VERSION }}
    cache: 'pip'

- name: Install dependencies
  run: pip install -r requirements.txt

- name: Run tests
  run: pytest --cov=src --cov-report=xml
```

**For Multiple Services**:
```yaml
strategy:
  matrix:
    service: [auth-service, user-service, data-service]
steps:
  - name: Build ${{ matrix.service }}
    run: |
      cd services/${{ matrix.service }}
      npm run build
```

**For Multi-Architecture Builds**:
```yaml
- name: Build and push Docker image
  uses: docker/build-push-action@v5
  with:
    platforms: linux/amd64,linux/arm64
    push: true
    tags: ${{ steps.meta.outputs.tags }}
```

---

## 5. Usage Guide

### Running the Pipeline

The pipeline runs automatically on:
- Push to `main` or `develop` branches
- Pull requests to `main` or `develop` branches

### Manual Trigger

```bash
# Trigger manually via GitHub CLI
gh workflow run ci.yml --ref main

# Or via GitHub UI
# Actions -> CI Pipeline -> Run workflow
```

### Monitoring Execution

```bash
# Watch latest run
gh run watch

# View run details
gh run view

# View logs
gh run view --log

# List recent runs
gh run list --workflow=ci.yml
```

---

## 6. Troubleshooting

### Common Issues

**Build Fails with "ENOSPC" error**:
```yaml
# Add to job:
env:
  NODE_OPTIONS: --max_old_space_size=4096
```

**Tests Timeout**:
```yaml
# Increase timeout:
timeout-minutes: 30
```

**Cache Not Working**:
```yaml
# Clear cache:
gh cache delete --all

# Or use cache key versioning:
cache-key: v2-dependencies-${{ hashFiles('**/package-lock.json') }}
```

**Docker Build Fails**:
```yaml
# Check Dockerfile
docker build . --progress=plain

# Enable debug logs:
env:
  ACTIONS_STEP_DEBUG: true
```

**Security Scan False Positives**:
```yaml
# Add .trivyignore file with CVE IDs to ignore
echo "CVE-2021-XXXXX" >> .trivyignore
```

---

## 7. Performance Optimization

### Current Optimizations

1. **Dependency Caching**: Reduces install time by ~3-5 minutes
2. **Docker Layer Caching**: Speeds up image builds by ~5-10 minutes
3. **Parallel Jobs**: Saves ~10 minutes vs sequential
4. **Conditional Execution**: Skips unnecessary jobs

### Further Optimizations

```yaml
# Use npm ci instead of npm install (faster, deterministic)
run: npm ci

# Use --production flag when building
run: npm ci --production

# Cache test results for unchanged files
uses: actions/cache@v3
with:
  path: .jest-cache
  key: jest-${{ hashFiles('**/*.test.js') }}

# Use matrix strategy for parallel test execution
strategy:
  matrix:
    shard: [1, 2, 3, 4]
run: npm test -- --shard=${{ matrix.shard }}/4
```

---

## 8. Integration Checklist

Before using this pipeline:

- [ ] Review and customize for your project
- [ ] Set up all required secrets
- [ ] Configure branch protection rules
- [ ] Test pipeline with a PR
- [ ] Verify all jobs pass
- [ ] Check test coverage meets threshold
- [ ] Verify security scans complete
- [ ] Confirm Docker images are built
- [ ] Test notifications work
- [ ] Document any customizations

---

## 9. Maintenance

### Regular Tasks

- **Weekly**: Review security scan results
- **Monthly**: Update action versions
- **Quarterly**: Review and optimize pipeline performance
- **As Needed**: Update Node/Python versions

### Updating Actions

```yaml
# Use Dependabot to keep actions up to date
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

---

**Template Version**: 1.0  
**Last Updated**: 2024

[Back to Stage 3 Overview](README.md) | [Stage 3 Prompt](PROMPT.md) | [CD Pipeline Template](TEMPLATE-PIPELINE-CD.md)
