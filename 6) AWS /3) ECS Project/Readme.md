# 🚀 AWS ECS Project **(Summary & Plan)**

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





