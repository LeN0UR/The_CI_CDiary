#  AWS ECS Project **(Summary & Plan ONLY)**

## 🎯 Objective
Build, containerise, and deploy a real application using **Docker**, **Terraform**, and **Amazon ECS (Fargate)** with **HTTPS** and a **custom domain**.

This worflow will follow: **manual ClickOps → Terraform IaC → CI/CD automation**.

---

# Principles for this project:

1) DRY, avoid duplication (e.g., use count/for_each for subnets instead of repeating blocks). 
2) Least Privelege, IAM policies and Docker container user should always follow minimal access.
3) AWS Well-Architected Framework, especially Security, Reliability, and Operational Excellence.
4) Reproducibility, pin Terraform module versions to avoid breakage.
5) Security by Design, scanning images (Trivy, Checkov), no hardcoded creds, use GitHub Secrets / OIDC.

# Quick initial steps

 1) Pick the application (Most likely memos right now)
 2) Write Dockerfile --> Make it multi-stage, non-root user, and use .dockerignore
 3) Test container locally. Confirm health endpoint works.
 4) Push image to ECR and time manual vs automated pushes (time difference and percentage change)  

# General notes and PERSONAL REMINDERS

1) Use non-root user in Dockerfile (default = root → not acceptable for production).

2) Decide between CMD vs ENTRYPOINT depending on whether the app needs override flexibility.

3) Use pre-commit hooks for Terraform quality:
repos:
  - repo: https://github.com/some/repo
    rev: 1.18.0
    hooks:
      - id: terraform_validate
      - id: terraform_fmt
      - id: terraform_tflint

  This falls in line with the performance efficiency of the aws arcitechured framework pillar

4) Never store credentials in code → use GitHub Secrets or AWS OIDC integration.

5) Explore DevSecOps tools early:

Trivy (image and IaC scanning)
Checkov (Terraform/static analysis) 

6) Pin my terraform module so it is reproducible months or years later, aboids sudden breakage from updates a


# Common ECS questions

1) How did you ensure your application was highly available?
→ Multiple AZs, ALB health checks, Fargate tasks across subnets, auto-healing, no single point of failure. 

# Useful links

AWS Well-Architected Framework
1) https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html

ECS Documentation
2) https://docs.aws.amazon.com/ecs/

Terraform AWS Provider Docs
3) https://registry.terraform.io/providers/hashicorp/aws/latest/docs

Terraform ECS Module
4) https://registry.terraform.io/modules/terraform-aws-modules/ecs/aws/latest

Terraform VPC Module
5) https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest

Memos GitHub
6) https://github.com/usememos/memos

ALB Documentation
7) https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html

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

---
click ops notes:

When making task definition for ecs i altered task size as the default was to large. reduced it to 0.25 vCPU and 0.5gb memory as memos is light weight and ruins fine on minimal settings.


Clickops errors

errors i ran into: There was an error creating cluster ecs-clickops-memos-cluster.
Resource handler returned message: "Invalid request provided: CreateCluster Invalid Request: Unable to assume the service linked role. Please verify that the ECS service linked role exists. (Service: AmazonECS; Status Code: 400; Error Code: InvalidParameterException; Request ID: 614fa368-f78a-4bab-8c0d-2ea184801699; Proxy: null)" (RequestToken: 357b75b2-1d3e-5d2c-2125-a5399380d83a, HandlerErrorCode: InvalidRequest)

## This was during the creation of the ecs cluster
## Solution made me feel pretty stupid, in my last session was working on eu-2 region and every time i login to my non-root it defaults to global. This is the kind of mistake you tend to remember hahaha

