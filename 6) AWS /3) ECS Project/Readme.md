# The "ECS" Project

## Objective
Build, containerise, and deploy an application using **Docker**, **Terraform**, and **ECS**, with **HTTPS** and a **custom domain** — exactly like a real production workload.

You’ll learn to go from manual AWS setup (ClickOps) → Infrastructure as Code (Terraform) → automated deployments (CI/CD).

---

## Application Setup

### Options
- Use the provided **Threat Composer** sample  
- Pick another lightweight app (Node.js, Go, Python, etc.) that runs on port 80  
- Bring your own (BYO) application

### Requirements
- Must expose a `/health` route returning:
  ```json
  {"status": "ok"}
