# BoardgameListingWebApp

## Description 

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊

## End-to-End DevSecOps CI/CD Pipeline: Boardgame Application
## 📌 Project Overview
This project demonstrates the implementation of a complete, secure Corporate CI/CD Pipeline for a Java-based Boardgame web application. It covers the entire deployment lifecycle, from infrastructure provisioning on AWS to static code analysis, security vulnerability scanning, artifact management, and containerized deployment onto a Kubernetes cluster, fully monitored by Prometheus.

## 🛠️ Technology Stack & Tools
Cloud Provider: AWS (EC2)

Version Control: Git & GitHub (kingpin1374/Boardgame)

CI/CD Server: Jenkins

Build Tool: Maven

Static Code Analysis: SonarQube

Artifact Repository: Sonatype Nexus

Containerization: Docker & DockerHub

Container Orchestration: Kubernetes (K8s)

Security Scanning: Trivy, kube-bench

Monitoring & Observability: Prometheus, Node Exporter, Blackbox Exporter

## 🏗️ Phase 1: Infrastructure Architecture
The underlying infrastructure was provisioned on AWS using t3.small and t3.micro EC2 instances distributed across the ap-south-1 region. The environment consists of dedicated servers to isolate CI/CD tools from the application runtime:

Kubernetes Cluster: 1 Master Node, 2 Worker Nodes (Slave-1, Slave-2)

CI/CD Servers: Dedicated instances for Jenkins, SonarQube, and Nexus.

Monitoring Server: Dedicated instance for Prometheus and Exporters.

Security: Initial cluster security benchmarking was performed using kube-bench to ensure CIS compliance.

## 🚀 Phase 2 & 3: The CI/CD Pipeline Architecture
The deployment process is fully automated via a Declarative Jenkins Pipeline with the following stages:

Source Code Checkout: Pulls the latest commits from the private GitHub repository.

Build & Unit Test: Compiles the application and runs unit tests using Maven.

Security Scanning (SCA): Integrates Trivy to scan dependencies for known vulnerabilities before building the image.

Static Code Analysis: Pushes code to SonarQube for quality gating. Current Quality Gate: Passed (0 Vulnerabilities).

Artifact Management: Packages the application and securely uploads the build artifacts to the Nexus Repository.

Dockerization: Builds the Docker image and pushes it to a centralized DockerHub registry.

Kubernetes Deployment: Authenticates securely with the K8s cluster using Service Accounts and RBAC, deploying the application to the webapps namespace. Exposes the application via a NodePort service.

## 📊 Phase 4: Monitoring & Observability
Continuous monitoring is established to track both system metrics and application uptime:

Prometheus: Acts as the central time-series database scraping metrics from deployed targets.

Node Exporter: Installed on instances to monitor hardware and OS-level metrics (CPU, memory, disk).

Blackbox Exporter: Configured to probe the application endpoints (HTTP 2xx) over the network to guarantee uptime and measure response times, capturing both successful connections and simulated context deadlines/failures.

## Author: Aman Sharma
