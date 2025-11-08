# 🚀 AWS ECS Project **(Summary & Plan ONLY)**

# I will be attatching a link here to the public repo of the ecs project soon 

# Quick steps
# Pick the right app for me, considering memos
# make the dockerfile 



# Quick notes

# 1) time how long it takes to push to Amazon ECR manually and automatically. Get the time difference and percentage change

# 2) implement securirty practices like running the app as a non-root user. If you don’t explicitly specify a user or group in your Dockerfile using the USER instruction, then:
# If no USER is specified...
# Everything runs as:
# user: root
# group: root
# Which is something we want to avoid as engineers that implement the pricinciple of the least priveleged. 

# 3) Implement DRY principles, for example if you want to make two subnets just make 1 and code it to make two

# 4) Figure out if I want CMD or ENTRYPOINT For my Dockerfile



## 🎯 Objective
Build, containerise, and deploy an application using **Docker**, **Terraform**, and **Amazon ECS (Fargate)** with **HTTPS** and a **custom domain**, just like a real production setup.  
The goal: go from **manual ClickOps → Terraform IaC → automated CI/CD**.

---

## 🧭 Project Overview
| Phase | Focus | Tools |
|-------|--------|-------|
| **1. Local App** | Run the sample or your own app locally | Node.js / Go / Python |
| **2. Containerisation** | Build Docker image, test `/health` | Docker |
| **3. Image Registry** | Push image to AWS ECR | AWS ECR |
| **4. ClickOps Deployment** | Deploy manually with ECS + ALB + HTTPS | AWS Console |
| **5. Terraform IaC** | Rebuild infrastructure as code | Terraform |
| **6. CI/CD** | Automate builds & deploys | GitHub Actions |
| **7. HTTPS & Domain** | Use ACM + Route 53 for SSL/TLS | ACM / Route 53 |

🥅 **End goal:** 🥅 `https://tm.<your-domain>` reachable, healthy, and HTTPS-secured.

---

## 🧱 Minimal Architecture
```mermaid
flowchart LR
  User -->|HTTPS| ALB[ALB :443/:80]
  ALB --> ECS[ECS Service (Fargate)]
  ECS --> ECR[(ECR Repo)]
  ALB -.-> ACM[(ACM Cert)]
  User -->|DNS| R53[(Route 53 Record)]





