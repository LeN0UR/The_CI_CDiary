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

### CLICKOPS


# 📘 AWS ECS ClickOps Deployment (Manual Provisioning)

This section documents the **ClickOps phase** of deploying **Memos** on AWS ECS with a custom domain, HTTPS termination, ALB routing, and complete networking.  
The goal of this manual phase is to build deep intuition for AWS services before implementing full automation with Terraform + CI/CD.

---

# 🏗️ Architecture Overview

The following components were created manually in the AWS Console:

- **VPC** (`10.0.0.0/16`)
- **Public + Private Subnets** in two AZs
- **Internet Gateway & Route Tables**
- **Security Groups** (ALB + ECS)
- **Application Load Balancer (ALB)** with HTTP + HTTPS
- **Target Group** for port 5230
- **ECS Fargate Cluster, Task Definition, Service**
- **ACM TLS Certificate** for domain
- **Route 53 Hosted Zone** with Alias A-record to ALB

Final result:  
👉 **https://nourdemo.com** serving the live Memos application.

---

# 🧱 1. VPC & Networking Setup

## 🔹 VPC
- CIDR: `10.0.0.0/16`

## 🔹 Subnets

| Subnet Type | AZ          | CIDR Block      |
|-------------|-------------|-----------------|
| Public      | eu-west-2a  | `10.0.1.0/24`   |
| Public      | eu-west-2b  | `10.0.3.0/24`   |
| Private     | eu-west-2a  | `10.0.2.0/24`   |
| Private     | eu-west-2b  | `10.0.4.0/24`   |

## 🔹 Routing
- Created **Internet Gateway** and attached to VPC
- Public Route Table:
  - `0.0.0.0/0 → igw-xxxx`
- Associated both public subnets

Private subnets remained isolated (no NAT).

---

# 🔐 2. Security Groups

## **ecs-clickops-alb-sg** (Load Balancer)
**Inbound:**
- HTTP 80 → `0.0.0.0/0`
- HTTPS 443 → `0.0.0.0/0`

**Outbound:** all allowed

---

## **ecs-clickops-ecs-sg** (ECS Tasks)
**Inbound:**
- TCP **5230** → **ecs-clickops-alb-sg**  
  *(Allows ALB → ECS traffic)*

**Outbound:** all allowed

---

# ⚖️ 3. Application Load Balancer

## **Configuration**
- Type: **Application Load Balancer**
- Scheme: **Internet-facing**
- Subnets: both **public** subnets
- Security group: `ecs-clickops-alb-sg`

## **Listeners**
- **HTTP : 80**
  - Action: Redirect → HTTPS:443
- **HTTPS : 443**
  - Certificate: ACM for `nourdemo.com`
  - Target: memos target group

---

# 🎯 4. Target Group

- Type: **IP**
- Protocol: HTTP
- Port: **5230**
- Health check:
  - Path: `/`
  - Port: `traffic-port`

Targets registered automatically by ECS.

---

# 🚀 5. ECS Setup

## 🔹 Cluster
- Name: `ecs-clickops-memos-cluster`
- Launch type: **Fargate**

## 🔹 Task Definition
- Runtime: **Fargate**
- CPU: 0.25 vCPU  
- Memory: 0.5 GB  
- Network mode: **awsvpc**
- Container:
  - Image: `neosmemo/memos:latest`
  - Port: **5230**
  - Logs: CloudWatch enabled

## 🔹 Service
- Name: `memos-service`
- Launch type: Fargate
- Subnets: public subnets
- Public IP: **Enabled**
- Load balancing: **Enabled**
- Target group: memos TG
- Desired count: **1**

### Debugging performed:
- Fixed broken ALB → ECS SG rule  
- Reattached proper target group  
- Corrected listener routing  
- Ensured health checks passed

---

# 🔐 6. TLS / HTTPS (ACM)

Requested ACM certificate for:

- `nourdemo.com`
- `www.nourdemo.com`

Used **DNS validation** (Route 53 CNAMEs auto-created).  
Certificate status: **Issued**.

Connected certificate to ALB’s **HTTPS:443** listener.

---

# 🌍 7. Route 53 DNS Integration

In Route 53 hosted zone:

### **A (Alias) Record**
| Name | Type | Value |
|------|------|--------|
| *(root)* | A (Alias) | ALB DNS name |

Optional:
- `www` → CNAME → root domain

This maps:

👉 **https://nourdemo.com** → ALB → ECS → Memos

---

# 🎉 Final Outcome

✔ Fully functioning Memos app on AWS ECS Fargate  
✔ Secure HTTPS with ACM  
✔ Custom domain with Route 53  
✔ Load-balanced, health-checked service  
✔ Production-grade manual AWS networking  
✔ Perfect foundation for Terraform automation  

---

# 🔧 Next Steps (Automation)

The next project phase will include:

- Dockerizing the service
- Manual push to ECR (**measure push time**)
- Terraform IaC for all infrastructure
- GitHub Actions CI/CD deployment pipeline
- Automatic rolling updates to ECS

---

# 🎥 Optional Enhancements
If you want, you can also add:

- Screenshots  
- Architectural diagram  
- Mermaid diagram  
- A short GIF/video of the final app running  

I can generate these upon request.



When making task definition for ecs i altered task size as the default was to large. reduced it to 0.25 vCPU and 0.5gb memory as memos is light weight and ruins fine on minimal settings.

clickops learning

You learned:
ALBs must have 2 public subnets in different AZs
ECS tasks can run in either AZ
Target group instances can become “draining” when ALB loses reachability
This is why ECS tasks bounce between subnets/AZs
This is real production-grade knowledge.
and so much more this readme would get tiring.

# CLICKOPS errors

errors i ran into: There was an error creating cluster ecs-clickops-memos-cluster.
Resource handler returned message: "Invalid request provided: CreateCluster Invalid Request: Unable to assume the service linked role. Please verify that the ECS service linked role exists. (Service: AmazonECS; Status Code: 400; Error Code: InvalidParameterException; Request ID: 614fa368-f78a-4bab-8c0d-2ea184801699; Proxy: null)" (RequestToken: 357b75b2-1d3e-5d2c-2125-a5399380d83a, HandlerErrorCode: InvalidRequest)

ran into "Unable to assume the service linked role. Verify that the ECS service-linked role exists.". Chcked through the Iam roles to see if i had the correct awsecsservice one and retried ended up being fine. Temporary issue on amazons part i assume.

my est task not reaching a healthy state every 2/3 minutes (RUNNING → DRAINING → STOPPED → RESTART → UNHEALTHY)
so i checked the releavant networks and updated correctly.

made the error of using a a default sg for my alb, promptly changed that. Now able to access the website.


## This was during the creation of the ecs cluster
## Solution made me feel pretty stupid, in my last session was working on eu-2 region and every time i login to my non-root it defaults to global. This is the kind of mistake you tend to remember hahaha

