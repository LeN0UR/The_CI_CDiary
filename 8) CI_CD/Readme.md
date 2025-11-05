# ⚙️ CI/CD — GitHub Actions & Real-World Automation

This module covers **Continuous Integration (CI)** and **Continuous Deployment (CD)** using **GitHub Actions**.  
It combines the fundamentals with **real-world DevOps challenges** that simulate production scenarios.

---

## 🧭 Overview

> *“Don’t do it manually — if it can be automated, it should be.”*

In this section, I learned how to:
- Automate Docker builds, testing, and deployment.
- Create workflows that lint, validate, and enforce quality standards.
- Use GitHub Actions securely with **secrets and environment variables**.
- Apply CI/CD principles to real projects (Docker, Terraform, ECS).

---

## 🧰 Key Concepts

| Concept | Description |
|----------|--------------|
| **Continuous Integration (CI)** | Automatically test, lint, and validate every code change. |
| **Continuous Deployment (CD)** | Automatically build, tag, and release containers or infrastructure after tests pass. |
| **Secrets Management** | Securely store credentials like PATs, AWS keys, and Docker tokens in GitHub Secrets. |
| **Workflow Triggers** | Define automation events such as `push`, `pull_request`, or `schedule`. |

---

## 🧪 Real-World CI/CD Challenges

Below are the two **core challenges** designed to build practical CI/CD skills using GitHub Actions.

---

### 🚀 Challenge 1: Automated Container Build & Push

**Objective:**  
Create a CI/CD pipeline that automatically builds a Docker image and pushes it to **Docker Hub** (or ECR) whenever new code is pushed.

**Steps:**
1. Build a small app (e.g., `HelloApp`) with a valid **Dockerfile**.  
2. Create a GitHub Actions workflow file:  
3. Configure it to:
- Build the Docker image.
- Tag it using `${{ github.repository }}` and `${{ github.sha }}`.
- Push it to your Docker Hub repository.
4. Store Docker Hub credentials in **GitHub → Settings → Secrets and variables → Actions**  
(use names like `DOCKER_USERNAME` and `DOCKER_TOKEN`).

**Example Workflow:**

```yaml
name: Build and Push Docker Image
on:
push:
 branches: [main]

jobs:
build:
 runs-on: ubuntu-latest
 steps:
   - name: Checkout Repository
     uses: actions/checkout@v4

   - name: Build Docker Image
     run: docker build -t ${{ github.repository }}:${{ github.sha }} .

   - name: Login to Docker Hub
     run: echo "${{ secrets.DOCKER_TOKEN }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

   - name: Push Image
     run: docker push ${{ github.repository }}:${{ github.sha }}
