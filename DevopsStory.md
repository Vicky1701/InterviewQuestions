# End-to-End DevOps Deployment Story - Spring Boot Microservices

## Technology Stack Overview

| Component | Technology |
|-----------|------------|
| **Source Control** | Git + GitHub/GitLab |
| **Build Tool** | Maven |
| **CI/CD** | Jenkins |
| **Containerization** | Docker |
| **Container Registry** | Docker Hub (or Nexus/Artifactory) |
| **Orchestration** | Kubernetes (K8s) |
| **Monitoring & Logging** | ELK Stack (Elasticsearch + Logstash + Kibana) |
| **Cloud/Infrastructure** | Kubernetes cluster (on-prem OR AWS EKS) |

> **Note:** This is the most standard and safe answer — 90% of Java microservices projects follow this approach.

## 📌 Complete End-to-End Deployment Process

### 1. Developer Phase

- Write code for microservices (e.g., Rewards Service, Campaign Service, Gateway Service) in Spring Boot
- Code is committed to Git (GitLab/GitHub)
- Follow branching strategy: 
  - Feature branches → Merge to develop branch → Finally to master branch

### 2. Build & Package

- Jenkins CI pipeline gets triggered after code is merged
- Jenkins pulls code from Git and runs Maven build: `mvn clean package`
- Unit tests run → Build creates a JAR file

### 3. Containerization

Using Docker, Jenkins builds an image:

- Dockerfile defines how to package the JAR inside a lightweight JDK image

**Example Dockerfile:**
```dockerfile
FROM eclipse-temurin:17-jdk-alpine
COPY target/app.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

- Jenkins tags the Docker image with version + Git commit ID
- Image is pushed to a Docker Registry (e.g., Docker Hub or company's Nexus)

### 4. Deployment to Kubernetes

- Jenkins triggers a CD (Continuous Deployment) step
- Deployment is done using Kubernetes manifests (YAML files) or Helm charts

**Example Kubernetes Deployment:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rewards-service
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: rewards
        image: my-registry/rewards-service:1.0
        ports:
        - containerPort: 8080
```

- Kubernetes schedules Pods, creates a Service for load balancing, and exposes API through an Ingress (API Gateway)

### 5. Configuration & Secrets Management

- **ConfigMaps** for environment configs (URLs, flags)
- **Secrets** for passwords/keys (Base64 encoded)
- Spring Boot picks these from environment variables inside the container

### 6. Monitoring & Logging

- Logs from all microservices go to **ELK Stack** (Elasticsearch, Logstash, Kibana)
- Metrics are collected by **Prometheus + Grafana**
- Health checks via **Spring Boot Actuator + K8s liveness/readiness probes**

### 7. Deployment Strategies

- **Rolling Updates** in Kubernetes (update one pod at a time, no downtime)
- **Blue/Green** for major releases (two environments, switch traffic)

### 8. Production & Rollback

- If deployment fails, Jenkins or Kubernetes can rollback to previous image/version
- Immutable Docker images make rollback easy (just redeploy old tag)

## 📌 Interview-Ready Answer Template

*"In our Mobility Rewards project, we follow a CI/CD pipeline using Jenkins. Once I push code to GitLab, Jenkins pulls the code, runs Maven build, executes unit tests, and packages it as a JAR. Then we create a Docker image, push it to our Docker Registry, and deploy it to Kubernetes. We use ConfigMaps and Secrets for environment variables, and health checks are handled with Spring Boot Actuator and Kubernetes readiness probes. Logs from all microservices go into ELK Stack, and we monitor system metrics with Prometheus and Grafana. We generally use rolling deployments, but for critical updates we do blue/green deployments. If something goes wrong, rollback is easy because we redeploy the previous Docker image version."*

> 👉 This single answer covers build + CI/CD + deployment + monitoring + rollback.

## 📌 Your Role as a Developer

When asked about your involvement, be honest:

*"I work as a developer, mainly focusing on microservice development and ensuring my code builds and runs properly. The actual deployments are managed by our DevOps team, but I have full visibility of the process end-to-end. I collaborate with them by preparing Dockerfiles, actuator health checks, and proper logging so that deployments are smooth."*

## Key Benefits of This Approach

- **Automated Pipeline**: Reduces manual errors and deployment time
- **Scalability**: Kubernetes handles auto-scaling based on load
- **Reliability**: Health checks and rolling updates ensure zero downtime
- **Observability**: Complete visibility through logging and monitoring
- **Quick Recovery**: Fast rollback capabilities with immutable images
