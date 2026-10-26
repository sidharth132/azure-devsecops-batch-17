# Class Notes: End-to-End Enterprise Java Maven CI/CD Pipeline ☕

**Date:** 17-01-2026  
**Topic:** Java Maven Build Lifecycle, Unit Testing, Artifact Packaging, and Automated VM Deployment

---

## 📌 1. Architecture: Java Application CI/CD Pipeline

```
 [ Java Git Repo ]
        |
        v
 [ Azure CI Pipeline: maven-build ]
   ├── Step 1: Maven Compile (mvn clean compile)
   ├── Step 2: Run Unit Tests (mvn test)
   ├── Step 3: SonarQube Code Quality & Test Coverage Analysis
   ├── Step 4: Package JAR / WAR (mvn package)
   └── Step 5: Publish Build Artifact
        |
        v
 [ Azure CD Pipeline: vm-deploy ]
   ├── Step 1: Download JAR Artifact
   ├── Step 2: Deploy to Target VM via systemd
   └── Step 3: Health Check Endpoint Validation (curl http://localhost:8080/actuator/health)
```

---

## ⚙️ 2. Core Pipeline Tasks

* `Maven@3`: Executes standard Maven goals (`clean package`) using pre-configured `pom.xml`.
* `PublishTestResults@2`: Publishes JUnit XML test results directly into Azure DevOps pipeline test dashboard.
* `systemd service`: Manages Java process lifecycle (`java -jar app.jar`) with automatic restart on failure.

---

## 💡 Key Takeaways
1. Always run unit tests and static code analysis prior to generating deployable JAR/WAR artifacts.
2. Use systemd to manage production Java services for automatic restart and structured logging.
