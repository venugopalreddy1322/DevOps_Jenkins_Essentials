
# 🚀 CI Pipeline on AWS EC2 with Jenkins, Terraform & Docker

## 📌 Overview
This project sets up a **CI pipeline** using:
- **Jenkins** for automation
- **Terraform** for infrastructure management
- **Docker** for containerization
- **AWS EC2** for hosting services
- **Python application deployment** using a **Dockerfile & Jenkinsfile**

## ⚙️ Technologies Used
- **AWS EC2** ☁️ – Compute instance for Jenkins
- **Jenkins** 🔧 – CI automation tool
- **Terraform** 🏗️ – Infrastructure as Code (IaC)
- **Docker** 🐳 – Container management
- **GitHub & Docker Hub** 🔑 – Repository & image storage
- **Python** 🐍 – Application backend

---

## 🛠️ Prerequisites

Before running this setup, ensure you have:
- An **AWS account** with EC2 access
- **IAM roles** with necessary permissions (instead of using aws acess-keys `aws configure`)
- SSH access to your EC2 instance

### **Required Softwares**
Install the following on **EC2**:

1. **Terraform** 🏗️:
   ```bash
   sudo yum install -y yum-utils
   sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
   sudo yum install -y terraform
   terraform -v  # Verify installation
   

Docker 🐳:
### Docker Installation
To install Docker on Amazon Linux:

```bash
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
docker --version  # Verify installation


Jenkins 🔧:

```bash
sudo yum install -y java-11-amazon-corretto
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum install -y jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

Install Necessary Jenkins Plugins

Terraform Plugin

Docker Plugin

EC2 Plugin

Configure credentials for GitHub & Docker Hub within Jenkins

📋 Terraform Configuration
Create a terraform directory in your project:

```bash
mkdir terraform && cd terraform
touch main.tf variables.tf outputs.tf

Terraform Script (main.tf)
```hcl

resource "aws_instance" "Webserver" {
  ami           = data.aws_ami.amz2ami.id
  instance_type = var.instance_type
  key_name      = var.keypair
  user_data     = file("userdata-jenkins.sh")
  vpc_security_group_ids = [aws_security_group.allow_http_ssh.id]

  root_block_device {
    volume_size = 16  # Default Free Tier size
    volume_type = "gp2"  # Free Tier eligible
  }

  tags = {
    "Name" = "Jenkins-Server-Amz-Linux"
  }
}
Initialize & Apply Terraform
```bash
terraform init
terraform plan
terraform apply -auto-approve

🏗️ Application Deployment: Python App
This project includes:

app.py (Python application)

requirements.txt (Dependencies)

Dockerfile (Container configuration)

Jenkinsfile (CI pipeline)

Dockerfile
```dockerfile

FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app.py .
CMD ["python", "app.py"]

Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push my-python-app'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 my-python-app'
            }
        }
    }
}
🔗 Accessing Jenkins
Once Jenkins is installed, access it via:

http://<elastic-ip>:8080/
Use ``` sudo cat /var/lib/jenkins/secrets/initialAdminPassword ``` to retrieve the admin password.

🚀 Start & Stop EC2 Instance Using Terraform
Terraform can manage instance states:

Stop EC2 Instance
```hcl
resource "aws_ec2_instance_state" "stop_instance" {
  instance_id = aws_instance.Webserver.id
  state       = "stopped"
}

Start EC2 Instance
```hcl
resource "aws_ec2_instance_state" "start_instance" {
  instance_id = aws_instance.Webserver.id
  state       = "running"
}
📄 License
This project is licensed under MIT License. See LICENSE.md for details.

📝 Contributors
Venu-DevOps


🎯 Next Steps
Implement CloudWatch monitoring for instance performance
