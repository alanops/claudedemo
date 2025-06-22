# CLAUDE.md - Project Guide

## Project Overview

This is a demonstration project showcasing the migration from Jenkins CI/CD pipelines to GitHub Actions. The codebase contains a simple Node.js application with Docker containerization and Kubernetes deployment configurations.

## Technology Stack

- **Language**: JavaScript/Node.js
- **Runtime**: Node.js 18 (Alpine Linux in Docker)
- **CI/CD**: Jenkins (current), GitHub Actions (migration target)
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Code Quality**: SonarQube
- **Package Manager**: npm

## Project Structure

```
claudedemo/
├── README.md                    # Main project README
└── jenkins-to-github-demo/      # Demo application
    ├── Dockerfile               # Docker container definition
    ├── Jenkinsfile             # Jenkins pipeline configuration
    ├── README.md               # Demo-specific documentation
    ├── package.json            # Node.js project configuration
    ├── sonar-project.properties # SonarQube configuration
    └── k8s/                    # Kubernetes manifests
        └── staging/
            ├── deployment.yaml  # K8s deployment configuration
            └── service.yaml     # K8s service configuration
```

## Common Commands

### Building the Application

```bash
# Install dependencies
npm install

# Build the application
npm run build
# Note: Creates a dist/ directory with app.js
```

### Testing

```bash
# Run tests
npm test
# Note: Generates test-results.xml in JUnit format
```

### Linting

```bash
# Run code linting
npm run lint
# Note: Currently a placeholder command
```

### Running the Application

```bash
# Start the application (requires build first)
npm start
# Runs: node dist/app.js
```

### Docker Operations

```bash
# Build Docker image
docker build -t myapp:latest .

# Run Docker container
docker run -p 3000:3000 myapp:latest
```

### Kubernetes Deployment

```bash
# Apply Kubernetes configurations to staging
kubectl apply -f k8s/staging/

# Update deployment with new image
kubectl set image deployment/myapp myapp=myapp:TAG -n staging
```

### SonarQube Analysis

```bash
# Run SonarQube scanner (requires configuration)
sonar-scanner
```

## High-Level Architecture

### Application Architecture

The project follows a simple Node.js application structure:

1. **Build Phase**: Creates a distributable version in the `dist/` directory
2. **Test Phase**: Runs unit tests and generates JUnit-compatible test results
3. **Package Phase**: Containerizes the application using Docker
4. **Deploy Phase**: Deploys to Kubernetes staging environment

### CI/CD Pipeline Architecture

The current Jenkins pipeline implements a multi-stage process:

1. **Checkout**: Retrieves source code from SCM
2. **Build**: Installs dependencies and builds the application
3. **Test** (Parallel execution):
   - Unit Tests: Runs npm tests and publishes results
   - SonarQube Analysis: Performs code quality analysis
4. **Docker Build**: Creates and pushes Docker images to registry
5. **Deploy to Staging**: Applies Kubernetes manifests (main branch only)
6. **Post-build**: Cleanup and failure notifications

### Deployment Architecture

The application is deployed to Kubernetes with:

- **Deployment**: 3 replicas with resource limits
- **Service**: LoadBalancer type exposing port 80
- **Namespace**: Staging environment
- **Container**: Node.js application on port 3000

### Key Configuration Files

1. **package.json**: Defines npm scripts and dependencies
2. **Jenkinsfile**: Jenkins pipeline configuration
3. **Dockerfile**: Multi-stage Docker build
4. **sonar-project.properties**: SonarQube analysis settings
5. **k8s/**: Kubernetes deployment manifests

## Migration Context

This project is designed to demonstrate converting from Jenkins to GitHub Actions, maintaining:
- Parallel test execution
- Docker image building and registry push
- Conditional deployment based on branch
- Post-build notifications
- Code quality analysis integration

## Environment Variables

- `NODE_ENV`: Set to "staging" in Kubernetes deployment
- `DOCKER_IMAGE`: Base image name (myapp)
- `DOCKER_TAG`: Build number for versioning
- `SONAR_TOKEN`: Authentication for SonarQube

## Notes

- The application uses placeholder build/test scripts for demonstration
- Docker images are pushed to Docker Hub (requires credentials)
- Kubernetes deployment targets a staging namespace
- Email notifications are configured for build failures