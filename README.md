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
ssh -i PEM_KEY.pem ubuntu@65.0.7.136

<img width="840" height="60" alt="image" src="https://github.com/user-attachments/assets/13f710a6-f7de-48e9-bcaa-fa2c4a56d1d1" />

###Step 3: Install Required Software
`Update system packages`

# Update system packages
`sudo apt update && sudo apt upgrade -y`

# Install Node.js (LTS v18)
`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs`

# Install Git
`sudo apt install git -y`

# Install Nginx
`sudo apt install nginx -y`

# Install PM2
`sudo npm install -g pm2`

###Step 4: Clone Repository

`git clone https://github.com/prassanaganesh/Travelmemory.git
cd Travel_Memory_App/backend`

###Step 5: Install Backend Dependencies

`npm install`

###Step 6: Configure Environment Variables

1.Use MongoDB Atlas instead of local MongoDB.
Create cluster → Whitelist 0.0.0.0/0 → Copy connection string.

`nano .env`

Add:

MONGO_URI=mongodb+srv://prassanaganesh_db_user:<db_password>@travelmemory.8kk7ump.mongodb.net/?appName=travelmemory
PORT=3001

###Step 7: Start Backend with PM2

```
pm2 start index.js --name travelmemory-backend

<img width="1132" height="177" alt="image" src="https://github.com/user-attachments/assets/8276a7cf-76b7-46b6-b837-1abab9911b78" />

pm2 save

<img width="747" height="77" alt="image" src="https://github.com/user-attachments/assets/214b6f52-abe1-4b3b-9b30-0810f092dfcd" />

pm2 startup

<img width="1292" height="92" alt="image" src="https://github.com/user-attachments/assets/399b784b-68d7-414f-be92-61006aaf5c5e" />

pm2 status

```

###Step 8: Configure Nginx Reverse Proxy

`sudo nano /etc/nginx/sites-available/default`

Replace inside server {} block:

```bash

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
###Step 9: Restart Nginx
<img width="862" height="70" alt="image" src="https://github.com/user-attachments/assets/72a3fa57-28d2-4321-a335-5d6ef1b12398" />

###Step 10: Test Backend

<img width="1437" height="357" alt="image" src="https://github.com/user-attachments/assets/bbb845d2-7b60-4e7e-9623-6b8183fb1e97" />

Open browser:

Step 11: Insert Sample Data (Testing)
```bash
curl -X POST http://your-ec2-public-ip/api/trips \
-H "Content-Type: application/json" \
-d '{
    "tripName": "Incredible India",
    "startDateOfJourney": "19-03-2022",
    "endDateOfJourney": "27-03-2022",
    "nameOfHotels":"Hotel Namaste, Backpackers Club",
    "placesVisited":"Delhi, Kolkata, Chennai, Mumbai",
    "totalCost": 800000,
    "tripType": "leisure",
    "experience": "Lorem Ipsum...",
    "image": "https://t3.ftcdn.net/jpg/03/04/85/26/360_F_304852693_nSOn9KvUgafgvZ6wM0CNaULYUa7xXBkA.jpg",
    "shortDescription":"India is a wonderful country",
    "featured": true
}'
```

###Step 12: Start Server

`node index.js`

🌐 Task 2: Frontend and Backend Connection

   1.Create .env file in frontend:

      -REACT_APP_BACKEND_URL=http://localhost:3001

   2.Start frontend:
   3.Test in browser using EC2 public IP.

<img width="1912" height="972" alt="image" src="https://github.com/user-attachments/assets/8bbb37e8-4815-4a14-98f1-4ca13b1a512a" />

## ⚙️ Scaling the Travel Memory Application

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
<img width="1601" height="686" alt="ami 2" src="https://github.com/user-attachments/assets/0cacd730-8a5c-4d49-af57-28fb1d8d7f72" />

<img width="1561" height="592" alt="image" src="https://github.com/user-attachments/assets/fc514966-edb8-4d17-8052-eb2863350f46" />

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

### STEP 3: Create Target Group
1.Go to EC2 → Target Groups
2.Click Create target group
   -Settings:
      -Type: Instances
      -Protocol: HTTP
      -Port: 80
      -Name: travelmemory-tg

   -Health Check:
      -Path /, 
      -Port: traffic port

<img width="1581" height="477" alt="Target gtoup" src="https://github.com/user-attachments/assets/c08e465f-ce47-47d8-a4a7-91fcefdaf1a5" />

### STEP 4: Create Load Balancer
   1.Go to EC2 → Load Balancers → Create
   2.Choose
      -Application Load Balancer
   3.Configure:
      -Basic
         -Name: travelmemory-alb
         -Scheme: Internet-facing
      -Network:
         -Select 2+ subnets

      -Security Group:
         -Allow 
            -HTTP (80) → 0.0.0.0/0
         -Listener:
            -Protocol 
            -HTTP → Forward to travelmemory-tg
   4.Click Create Load Balancer

<img width="1536" height="407" alt="LB" src="https://github.com/user-attachments/assets/6052ca04-83e6-4e04-9b45-d6b87915b262" />
   
### STEP 5: Create Auto Scaling Group
   
   1.Go to EC2 → Auto Scaling Groups → Create
   2.Choose Launch Template:
      -travelmemory-template
   3.Network:
      -Same VPC,
      -select 2 subnets (multi-AZ)
   4.Attach Load Balancer:
      -Select:
      -Existing Load Balancer
      -Choose travelmemory-alb

   5.Group Size:
      -Set:
         -Desired: 2
         -Min: 2
         -Max: 4

   6.Scaling Policy:
      -Choose
         -Target tracking → CPU Utilization 50%
         
   7.Create ASG

<img width="1795" height="717" alt="asg" src="https://github.com/user-attachments/assets/75c0a802-305f-4b46-889e-4b8921b2a682" />
   

### STEP 6: Verify multiple instances are running as per scaling group

<img width="1597" height="262" alt="image" src="https://github.com/user-attachments/assets/1ce1a4a6-b512-42ac-b7f9-c91f34ab60e4" />


### 🌍 Task 4: Domain Setup with Cloudflare

### Step 1: Buy a Domain
   1.Example: memoryapp.site (Namecheap)

### Step 2: Add Domain to Cloudflare
   1.Login → Cloudflare Dashboard
   2.Click → Add Site
   3.Enter domain: memoryapp.site
   4.Choose Free Plan

<img width="1521" height="111" alt="image" src="https://github.com/user-attachments/assets/46c33369-d409-4836-b63d-d31e7d1e961d" />


### Step 3: Create CNAME Record
   Go to : DNS → Records → Add Record
      -Create: CNAME: www → travelmemory-alb-880954464.us-east-1.elb.amazonaws.com

### Step 4: Create A Record (Frontend EC2)
      -Add Record: A @ 54.197.83.104

<img width="1406" height="162" alt="image" src="https://github.com/user-attachments/assets/9d985d1f-2650-4f9a-8e49-74f4083e237a" />

### Step 5: Change Nameservers
      -Cloudflare provides nameservers (e.g., miles.ns.cloudflare.com, vivienne.ns.cloudflare.com)
      -Update nameservers in your domain registrar

      ✅ After completing Cloudflare setup, the site will be accessible at:-http://www.memoryapp.site & -https://memoryapp.site/addexperience

## Task 5: Design a deployment architecture diagram using draw.io to visualize the flow and connections.

<img width="791" height="667" alt="image" src="https://github.com/user-attachments/assets/eac42111-3020-4042-8e6e-fba042bfbdd2" />

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/dec89a83-1f54-4096-981c-8099e4557a9f" />

