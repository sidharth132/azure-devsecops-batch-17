# Class Notes: Docker Build & Push Pipeline to Azure Container Registry (ACR) 🐳

**Date:** 26-09-2026  
**Topic:** Container Image CI Pipeline, ACR Service Connections, Dynamic Build Tagging, and Security Scanning

---

## 📌 1. Architecture: Container Build & Push Flow

```
 [ Git Repo Commit ]
        |
        v
 [ Azure Pipelines Build Agent ]
   ├── Step 1: Login to Azure Container Registry (using Service Connection)
   ├── Step 2: Docker Build (trivy scan integrated)
   │     docker build -t axionacr.azurecr.io/axion-ui:$(Build.BuildId) .
   │
   └── Step 3: Docker Push
         docker push axionacr.azurecr.io/axion-ui:$(Build.BuildId)
         docker push axionacr.azurecr.io/axion-ui:latest
```

---

## 📜 2. Production YAML Pipeline Example

```yaml
trigger:
  branches:
    include:
      - main
  paths:
    include:
      - src/**

variables:
  acrName: 'axionacr.azurecr.io'
  imageName: 'axion-ui'

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: Docker@2
    displayName: 'Login to ACR'
    inputs:
      command: login
      containerRegistry: 'acr-service-connection'

  - task: Docker@2
    displayName: 'Build and Push Docker Image'
    inputs:
      command: buildAndPush
      repository: $(imageName)
      dockerfile: '**/Dockerfile'
      tags: |
        $(Build.BuildId)
        latest
```

---

## 💡 Key Takeaways
1. Always tag container images with immutable build IDs (`$(Build.BuildId)` or Git commit SHAs); never rely solely on `latest` in production.
2. Authenticate using Azure DevOps Service Connections backed by Managed Identities or Service Principals.
