# Cloud-1: Automated Deployment of Inception
### Overview
The Cloud-1 project automates the deployment of a WordPress website and its associated infrastructure using cloud services (AWS and GCP). The deployment is orchestrated using Terraform for infrastructure provisioning and Ansible for configuration management. This project ensures that each service (WordPress, MySQL, PHPMyAdmin, etc.) runs in its own isolated Docker container, providing a scalable and secure web hosting environment.

The application is designed to run on both AWS and GCP cloud platforms(check commits), with the deployment process automated for efficiency. This README will guide you through the infrastructure provisioning and deployment process.

### Technologies Used
Terraform: Used for cloud resource provisioning (e.g., virtual machines, static IPs, security groups/firewalls).
Ansible: Automates the configuration and deployment process on provisioned cloud instances.
Docker: Used for containerizing the WordPress application, MySQL database, PHPMyAdmin, etc.
AWS & GCP: The cloud platforms chosen for hosting the infrastructure.
Ubuntu 20.04 LTS: The operating system used on the provisioned instances.
### Prerequisites
Ensure the following are installed and configured:

Terraform for provisioning cloud infrastructure.
Ansible for automating the configuration management and service deployment.
AWS/GCP Account: Cloud provider accounts with sufficient permissions to create instances and manage infrastructure.
SSH Key for accessing cloud instances remotely.
Docker & Docker Compose for managing the containerized services.
### Infrastructure Setup
#### Step 1: Provisioning Cloud Infrastructure
Terraform is used to provision the infrastructure, which includes creating virtual machines, static IPs, and security/firewall configurations.

#### AWS Setup
Terraform provisions the following resources on AWS:

```EC2 Instance:``` A virtual machine to host the application.
```Elastic IP:``` A static IP address for the EC2 instance.
```Security Group:``` Configured to allow traffic on ports 80 (HTTP) and 443 (HTTPS), ensuring the web server is accessible securely. Example AWS-specific Terraform code for provisioning the EC2 instance and allocating an Elastic IP:
### Configuration Management with Ansible
#### Step 2: Configuring the Instance with Ansible
Once the infrastructure is provisioned, Ansible is used to configure the instance. This involves several tasks, including:

Installing Dependencies: Install Docker, Docker Compose, and other necessary utilities.
Folder Structure: Set up the required folder structure for the application (e.g., /home/ubuntu/cloud-1).
Copying Files: The Inception-42 repository (which contains the necessary configurations for WordPress) is copied to the instance.
Running Services: Ansible is used to start the necessary services in containers, including WordPress, MySQL, and PHPMyAdmin. Ansible plays are organized into roles and tasks for modularity and maintainability.
### Application Deployment
#### Step 3: Running the Application
Once the configuration is complete, Ansible continues to run the necessary services:

#### Inventory reminder

Your inventory.ini file should only contain the IPs of the target instance.

```ansible-playbook -i inventory.ini ping.yml ```

```ansible-playbook -i inventory.ini install_docker.yml ```

```ansible-playbook -i inventory.ini install_docker_compose.yml ```

```ansible-playbook -i inventory.ini deploy_project.yml -u ubuntu --private-key=~/.ssh/id_rsa```

🔧 We install rsync, because the synchronize module uses it for SSH-based transfers.

📁 We use the synchronize module to transfer the project to the /home/ubuntu/inception/ location of the target instance.

🐳 We make sure that docker-compose is installed (for security, if not).

🚀 We go to the project folder and run docker compose up -d --build.

