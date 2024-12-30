# Install Jenkins on AWS

## Technologies Used
- **Jenkins**
- **Docker**
- **AWS**
- **Linux**

## Project Description
This project demonstrates how to install and set up Jenkins on an AWS Ubuntu server using Docker. The steps include:
1. Creating an Ubuntu server on AWS.
2. Setting up and running Jenkins as a Docker container.
3. Initializing Jenkins for first-time use.

---

## Steps to Install Jenkins on AWS

### Step 1: Create an AWS EC2 Instance
1. Log in to your AWS Management Console.
2. Navigate to **EC2** and click **Launch Instance**.
3. Choose an **Ubuntu Server** AMI (e.g., Ubuntu 20.04 LTS).
4. Select a **t2.medium** instance type (2 vCPUs, 4GB RAM) to ensure sufficient resources for Jenkins.
5. Configure security groups:
   - Open **port 22** for SSH access (source: your IP address).
   - Open **port 8080** for Jenkins access (source: 0.0.0.0/0 or restrict as per your requirements).
6. Launch the instance and note its public IP address.

### Step 2: Install Docker on the Ubuntu Server
1. SSH into the EC2 instance:
   ```bash
   ssh -i server-key.pem ubuntu@18.207.123.241
   ```
2. Update the package list:
   ```bash
   apt update
   ```
3. Install Docker:
   ```bash
   apt install -y docker.io
   ```
4. Start and enable the Docker service:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
5. Verify Docker is installed:
   ```bash
   docker --version
   Docker version 24.0.7, build 24.0.7-0ubuntu4.1
   ```

### Step 3: Start Jenkins in a Docker Container
1. Pull the Jenkins Docker image:
   ```bash
   docker pull jenkins/jenkins:lts
   ```
2. Run the Jenkins container:
   ```bash
   docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
   ```
   - Port 8080: Used for the Jenkins web interface.
   - Port 50000: Used for Jenkins master and worker node communication.
     
3. Verify the Jenkins container is running:
   ```bash
   docker ps
   ```

### Step 4: Initialize Jenkins
1. Retrieve the initial administrator password:
     ```bash
     sudo cat /var/lib/docker/volumes/jenkins_home/_data/secrets/initialAdminPassword
     ```
2. Open Jenkins in your browser:
   ```
   http://18.207.123.241:8080
   ```
3. Enter the initial administrator password.
4. Install the suggested plugins.
5. Create your first admin user.
6. If you encounter permission issues with the Docker socket, perform the following steps:
   ```bash
   docker exec -it -u 0 <container-id> bash
   ```
   Inside the container:
   ```bash
   chmod 666 /var/run/docker.sock
   ```
7. Complete the setup and access the Jenkins dashboard.
---

## Notes
- Ensure your security group rules allow traffic to ports 22 and 8080.
- Use `t2.medium` or higher instance type for better performance.


