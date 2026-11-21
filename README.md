# Jenkins DevSecOps CI/CD Pipeline

An automated DevSecOps pipeline designed to integrate security practices into the software delivery lifecycle using **Jenkins, SonarQube, Trivy, OWASP Dependency-Check, Docker, and Kubernetes**.

This project demonstrates how application code can be automatically checked for code quality, dependency vulnerabilities, and container image vulnerabilities before proceeding through the deployment workflow.

The objective is to implement a security-first CI/CD process where security validation is integrated into application build and delivery rather than treated as a separate activity.

## Architecture

The pipeline follows a continuous integration and continuous deployment workflow.

### Pipeline Architecture

```text
             Developer
                 |
                 v
             GitHub
                 |
                 v
             Jenkins
                 |
                 v
          Source Checkout
                 |
                 v
          Trivy FS Scan
                 |
                 v
       OWASP Dependency Check
                 |
                 v
        SonarQube Analysis
                 |
                 v
          Quality Gate
                 |
                 v
          Docker Build
                 |
                 v
         Docker Image Scan
                 |
                 v
         Docker Registry
                 |
                 v
         Deployment Job
                 |
                 v
         Kubernetes / EKS
                 |
                 v
          Application
```

*The diagram represents the intended pipeline flow. Keep only the stages and deployment components implemented in your actual Jenkins and deployment configurations.*

## Technology Stack

| Technology | Purpose |
|---|---|
| Jenkins | CI/CD pipeline automation |
| Git & GitHub | Source code management |
| SonarQube | Static code analysis and quality gates |
| Trivy | Filesystem and container image vulnerability scanning |
| OWASP Dependency-Check | Third-party dependency vulnerability analysis |
| Docker | Application containerization |
| Docker Hub / Container Registry | Docker image storage |
| Kubernetes | Container orchestration |
| AWS EKS | Managed Kubernetes deployment, if configured |
| Argo CD | GitOps-based deployment, if configured |
| AWS | Cloud infrastructure |

## Key Features

- **Automated CI/CD:** Jenkins automates application build and delivery workflows.
- **Static Application Security Testing (SAST):** SonarQube analyzes source code for bugs, vulnerabilities and code quality issues.
- **Software Composition Analysis (SCA):** OWASP Dependency-Check identifies known vulnerabilities in third-party dependencies.
- **Filesystem Security Scanning:** Trivy scans project files for potential security vulnerabilities and misconfigurations.
- **Container Image Scanning:** Trivy checks Docker images for known vulnerabilities.
- **Quality Gate Integration:** SonarQube quality gate validation helps prevent code that fails configured quality criteria from progressing.
- **Containerization:** Docker packages the application and its dependencies into a portable image.
- **Automated Image Publishing:** The pipeline publishes the built image to a container registry.
- **Separate Deployment Workflow:** A dedicated deployment job handles the application delivery process.
- **Kubernetes Integration:** Kubernetes provides the container orchestration environment for deployment.

## Pipeline Workflow

The pipeline is organized into multiple stages.

### 1. Source Code Checkout

Jenkins retrieves application source code from the GitHub repository.

### 2. Filesystem Security Scan

Trivy scans the project filesystem to identify security issues and vulnerabilities in supported files and dependencies.

### 3. Dependency Vulnerability Analysis

OWASP Dependency-Check analyzes third-party libraries and dependencies against known vulnerability information.

### 4. SonarQube Code Analysis

SonarQube performs static code analysis to identify:

- Bugs
- Code smells
- Security vulnerabilities
- Maintainability issues

### 5. SonarQube Quality Gate

The pipeline evaluates the SonarQube Quality Gate result to determine whether the code meets the configured quality requirements.

### 6. Docker Image Build

After the required validation stages, Jenkins builds a Docker image containing the application and its runtime dependencies.

### 7. Docker Image Vulnerability Scan

Trivy scans the generated Docker image to identify vulnerabilities in operating system packages and application dependencies.

### 8. Docker Image Publishing

The validated Docker image is tagged and pushed to the configured container registry.

### 9. Deployment Pipeline

Jenkins triggers a separate deployment job to handle application deployment.

The deployment workflow integrates with the configured Kubernetes environment and associated deployment tooling.

### 10. Application Deployment

The application is deployed using the configured Kubernetes deployment workflow.

## Security Integration

Security is incorporated at multiple stages of the CI/CD pipeline.

