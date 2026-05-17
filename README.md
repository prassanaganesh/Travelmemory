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
```bash
ssh -i PEM_KEY.pem ubuntu@65.0.7.136

<img width="840" height="60" alt="image" src="https://github.com/user-attachments/assets/13f710a6-f7de-48e9-bcaa-fa2c4a56d1d1" />

###Step 3: Install Required Software
Update system packages

# ⚙️ Scaling the Travel Memory Application

## STEP 1: Create AMI (Golden Image)
This captures your working server.

In **AWS Console**:
1. Go to **EC2 → Instances**
2. Select your working instance
3. Click **Actions → Image and templates → Create Image**
4. Fill:
   - **Name**: `travelmemory-ami`
   - Leave defaults

---

## STEP 2: Create Launch Template
1. Go to **EC2 → Launch Templates**
2. Click **Create Launch Template**

Fill the following:

**Basic:**
- **Name**: `travelmemory-template`

**AMI:**
- Select your created AMI

**Instance Type:**
- `t2.micro`

**Key Pair:**
- Select your existing key

**Security Group:**
- Create/select one with:
  - HTTP → Port 80 → `0.0.0.0/0`
  - SSH → Port 22 → Your existing EC2 IP

--
<img width="1601" height="686" alt="image" src="https://github.com/user-attachments/assets/4ba00638-9eea-4f1e-bcc3-b76305b7ef02" />



