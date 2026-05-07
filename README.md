# EasyCRUD Application Deployment Guide  
### Docker + AWS RDS + EC2

This document explains the deployment process for the EasyCRUD application using Docker containers, AWS RDS for the database, and an EC2 instance for hosting the application.

---

# Technologies Used

- Docker
- AWS EC2
- AWS RDS (MySQL)
- Java Spring Boot
- React

---

# Step 1: Install Required Packages

Update the system packages and install Docker along with the MySQL client.

```bash
apt update && apt install docker.io -y && apt install mysql-client -y
```

Check whether Docker is installed properly:

```bash
docker --version
```

---

# Step 2: Connect to AWS RDS Database

Use the following command to connect the EC2 instance with the MySQL database hosted on AWS RDS.

```bash
mysql -h database-1.cxm8cayykbqx.eu-north-1.rds.amazonaws.com -u admin -p
```

### Parameters

- `-h` → RDS endpoint
- `-u` → database username
- `-p` → password prompt

After entering the command, provide the database password when prompted.

---

# Step 3: Clone the Project Repository

Clone the project from GitHub and move into the project directory.

```bash
git clone https://github.com/Rohit-1920/EasyCRUD-Updated.git
```

```bash
cd EasyCRUD-Updated/
```

---

# Step 4: Backend Configuration and Deployment

Move into the backend folder:

```bash
cd backend
```

List the project files:

```bash
ls
```

## Build Docker Image

```bash
docker build . -t backend:v1
```

## Configure Database Connection

Open the application properties file:

```bash
nano src/main/resources/application.properties
```

Update the following values:

- Database URL
- Database username
- Database password

Save the file after making the changes.

## Rebuild the Docker Image

```bash
docker build . -t backend:v1
```

## Run Backend Container

```bash
docker run -d -p 8080:8080 --name backend backend:v1
```

The backend service will start on port `8080`.

---

# Step 5: Frontend Configuration and Deployment

Navigate to the frontend folder:

```bash
cd ../frontend
```

## Configure Environment Variables

Open the `.env` file:

```bash
nano .env
```

Update the backend API URL according to the EC2 public IP or domain.

## Build Docker Image

```bash
docker build . -t frontend:v1
```

## Run Frontend Container

```bash
docker run -d -p 80:80 --name frontend frontend:v1
```

The frontend application will be accessible through port `80`.

---

# Step 6: Verify Running Containers

Check whether both containers are running successfully.

```bash
docker ps
```

You should see:

- Backend container
- Frontend container

---

# Security Group Configuration

Ensure the EC2 security group allows the following inbound ports:

| Port | Purpose |
|------|----------|
| 80   | Frontend Application |
| 8080 | Backend Application |
| 3306 | MySQL / RDS Access |

---

# Application Architecture

```text
User → Frontend Container → Backend Container → AWS RDS Database
```

---

# Conclusion

The EasyCRUD application has been deployed using Docker containers on an AWS EC2 instance with AWS RDS used as the backend database service. This setup makes the application easier to manage, portable across environments, and simpler to deploy.
