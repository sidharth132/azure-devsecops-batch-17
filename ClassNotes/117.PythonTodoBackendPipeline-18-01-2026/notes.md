# Class Notes: Python Backend CI/CD Pipeline & 3-Tier Network Integration 🐍

**Date:** 18-01-2026  
**Topic:** Python API Automation, Virtual Environments, Pytest, and 3-Tier Network Connectivity

---

## 📌 1. Architecture: 3-Tier Network & Python Backend

Connecting the Python REST API backend securely between the frontend web tier and the database tier:

```
 [ Internet ]
      |
      v
 [ Frontend Subnet ]
   └── Frontend VM (Public IP, NGINX reverse proxies /api requests)
            | (Internal VNet communication over port 5000)
            v
 [ Backend Subnet ]
   └── Backend VM (Private IP only, Python Flask / FastAPI daemon)
            | (Internal DB port 5432 / 1433)
            v
 [ Database Subnet ]
   └── PostgreSQL / Azure SQL Database
```

---

## ⚙️ 2. Python CI Pipeline Tasks

```yaml
steps:
  - task: UsePythonVersion@0
    inputs:
      versionSpec: '3.10'

  - script: |
      python -m venv venv
      source venv/bin/activate
      pip install --upgrade pip
      pip install -r requirements.txt
    displayName: 'Install Dependencies'

  - script: |
      source venv/bin/activate
      pytest tests/ --junitxml=test-results.xml
    displayName: 'Execute Pytest Suite'

  - script: |
      flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
    displayName: 'Lint Python Code'
```

---

## 💡 Key Takeaways
1. Keep backend application VMs entirely private; expose them only through internal reverse proxies or load balancers.
2. Automate dependency caching and virtual environment isolation within CI pipelines.
