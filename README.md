# Java Maven Docker App with Jenkins CI/CD

This project is a simple Java application built using Maven and integrated with Jenkins for a complete CI/CD pipeline. It includes:

- Java 21 Application
- Maven Build and Test
- JUnit Testing
- SonarQube Static Code Analysis
- Docker Image Build and Push
- Trivy Security Scan
- Jenkins Declarative Pipeline

Tool	Recommended Version
Java	21
Maven	3.8.5+
Jenkins	LTS
Docker	20.10+
SonarQube	9.x – 10.x
Trivy	Latest
---

## 📁 Project Structure

```
java-maven-docker-app/
├── src/
│   ├── main/java/com/example/App.java
│   └── test/java/com/example/AppTest.java
├── pom.xml
├── Dockerfile
├── Jenkinsfile
└── README.md
```

---

## ⚙️ Prerequisites

- Java 21
- Maven
- Docker
- Jenkins (with required plugins)
- SonarQube Server
- Trivy (optional for image scanning)

---

## 📦 Build with Maven

```bash
mvn clean package
```

This command compiles the Java source, runs tests, and packages the app into a `.jar`.

---

## ✅ Run Tests

```bash
mvn test
```

JUnit test located in `AppTest.java` verifies the `add()` method.

---

## 🐳 Docker Image

### Build Image

```bash
docker build -t java-maven-app .
```

### Run Container

```bash
docker run --rm java-maven-app
```

---

## 🔐 Trivy Scan

To scan the built Docker image for vulnerabilities:

```bash
trivy image java-maven-app
```

---

## 🚀 Jenkins Pipeline

### Jenkinsfile Overview

The pipeline includes:

1. **Checkout** – Get source from Git
2. **Maven Build** – Compile and package
3. **JUnit Tests** – Run unit tests
4. **JUnit Reporting** – Publish test results
5. **SonarQube Analysis** – Static code analysis
6. **Docker Build** – Build the image
7. **Trivy Scan** – Scan the image
8. **Docker Push** – Push to DockerHub (if credentials set)

### Required Jenkins Tools & Plugins

- JDK 21
- Maven (e.g., 3.8.5)
- Docker
- SonarQube Scanner Plugin
- Trivy CLI installed on agent
- DockerHub credentials (optional)

### SonarQube Setup in Jenkins

1. Go to `Manage Jenkins → Configure System`
2. Add a SonarQube server with name `MySonarQube`
3. Add authentication token if needed

---

## 🧪 Sample Output

### Running the App

```bash
Hello from Java Maven Docker App!
```

### Running Tests

```bash
[INFO] Running com.example.AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0
```

---

## 👤 Author

Created for DevOps CI/CD demo with full test/build/scan pipeline integration.


##Installations


Here is a step-by-step guide to install Jenkins, configure it, and set it up for the provided Java-Maven-Docker project with JUnit, SonarQube, and Trivy integration.

✅ Step 1: Install Jenkins on Ubuntu
bash
Copy
Edit
# 1. Update system
sudo apt update

# 2. Install Java
sudo apt install openjdk-21-jdk -y

# 3. Add Jenkins repo and key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

# 4. Install Jenkins
sudo apt update
sudo apt install jenkins -y

# 5. Start and enable Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

# 6. Check status
sudo systemctl status jenkins
✅ Step 2: Access Jenkins
Open browser: http://<your-server-ip>:8080

Get initial admin password:


sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Follow setup wizard and install recommended plugins

✅ Step 3: Install Required Jenkins Plugins
Install the following plugins via:

Manage Jenkins → Plugin Manager → Available → Search and Install:

Maven Integration

JUnit

SonarQube Scanner

Docker Pipeline

Git

Pipeline

Credentials Binding

Warnings Next Generation (optional for linting)

Blue Ocean (optional UI)

✅ Step 4: Configure Jenkins Global Tools
Manage Jenkins → Global Tool Configuration

JDK:

Name: JDK-21

JAVA_HOME: /usr/lib/jvm/java-21-openjdk-amd64

Maven:

Name: Maven-3.8.5

Auto-install or install manually.

SonarQube:

Manage Jenkins → Configure System → SonarQube Servers

Add new server

Name: MySonarQube

URL: http://<your-sonarqube-server>:9000

Token-based auth (generate token in Sonar UI)

✅ Step 5: Create Jenkins Credentials
Manage Jenkins → Credentials → Global:

DockerHub Credentials

Type: Username with password

ID: dockerhub-creds

Use in pipeline for Docker image push

✅ Step 6: Install Trivy (on Jenkins agent)

sudo apt install wget -y
wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.50.1_Linux-64bit.deb
sudo dpkg -i trivy_0.50.1_Linux-64bit.deb
✅ Step 7: Create and Configure Pipeline Job
New Item → Pipeline

Name: java-maven-docker-pipeline

Type: Pipeline → OK

In Pipeline Script, select:

Pipeline script from SCM

SCM: Git

Repo URL: <your-github-or-local-repo-url>

Branch: main

Script Path: Jenkinsfile

✅ Step 8: Run the Pipeline
Click Build Now and monitor pipeline stages:

Maven build

JUnit test and reporting

SonarQube scan

Docker build and push

Trivy scan

✅ Optional: Trigger Automatically
Poll SCM or setup GitHub Webhook to trigger pipeline on each push.


