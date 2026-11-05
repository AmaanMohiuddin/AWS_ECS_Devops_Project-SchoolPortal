# 🚀 Three-Tier Application Deployment on AWS ECS (Fargate) with CI/CD

## 📌 Project Overview
This project demonstrates a fully automated, end-to-end DevOps pipeline for deploying a Three-Tier Application (React frontend, Node.js backend, MySQL database) to AWS. 

The core focus of this project is **Infrastructure & Deployment Automation**: moving away from local Docker Compose setups to a scalable, production-style cloud architecture using Amazon ECS Fargate, Amazon RDS, and GitHub Actions.

**Deployment Workflow:**
1. **Developer** pushes code to the `main` branch.
2. **GitHub Actions** triggers the CI/CD pipeline.
3. Docker images (Frontend & Backend) are built and pushed to **Amazon ECR**.
4. **Amazon ECS (Fargate)** pulls the new images and deploys the updated containers.
5. The backend connects to a managed **Amazon RDS MySQL** database.
6. **Application Load Balancer (ALB)** routes external traffic to the frontend/backend containers.

## 🛠️ AWS Services & Tech Stack
*   **Compute:** Amazon ECS (Fargate) - *Serverless container orchestration.*
*   **Container Registry:** Amazon ECR
*   **Database:** Amazon RDS (MySQL)
*   **Networking:** VPC, Public Subnets, Security Groups, Application Load Balancer (ALB)
*   **CI/CD:** GitHub Actions
*   **Containerization:** Docker

## 🧠 Key Challenges & Learnings
*   **Challenge:** The ECS service kept failing health checks and rolling back deployments.
    *   **Solution:** Checked CloudWatch logs and realized the Node.js backend was taking longer to start than the default ALB health check timeout. I adjusted the `healthCheckGracePeriodSeconds` in the ECS Task Definition to allow sufficient boot time.
*   **Challenge:** GitHub Actions failed with an `AccessDeniedException` when pushing to ECR.
    *   **Solution:** The IAM user configured in GitHub Secrets lacked the `AmazonEC2ContainerRegistryFullAccess` policy. I updated the IAM permissions to enforce least-privilege access.

## 💰 Cost Optimization (Free Tier Strategy)
To ensure this project remained entirely within the AWS Free Tier:
*   **RDS:** Utilized a `db.t3.micro` Single-AZ instance.
*   **ECR:** Only stored the essential Docker images and cleaned up old, untagged images.

