# devops-assigment
job assigment appscrip

# 🚀 DevOps Assignment: Nginx Deployment using Docker & Terraform

## 📁 Project Overview

This project demonstrates two core DevOps skills:

1. **Dockerize a custom Nginx page and run it locally.**
2. **Deploy an EC2 instance with Nginx using Terraform (IaC) on AWS.**

---

## 🧩 Task 1: Dockerize Nginx with a Custom HTML Page

### ✅ Objective

- Create a Docker image that runs a custom Nginx server using your own `index.html`.
- Run it locally using Docker.
- Optionally, push the image to Docker Hub.

### 📂 Steps

#### 1. Create Project Directory
```bash
mkdir docker-nginx
cd docker-nginx
2. Create index.html
html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
    <title>Docker Nginx Page</title>
</head>
<body>
    <h1>Hello, this is a custom Nginx page running in Docker!</h1>
</body>
</html>

#c3. Create Dockerfile
Dockerfile
Copy
Edit
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
4. Build Docker Image
bash
Copy
Edit
docker build -t my-nginx-app .
5. Run Docker Container
bash
Copy
Edit
docker run -d -p 8080:80 my-nginx-app
6. Test in Browser
Open this URL in your browser:

arduino
Copy
Edit
http://localhost:8080


☁️ Task 2: Launch EC2 with Nginx using Terraform
✅ Objective
Use Terraform to launch an EC2 instance on AWS.

Install Nginx using user_data.

Create a security group to allow SSH and HTTP access.

Output the public IP for browser access.

⚙️ Prerequisites
Terraform installed

AWS CLI installed and configured (aws configure)

AWS account (Free Tier eligible)

Code Editor (e.g., VS Code with Terraform extension)

📂 Steps
1. Create Project Directory
bash
Copy
Edit
mkdir terraform-ec2-nginx
cd terraform-ec2-nginx
2. Create main.tf
hcl
Copy
Edit
provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "allow_ssh_http" {
  name        = "allow_ssh_http"
  description = "Allow SSH and HTTP traffic"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "nginx_server" {
  ami           = "ami-0c02fb55956c7d316" # Amazon Linux 2 AMI
  instance_type = "t2.micro"
  security_groups = [aws_security_group.allow_ssh_http.name]

  user_data = <<-EOF
              #!/bin/bash
              yum update -y
              amazon-linux-extras install nginx1 -y
              systemctl start nginx
              systemctl enable nginx
              EOF

  tags = {
    Name = "Terraform-Nginx-Server"
  }
}

output "public_ip" {
  value = aws_instance.nginx_server.public_ip
}
3. Initialize Terraform
bash
Copy
Edit
terraform init
4. Apply Configuration
bash
Copy
Edit
terraform apply
Type yes when prompted to approve the plan.

5. Access Nginx in Browser
After the instance is created, Terraform will output the public_ip. Open this in your browser:

cpp
Copy
Edit
http://<public_ip>
Example:

cpp
Copy
Edit
http://98.81.216.8/
📸 Suggested Screenshots
Dockerized Nginx running on localhost:8080

Terraform CLI output showing the public IP

AWS Console showing the running EC2 instance

Nginx home page running in the browser via EC2

Folder structure in VS Code

📦 Directory Structure
css
Copy
Edit
.
├── docker-nginx
│   ├── index.html
│   └── Dockerfile
└── terraform-ec2-nginx
    └── main.tf
✅ Conclusion
This project demonstrates:

✅ Docker containerization of Nginx

✅ AWS EC2 provisioning

✅ Infrastructure as Code using Terraform

✅ Web server deployment both locally and in the cloud

✍️ Author
Kamlesh Parihar
🔗 LinkedIn
