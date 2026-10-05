# Spring Boot Application – Jenkins CI/CD Deployment

A hands-on Spring Boot application demonstrating **Java application development, Maven build automation, and CI/CD deployment using Jenkins on an AWS EC2 Ubuntu server**.

The project uses a Jenkins Declarative Pipeline to automatically:

* Checkout source code from GitHub
* Build the Spring Boot application using Maven
* Stop the previously running application
* Deploy the newly generated JAR file
* Run the application on port `9091`

---

## Project Overview

This project was created to understand how a Java Spring Boot application can be integrated with a DevOps CI/CD workflow.

The application is packaged as a JAR using Maven and deployed through Jenkins. The Jenkins pipeline automates the application deployment process instead of requiring the application to be manually built and started after every code change.

### CI/CD Workflow

```text
Developer
    |
    | Push Code
    v
GitHub Repository
    |
    v
Jenkins
    |
    +---- Checkout Code
    |
    +---- Maven Build
    |
    +---- Stop Old Application
    |
    +---- Deploy New JAR
    |
    v
Spring Boot Application
    |
    v
AWS EC2 :9091
```

---

## Technologies Used

| Technology        | Purpose                           |
| ----------------- | --------------------------------- |
| Java 17           | Application development           |
| Spring Boot 3.5.6 | Web application framework         |
| Maven             | Build and package the application |
| Jenkins           | CI/CD automation                  |
| Git & GitHub      | Source code management            |
| Linux / Ubuntu    | Deployment environment            |
| AWS EC2           | Application hosting               |

The project's `pom.xml` uses Spring Boot `3.5.6`, Java `17`, `spring-boot-starter-web`, Spring Boot testing support, and the Spring Boot Maven plugin.

---

## Repository Structure

```text
Spring-Boot/
│
├── .mvn/
│   └── wrapper/
│
├── src/
│   ├── main/
│   └── test/
│
├── .gitattributes
├── .gitignore
├── jenkinsfile
├── mvnw
├── mvnw.cmd
└── pom.xml
```

---

## Spring Boot Application

The project is a Maven-based Spring Boot application.

The application is packaged into an executable JAR using Maven:

```bash
mvn clean package
```

The generated JAR is then used by Jenkins during deployment.

---

# Jenkins CI/CD Pipeline

The repository contains a `jenkinsfile` that defines the CI/CD pipeline.

The pipeline contains the following stages:

### 1. Checkout Code

Jenkins checks out the application source code from the GitHub `main` branch.

```groovy
stage('Checkout Code') {
    steps {
        git branch: 'main',
            url: 'https://github.com/sivanarasimhulu21/Spring-Boot.git'
    }
}
```

### 2. Build Application

Maven cleans the previous build and packages the Spring Boot application.

```bash
mvn clean package -DskipTests
```

### 3. Stop Old Application

Before deploying the new version, Jenkins checks which process is using port `9091`.

If an existing application is running, Jenkins stops it.

```bash
PID=$(lsof -t -i:${APP_PORT}) || true

if [ ! -z "$PID" ]; then
    kill -9 $PID
fi
```

### 4. Deploy Application

The newly generated JAR file is started in the background.

```bash
nohup java -jar target/*.jar > app.log 2>&1 &
```

The application is configured to run on:

```text
Port: 9091
```

The Jenkins pipeline reports a successful deployment after the deployment stage completes.

---

# AWS EC2 Deployment

The application can be deployed on an Ubuntu-based AWS EC2 instance.

### Basic Environment

```text
AWS EC2
   |
   +-- Ubuntu
   |
   +-- Java
   |
   +-- Maven
   |
   +-- Jenkins
   |
   +-- Spring Boot Application
```

Make sure the required security-group port is allowed if the application needs to be accessed externally.

For this project, the application runs on:

```text
9091
```

Application URL:

```text
http://<EC2-PUBLIC-IP>:9091
```

---

# Jenkins Job Configuration

Create a Jenkins **Pipeline** job.

Use:

```text
Definition:
Pipeline script from SCM

SCM:
Git

Repository URL:
https://github.com/sivanarasimhulu21/Spring-Boot.git

Branch:
main

Script Path:
jenkinsfile
```

Jenkins will then obtain the pipeline definition directly from the repository.

---

# Running the Project Manually

If you want to run the application without Jenkins:

### Clone the repository

```bash
git clone https://github.com/sivanarasimhulu21/Spring-Boot.git
cd Spring-Boot
```

### Build the application

```bash
mvn clean package
```

### Run the generated JAR

```bash
java -jar target/*.jar
```

Then access the application using:

```text
http://localhost:9091
```

or, when running on EC2:

```text
http://<EC2-PUBLIC-IP>:9091
```

---

# CI/CD Pipeline Flow

```text
GitHub
   |
   | Source Code
   v
Jenkins
   |
   | Checkout
   v
Maven
   |
   | Build & Package
   v
Spring Boot JAR
   |
   | Stop Previous Version
   v
Deploy New Version
   |
   v
AWS EC2
   |
   v
Application :9091
```

---

# What I Learned

Through this project, I practiced:

* Spring Boot application packaging
* Maven build lifecycle
* Git and GitHub source-code management
* Jenkins Declarative Pipelines
* Jenkins Pipeline stages
* Linux process management
* JAR-based application deployment
* AWS EC2 application hosting
* Basic CI/CD automation
* Deploying a new application version over an existing running version

---

# DevOps Skills Demonstrated

```text
Git
GitHub
Jenkins
CI/CD
Maven
Java
Spring Boot
Linux
AWS EC2
Shell Scripting
Application Deployment
```

---

## Repository

GitHub:

https://github.com/sivanarasimhulu21/Spring-Boot

---

## Author

**Siva Narasimhulu**

DevOps / Cloud / Linux Administration Enthusiast

---

⭐ If you find this project useful, consider giving the repository a star.