| Security Tool | Security Function |
|---|---|
| Trivy | Filesystem vulnerability scanning |
| OWASP Dependency-Check | Third-party dependency vulnerability detection |
| SonarQube | Static code analysis |
| SonarQube Quality Gate | Code quality validation |
| Trivy Image Scan | Container image vulnerability detection |

This approach helps identify security issues earlier in the development lifecycle and reduces the possibility of deploying vulnerable application builds.

## Project Structure

```text
Jenkins-DevSecOps-Pipeline/
│
├── Jenkinsfile
│
├── Dockerfile
│
├── docker-compose.yml
│
├── scripts/
│   ├── deployment.sh
│   └── setup.sh
│
├── kubernetes/
│   └── deployment configurations
│
├── README.md
│
└── .gitignore
```

*Update this example to reflect the actual files and folders in your repository.*

## Prerequisites

To reproduce the pipeline, the following tools and configurations are required:

- Linux environment or compatible Jenkins agent
- Jenkins with required plugins
- Git and GitHub repository access
- Docker Engine
- SonarQube server
- Trivy
- OWASP Dependency-Check
- Docker registry account
- Kubernetes cluster and kubectl, if using Kubernetes deployment
- AWS CLI and EKS access, if deploying to Amazon EKS

## Jenkins Configuration

Before running the pipeline, configure the required tools and credentials in Jenkins.

### Required Configuration

1. Configure GitHub repository access.
2. Configure SonarQube server integration.
3. Configure the SonarQube authentication token.
4. Configure Docker registry credentials.
5. Install and configure Trivy.
6. Configure OWASP Dependency-Check.
7. Configure Kubernetes access for the deployment job, if applicable.
8. Configure the deployment job and its required parameters.

Sensitive credentials should be stored in the Jenkins Credentials Store rather than hardcoded in the Jenkinsfile or shell scripts.

## Running the Pipeline

### 1. Clone the repository

```bash
git clone https://github.com/akhilesh-singh-official/Jenkins-DevSecOps-Pipeline.git

cd Jenkins-DevSecOps-Pipeline
```

### 2. Configure Jenkins

Set up Jenkins and configure the required plugins, tools, servers and credentials.

### 3. Configure the pipeline job

Create a Jenkins Pipeline job and connect it to your GitHub repository.

Configure the job to use the Jenkinsfile stored in the repository.

### 4. Execute the pipeline

Trigger the Jenkins pipeline manually or through the configured source control integration.

### 5. Monitor execution

Monitor the Jenkins console output to inspect:

- Build status
- Security scan results
- SonarQube analysis results
- Quality Gate status
- Docker image build and scan results
- Deployment job execution

## Security Best Practices

The project also provides an opportunity to implement important DevSecOps security practices:

- Store credentials securely using Jenkins Credentials.
- Avoid hardcoding AWS instance IDs and access credentials.
- Use least-privilege IAM permissions.
- Avoid copying sensitive environment files into Docker images.
- Scan container images before publishing.
- Use versioned Docker image tags rather than relying only on `latest`.
- Restrict Kubernetes access using appropriate RBAC permissions.
- Configure security gates based on vulnerability severity.
- Keep Jenkins plugins and scanning tools updated.

## Future Improvements

Planned enhancements to improve the pipeline:

- Integrate additional automated unit and integration tests.
- Implement stronger vulnerability-based pipeline failure policies.
- Add automated deployment rollback.
- Introduce image signing and verification.
- Implement centralized monitoring using Prometheus and Grafana.
- Improve Jenkins agent isolation and permissions.
- Introduce automated infrastructure provisioning using Terraform.
- Implement GitOps-based deployment improvements.
- Add pipeline notifications and deployment reports.

## What I Learned

Through this project, I gained practical experience with:

- Building declarative Jenkins pipelines.
- Integrating security scanning into CI/CD workflows.
- Performing static code analysis using SonarQube.
- Identifying dependency vulnerabilities using OWASP Dependency-Check.
- Scanning filesystems and Docker images using Trivy.
- Building and publishing Docker images through Jenkins.
- Managing Jenkins credentials and pipeline configuration.
- Understanding automated application deployment workflows.
- Integrating CI/CD with container orchestration technologies.

## Author

**Akhilesh Singh**

DevOps | Cloud Infrastructure | CI/CD | DevSecOps

GitHub: [akhilesh-singh-official](https://github.com/akhilesh-singh-official)

---

*This project is part of my hands-on DevOps journey, focused on implementing automated CI/CD pipelines and integrating security practices into modern application delivery workflows.*
