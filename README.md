# CI Pipeline with Checkstyle, Snyk, SonarQube, JFrog & Docker

This repository demonstrates an enhanced CI pipeline using **GitHub Actions**.  
The existing pipeline was extended to include:

- **Checkstyle** (code quality / linting)
- **JaCoCo** (test coverage)
- **SonarQube** (code quality & coverage analysis)
- **Snyk** (security scanning)
- **JFrog Artifactory** (artifact storage)
- **Docker** (container build & push)

The pipeline is implemented using a **reusable workflow template** and a **consumer workflow**.

---

## 📌 Workflow Architecture

### 1️⃣ Reusable Workflow (Template)
**File:**  
`.github/workflows/docker-ci-template.yml`

This workflow contains all CI logic and is reused by multiple repositories.

### 2️⃣ Consumer Workflow
**File:**  
`.github/workflows/consumer-ci.yml`

This workflow simply calls the reusable template and passes required inputs and secrets.

---

## 🔄 Pipeline Execution Flow

The pipeline runs on:
- `push` to `main`
- `pull_request` to `main`

### Step-by-step execution:

### 1. Checkout & Setup
- Checks out source code
- Sets up **Java 17**
- Caches Maven dependencies

---

### 2. Checkstyle (Code Scanning)
- Runs Checkstyle using **Google style rules**
- Generates a Checkstyle report
- Posts error count as a **PR comment**

📂 Artifact:
- `checkstyle-report`

---

### 3. Tests & Code Coverage
- Runs unit tests
- Generates **JaCoCo coverage report**
- Calculates coverage percentage
- Posts coverage to PR

📂 Artifact:
- `jacoco-report`

---

### 4. SonarQube Analysis (Optional)
- Runs SonarQube scan if token is provided
- Fetches coverage from SonarQube API
- Posts coverage result to PR

📊 Dashboard:
- SonarQube Project Dashboard

---

### 5. Build Application
- Builds the application using Maven
- Generates executable JAR

---

### 6. Snyk Security Scan
- Scans:
  - `pom.xml` (dependencies)
  - Dockerfile
  - Kubernetes manifests
  - Code
- Identifies vulnerabilities by severity

📊 Dashboard:
- **Snyk → Projects**

---

### 7. Publish JAR to JFrog
- Configures Maven authentication
- Publishes artifacts to **JFrog Artifactory**

📦 Repository:
- `libs-snapshot-local`

Stored files:
- `.jar`
- `.pom`
- `maven-metadata.xml`
- SBOM files (CycloneDX)

---

### 8. Docker Build & Push
- Builds Docker image using generated JAR
- Pushes image to Docker registry

🐳 Image:
- `${image_name}:${image_tag}`

---

## 📦 Artifacts Generated

| Artifact | Purpose |
|--------|--------|
| checkstyle-report | Code style violations |
| jacoco-report | Test coverage |
| Docker Image | Deployable container |
| JAR file | Application artifact |

Artifacts are available under **GitHub Actions → Run → Artifacts**.

---

## 🔐 Secrets & Variables Used

### GitHub Secrets
| Name | Purpose |
|----|----|
| `DOCKER_USERNAME` | Docker login |
| `DOCKER_PASSWORD` | Docker login |
| `JFROG_USER` | Artifactory auth |
| `JFROG_PASSWORD` | Artifactory auth |

### GitHub Variables
| Name | Purpose |
|----|----|
| `SONAR_PROJECT_KEY` | Sonar project |
| `SONAR_HOST_URL` | Sonar server |
| `SONAR_ORGANIZATION` | Sonar org |
| `SONAR_TOKEN` | Sonar auth |
| `SNYK_TOKEN` | Snyk auth |

---

## ✅ Summary

This CI pipeline ensures:
- Code quality validation
- Security scanning
- Test coverage reporting
- Artifact versioning
- Dockerized delivery

All checks run automatically on every PR and push, providing early feedback and consistent build quality.
