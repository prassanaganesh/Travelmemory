# 🚀 Travel Memory Application Deployment

The **Travel Memory** application has been developed using the **MERN stack**.  
This project aims to deploy a production-ready MERN application on **AWS**, using:

- Separate **EC2 instances** for frontend and backend
- **NGINX** as a reverse proxy
- **Cloudflare** for secure public access
- **AWS Application Load Balancer (ALB)** to handle failures and distribute traffic

---

## 📌 Project Objectives and Scope
- Deploy a MERN application on AWS EC2 instances
- Centralize traffic through NGINX, routing frontend and backend paths appropriately
- Use an AWS ALB so backend failures on one instance do not cause downtime
- Securely expose the application over HTTPS using Cloudflare

---

## 🛠 Tech Stack

### Frontend
- React.js  
- CSS  

### Backend
- Node.js  
- MongoDB (Atlas)  

### Infrastructure
- AWS EC2 (Compute)  
- Nginx (Reverse Proxy + Static Hosting)  
- PM2 (Process Manager)  
- AWS Application Load Balancer (ALB)  
- Cloudflare (DNS, CDN, SSL)  

---

## 📂 Task 1: Backend Configuration

### Step 1: Launch EC2 Instance
1. Go to **AWS Console → EC2**
2. Click **Launch Instance**
3. Configure:
   - **AMI**: Ubuntu  
   - **Instance type**: `t2.micro` (Free Tier)  
   - **Key pair**: Create/Download  
   - **Security Group**:  
     - SSH → Port 22 (Anywhere)  
     - HTTP → Port 80 (Anywhere)
       
<img width="1786" height="556" alt="image" src="https://github.com/user-attachments/assets/74c28194-d18b-4de8-be90-6194838232ec" />

### Step 2: Connect to EC2
<img width="840" height="60" alt="image" src="https://github.com/user-attachments/assets/13f710a6-f7de-48e9-bcaa-fa2c4a56d1d1" />
