## CI/CD Pipeline using GitHub, Jenkins, SonarQube and Docker
### Overview
- A **CI/CD pipeline** automates the process of building, testing, and deploying applications.
- In this setup the workflow is:
  - Developer → GitHub → Jenkins → SonarQube → Docker → Application Deployment
- Steps:
  1. Developer pushes code to GitHub.
  2. Jenkins pulls the code automatically.
  3. SonarQube performs static code analysis.
  4. If the code passes quality checks, it is deployed as a Docker container.
  5. The application becomes accessible through a browser.

- Three **AWS EC2 instances** are used:

| Server           | Purpose                   |
| ---------------- | ------------------------- |
| Jenkins Server   | CI/CD pipeline automation |
| SonarQube Server | Code quality analysis     |
| Docker Server    | Application deployment    |

## Step 1 – Push Code to GitHub
- The developer pushes the application code to a **GitHub repository**.
- GitHub acts as the **source code management system**.

## Step 2 – Jenkins Setup
- Jenkins is used to automate the pipeline.

### Installation
- Jenkins requires **Java Runtime Environment**.
- Example installation:
    ```bash
    sudo apt update
    sudo apt install openjdk-11-jre
    ```
- Install Jenkins and start the service.
- Access Jenkins through browser:
    ```
    http://<EC2-IP>:8080
    ```

## Step 3 – Configure Jenkins Pipeline
- Create a **Freestyle Project** or Pipeline.
- Configure:
  - Source Code Management → Git
  - Repository URL, Branch

## Step 4 – GitHub Webhook Integration
- To automate builds when code changes:
- GitHub → Settings → Webhooks
- Webhook URL:
    ```
    http://<jenkins-ip>:8080/github-webhook/
    ```
- Now whenever code is pushed:
  * Jenkins automatically triggers the pipeline.

## Step 5 – SonarQube Setup
- SonarQube is used for **static code analysis**.
- It checks:
  * Bugs
  * Vulnerabilities
  * Code smells
  * Code quality metrics
- Install SonarQube on another EC2 instance.
- Access SonarQube:
    ```
    http://<sonarqube-ip>:9000
    ```

## Step 6 – SonarQube Integration with Jenkins
- Install Jenkins plugins:
  * SonarQube Scanner
  * SonarQube Plugin
- Configure:
    ```
    Manage Jenkins → Configure System
    ```
- Add SonarQube server details:
    ```
    SonarQube URL
    Authentication Token
    ```
- During pipeline execution Jenkins sends code to SonarQube for scanning.

## Step 7 – Docker Setup
- Docker is used to **package the application into containers**.
- Install Docker on the deployment server.
- Example commands:
    ```bash
    sudo apt update
    sudo apt install docker.io
    ```
- Verify installation:
    ```bash
    docker --version
    ```

## Step 8 – Create Dockerfile
- A Dockerfile defines how the container image is built.
- Example:
    ```dockerfile
    FROM nginx
    COPY . /usr/share/nginx/html
    ```
- This means:
  * Use Nginx image
  * Copy website files into Nginx directory.

## Step 9 – Deploy Application with Docker
- Build Docker image:
    ```bash
    docker build -t mywebsite .
    ```
- Run container:
    ```bash
    docker run -d -p 8085:80 mywebsite
    ```
- Application becomes available at:
    ```
    http://<docker-server-ip>:8085
    ```

## Step 10 – Pipeline Workflow Summary
- Complete automated workflow:
    ```
    Developer
       ↓
    GitHub Repository
       ↓
    Jenkins Pipeline
       ↓
    SonarQube Code Analysis
       ↓
    Docker Image Build
       ↓
    Docker Container Deployment
       ↓
    Application Accessible via Browser
    ```

## Benefits of this CI/CD Pipeline
### Automation
- Code build, testing, and deployment happen automatically.

### Faster Development
- Developers can deploy features quickly.

### Code Quality
- SonarQube ensures high code quality.

### Consistency
- Docker ensures the application runs the same everywhere.

### Continuous Integration
- New code changes are automatically integrated and tested.

## Key DevOps Tools Used

| Tool      | Purpose                 |
| --------- | ----------------------- |
| GitHub    | Source code repository  |
| Jenkins   | CI/CD automation server |
| SonarQube | Static code analysis    |
| Docker    | Containerization        |
| AWS EC2   | Cloud infrastructure    |

## Simple CI/CD Flow Diagram
```
Developer
   ↓
GitHub
   ↓
Webhook Trigger
   ↓
Jenkins Pipeline
   ↓
SonarQube Scan
   ↓
Docker Build
   ↓
Docker Container
   ↓
Application Deployment
```