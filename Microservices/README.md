# Microservices Task

## 📌 Project Overview
This project consists of **four containerized Node.js microservices**:
- **User Service** (`Port 3000`)
- **Product Service** (`Port 3001`)
- **Order Service** (`Port 3002`)
- **Gateway Service** (`Port 3003`)

These services run inside Docker containers and communicate using `Docker Compose`.

---

## 📁 **Project Structure**
```
submission/
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── app.js
├── product-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── app.js
├── order-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── app.js
├── gateway-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── app.js
├── docker-compose.yml
└── README.md
```

---

## 📜 **Dockerfile for Each Service**

Create a `Dockerfile` inside each service directory.

### **Example Dockerfile (`user-service/Dockerfile`)**
```dockerfile
# Use Node.js base image
FROM node:18

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Expose the port
EXPOSE 3000

# Command to run the application
CMD ["node", "app.js"]
```
📌 **Repeat the same for `product-service`, `order-service`, and `gateway-service` with their respective ports.**

---

## 📜 **docker-compose.yml File**
Create a `docker-compose.yml` file in the root directory:

```yaml
version: '3.8'

services:
  user-service:
    build: ./user-service
    ports:
      - "3000:3000"
    networks:
      - app-network

  product-service:
    build: ./product-service
    ports:
      - "3001:3001"
    networks:
      - app-network

  order-service:
    build: ./order-service
    ports:
      - "3002:3002"
    networks:
      - app-network

  gateway-service:
    build: ./gateway-service
    ports:
      - "3003:3003"
    networks:
      - app-network
    depends_on:
      - user-service
      - product-service
      - order-service

networks:
  app-network:
    driver: bridge
```

---

## 🚀 **Setup Instructions**

### **1️⃣ Prerequisites**
Ensure you have installed:
- **Docker** (`docker --version`)
- **Docker Compose** (`docker-compose --version`)
- **Git** (`git --version`)

---

### **2️⃣ Clone the Repository**
Run the following command to clone the repo:
```sh
 git clone https://github.com/mohanDevOps-arch/Microservices-Task.git
 cd Microservices-Task/Microservices
```

---

### **3️⃣ Build & Run the Microservices**
```sh
docker-compose up --build -d
```
This command:
- Builds all Docker images
- Starts all containers in **detached mode (`-d`)**

Verify running containers:
```sh
docker ps
```
📸 **Attach Screenshot:** `docker ps` output showing all services running.

---

### **4️⃣ Test API Endpoints**

#### **User Service**
```sh
curl http://localhost:3000/users
```
📸 **Attach Screenshot:** Output showing list of users.

#### **Product Service**
```sh
curl http://localhost:3001/products
```
📸 **Attach Screenshot:** Output showing list of products.

#### **Order Service**
```sh
curl http://localhost:3002/orders
```
📸 **Attach Screenshot:** Output showing list of orders.

#### **Gateway Service**
```sh
curl http://localhost:3003/gateway
```
📸 **Attach Screenshot:** Output showing gateway response.

---

## 📦 **Deploying on AWS EC2**

### **1️⃣ Launch an EC2 Instance**
- Use **Ubuntu 22.04 LTS** or Amazon Linux.
- Open ports **3000-3003** in **security groups**.
- SSH into the instance:
```sh
ssh -i your-key.pem ubuntu@your-ec2-ip
```

---

### **2️⃣ Install Docker & Docker Compose on EC2**
```sh
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```
Then install **Docker Compose**:
```sh
sudo apt install -y docker-compose
```
Logout and reconnect:
```sh
exit
ssh -i your-key.pem ubuntu@your-ec2-ip
```

📸 **Attach Screenshot:** Docker installation verification (`docker --version`).

---

### **3️⃣ Copy Project to EC2**
On your local machine:
```sh
scp -i your-key.pem -r Microservices-Task ubuntu@your-ec2-ip:~
```

On EC2:
```sh
cd Microservices-Task/Microservices
docker-compose up --build -d
```
📸 **Attach Screenshot:** `docker ps` output on EC2 instance.

---

### **4️⃣ Test Services on EC2**
Use your EC2 public IP:
```sh
curl http://your-ec2-ip:3000/users
curl http://your-ec2-ip:3001/products
curl http://your-ec2-ip:3002/orders
curl http://your-ec2-ip:3003/gateway
```
📸 **Attach Screenshot:** API responses from EC2.

---

## 🛠 **Troubleshooting**
### **Check Logs for Errors**
```sh
docker logs microservices-user-service-1
```

### **Restart Services**
```sh
docker-compose down && docker-compose up --build -d
```

📸 **Attach Screenshot:** If errors occur, include logs.

---

## ✅ **Final Submission**
Zip the `submission/` folder:
```sh
tar -czvf submission.tar.gz submission/
```
Upload `submission.zip` as per the submission guidelines.

📸 **Attach Screenshot:** Final folder structure before zipping.

---

## 🎉 Congratulations!
Your microservices project is successfully deployed! 🚀

