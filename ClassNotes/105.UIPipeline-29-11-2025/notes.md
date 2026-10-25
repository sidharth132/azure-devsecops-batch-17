# Class Notes: UI CI/CD Pipeline & End-to-End Automation Roadmap 🎨

**Date:** 29-11-2025  
**Topic:** Monolithic UI Deployment Pipeline, Node.js Build, Artifact Publishing, and Release Automation

---

## 📌 1. Architecture: Automated UI Deployment Pipeline

Building an automated pipeline to take frontend code from developer commit to live NGINX web server hosting on Azure:

```
 [ GitHub / Azure Repos ]
            |
            v  (Triggers on commit)
 [ Azure Pipeline: dev-ui-pipeline ]
   ├── Stage 1: Build
   │     ├── npm install (Install dependencies)
   │     ├── npm run build (Compile React/Vue frontend to production bundle)
   │     └── PublishPipelineArtifact (Package 'dist' / 'build' folder)
   │
   └── Stage 2: Deploy to VM
         ├── DownloadPipelineArtifact
         ├── Transfer files via SCP to /var/www/html
         └── Reload NGINX service (sudo systemctl reload nginx)
```

---

## ⚙️ 2. Step-by-Step Pipeline Tasks

1. **Node.js Environment Setup:** Installs required Node.js runtime version on the agent.
2. **Build Generation:** Generates optimized production HTML, CSS, and minified JS assets.
3. **Artifact Staging:** Archives build outputs as an immutable pipeline artifact for traceable deployments.
4. **Target Deployment:** Deploys artifacts directly to the target Linux host's web root (`/var/www/html`).

---

## 💡 Key Takeaways
1. Decouple build generation from deployment so the same build artifact can be promoted across environments.
2. Never build directly on production web servers; build on ephemeral CI build agents.
