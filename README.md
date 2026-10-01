# 🚀 AWS EC2 Automated Infrastructure Lifecycle Engine

[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20Security%20Groups%20%7C%20KMS-orange?logo=amazon-aws)](https://aws.amazon.com/)
[![LocalStack](https://img.shields.io/badge/Emulation-LocalStack%203.8-blue?logo=docker)](https://localstack.cloud/)
[![Docker](https://img.shields.io/badge/Container-Docker%20Desktop-2496ED?logo=docker)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An enterprise-ready automated cloud compute provisioning pipeline. This project demonstrates end-to-end infrastructure lifecycle management—provisioning virtual machines, configuring least-privilege firewalls, managing cryptographic SSH keys, executing dynamic vertical scaling, and performing clean resource teardown.

---

## 🏛️ Architecture Diagram

```mermaid
flowchart TD
    Admin["👨‍💻 Cloud Engineer / CI/CD"]
    
    subgraph Cloud_Environment ["☁️ AWS Emulation Layer (Docker / LocalStack)"]
        SG["🛡️ Security Group Firewall<br>(Ports 80 & 22 Only)"]
        KP["🔑 RSA 2048-bit Key Pair<br>(production-key.pem)"]
        
        subgraph Compute_Tier ["EC2 Virtual Machine Fleet"]
            EC2_Micro["🖥️ EC2: t3.micro<br>(1 vCPU, 1 GB RAM)<br><i>State: Initial Deployment</i>"]
            EC2_Medium["🖥️ EC2: t3.medium<br>(2 vCPU, 4 GB RAM)<br><i>State: Scaled Capacity</i>"]
            
            EC2_Micro -.->|Dynamic Vertical Scale| EC2_Medium
        end
        
        SG --> Compute_Tier
        KP --> Compute_Tier
    end
    
    Admin -->|awslocal CLI Automation| SG
    Admin -->|Key Generation| KP
    Admin -->|Provision & Scale| Compute_Tier
