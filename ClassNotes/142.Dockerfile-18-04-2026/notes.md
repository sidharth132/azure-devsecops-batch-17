# Class Notes: Writing Dockerfiles & Optimizing Layer Caching 📝

**Date:** 18-04-2026  
**Topic:** Dockerfile Directives, Layer Caching Optimization, Image Size Reduction, and `docker build`

---

## 📌 1. Essential Dockerfile Instructions

```dockerfile
# 1. Base Image
FROM node:18-alpine

# 2. Set Working Directory inside container
WORKDIR /app

# 3. Copy dependency manifests first (Layer Caching Optimization)
COPY package*.json ./

# 4. Install dependencies
RUN npm install --production

# 5. Copy remaining application source code
COPY . .

# 6. Expose documentation port
EXPOSE 3000

# 7. Runtime Startup Command
CMD ["node", "server.js"]
```

---

## ⚡ 2. Mastering Layer Caching

Docker caches the result of each instruction. When building an image:
* If a layer's input has not changed, Docker reuses the cached layer (`Using cache`).
* If a layer changes (e.g., `COPY . .`), all subsequent layers must be rebuilt from scratch.
* **Best Practice:** Copy package manifests (`package.json`) and run dependencies (`npm install`) **BEFORE** copying source code. This ensures library downloads are cached unless dependencies actually change.

---

## 💡 Key Takeaways
1. Order Dockerfile commands from least-frequently changing (base image, dependencies) to most-frequently changing (source code).
2. Use `.dockerignore` to exclude `node_modules`, `.git`, and sensitive `.env` files from entering image builds.
