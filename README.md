# 🤖 CI/CD Pipeline with GitHub Actions

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-blue?logo=githubactions)
![Docker](https://img.shields.io/badge/Docker-29.1.3-blue?logo=docker)
![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Status](https://img.shields.io/badge/Pipeline-Active-brightgreen)
![Auto Deploy](https://img.shields.io/badge/Auto_Deploy-Enabled-green)

---

## 📌 Project Overview

Built a **fully automated CI/CD pipeline** using GitHub Actions that automatically builds, tests, and deploys the three-tier Docker application on every code push to the main branch.

> 🔄 **Push code → Pipeline triggers → Builds → Deploys → Done! All automatic!**

---

## 🏗️ Architecture Diagram

```
Developer (You)
      │
      │  git push origin main
      ▼
┌─────────────────────────────────────────────────┐
│                  GitHub                          │
│                                                  │
│  Repository: -three-tier-docker-app              │
│                    │                             │
│                    │ Triggers on push             │
│                    ▼                             │
│  ┌─────────────────────────────────────────┐    │
│  │         GitHub Actions Runner           │    │
│  │         (ubuntu-latest VM)              │    │
│  │                                         │    │
│  │  Step 1: ✅ Checkout Code               │    │
│  │  Step 2: ✅ Install Docker Compose      │    │
│  │  Step 3: ✅ Build Docker Images         │    │
│  │  Step 4: ✅ Run Containers              │    │
│  │  Step 5: ✅ Verify Running              │    │
│  │  Step 6: ✅ Stop Containers             │    │
│  │                                         │    │
│  │  Result: ✅ GREEN TICK (Success)        │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
-three-tier-docker-app/
│
├── .github/
│   └── workflows/
│       └── deploy.yml          # ⭐ CI/CD Pipeline definition
│
├── docker-compose.yml          # Docker orchestration
├── screenshots/
│   ├── pipeline-running.png    # Yellow circle (in progress)
│   ├── pipeline-success.png    # Green tick (success)
│   └── pipeline-steps.png      # All steps completed
└── README.md                   # This file
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| GitHub Actions | CI/CD automation platform |
| GitHub Workflows (YAML) | Pipeline definition |
| Docker | Container build & run |
| Docker Compose | Multi-container management |
| Ubuntu Latest | GitHub Actions runner OS |
| Git | Version control & trigger |

---

## 📄 Pipeline Source Code

### .github/workflows/deploy.yml

```yaml
name: Three-Tier App CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Install Docker Compose
      run: |
        sudo apt-get update
        sudo apt-get install -y docker-compose

    - name: Build Docker Images
      run: |
        docker compose build

    - name: Run Containers
      run: |
        docker compose up -d

    - name: Check Running Containers
      run: |
        docker compose ps

    - name: Stop Containers
      run: |
        docker compose down
```

---

## 🚀 Setup Steps

### Step 1: Create GitHub Repository

```
1. Go to github.com
2. Click "+" → New Repository
3. Name: -three-tier-docker-app
4. Public ✅
5. No README (we'll add manually)
6. Click Create Repository
```

### Step 2: Create Personal Access Token

```
1. GitHub → Profile Picture → Settings
2. Developer Settings
3. Personal Access Tokens → Tokens (Classic)
4. Generate New Token (Classic)
5. Select these scopes:
   ✅ repo (full control)
   ✅ workflow (GitHub Actions)
   ✅ admin:repo_hook
6. Generate Token
7. COPY IMMEDIATELY! (shown only once)
```

### Step 3: Create GitHub Actions Directory

```bash
cd ~/three-tier-app
mkdir -p .github/workflows
```

### Step 4: Create Pipeline File

```bash
cat > .github/workflows/deploy.yml << 'EOF'
name: Three-Tier App CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Install Docker Compose
      run: |
        sudo apt-get update
        sudo apt-get install -y docker-compose

    - name: Build Docker Images
      run: |
        docker compose build

    - name: Run Containers
      run: |
        docker compose up -d

    - name: Check Running Containers
      run: |
        docker compose ps

    - name: Stop Containers
      run: |
        docker compose down
EOF
```

### Step 5: Push to GitHub

```bash
# Set git config
git config --global user.name "Sayali Magdum"
git config --global user.email "your-email@gmail.com"

# Initialize and push
git init
git add .
git commit -m "Added CI/CD pipeline with GitHub Actions"
git branch -M main

# Add remote with token
git remote add origin https://sayalimagdum0712:YOUR_TOKEN@github.com/sayalimagdum0712/-three-tier-docker-app.git

git push -u origin main
```

### Step 6: Verify Pipeline

```
1. Go to your GitHub repository
2. Click "Actions" tab
3. You'll see pipeline running! 🤖
4. Wait 2-3 minutes
5. Green tick ✅ = Success!
6. Red cross ❌ = Check logs
```

---

## 📊 Pipeline Results

| Step | Status | Description |
|------|--------|-------------|
| Checkout Code | ✅ Pass | Code pulled from GitHub |
| Install Docker Compose | ✅ Pass | Compose installed on runner |
| Build Docker Images | ✅ Pass | All 3 images built |
| Run Containers | ✅ Pass | All 3 containers started |
| Check Running Containers | ✅ Pass | All containers verified |
| Stop Containers | ✅ Pass | Clean shutdown |
| **Overall Pipeline** | ✅ **GREEN** | **Success!** |

---

## 📸 Screenshots

> Add screenshots in `screenshots/` folder:
> - `screenshots/pipeline-running.png` → Yellow circle (in progress)
> - `screenshots/pipeline-success.png` → Green tick
> - `screenshots/pipeline-steps.png` → All steps green

---

## 🔧 How to Trigger Pipeline

```bash
# Pipeline triggers automatically on:
# 1. Every push to main branch
git push origin main

# 2. Every pull request to main branch
git checkout -b new-feature
git push origin new-feature
# Then create PR on GitHub
```

---

## ⚠️ Troubleshooting

| Issue | Fix |
|-------|-----|
| Permission denied pushing | Add `workflow` scope to token |
| Pipeline not triggering | Check branch name matches `main` |
| Docker build failing | Check docker-compose.yml syntax |
| Red cross on pipeline | Click on job → Read error logs |

---

## 💡 CI/CD Concepts Explained

```
CI (Continuous Integration):
→ Every code push is automatically built & tested
→ Catches bugs early before they reach production
→ Team always works with tested code

CD (Continuous Deployment):
→ Tested code is automatically deployed
→ No manual deployment needed
→ Faster delivery to users
```

---

## 🎯 Key Learnings

- GitHub Actions workflow configuration (YAML)
- CI/CD pipeline concepts and implementation
- Automated Docker build and deploy
- GitHub Personal Access Token management
- Branch-based deployment strategies
- Pipeline debugging and troubleshooting

---

## 👩‍💻 Author

**Sayali Magdum**
- 🔗 GitHub: [@sayalimagdum0712](https://github.com/sayalimagdum0712)
- 💼 LinkedIn: [Sayali Magdum](https://linkedin.com/in/sayalimagdum)
- 🎯 Role: Aspiring Cloud & DevOps Engineer

---

# 🏗️ AWS Infrastructure Provisioning with Terraform

![Terraform](https://img.shields.io/badge/Terraform-v1.x-purple?logo=terraform)
![AWS](https://img.shields.io/badge/AWS-ap--south--1-orange?logo=amazonaws)
![Provider](https://img.shields.io/badge/AWS_Provider-v5.100.0-blue)
![Resources](https://img.shields.io/badge/Resources-5_Created-green)
![Status](https://img.shields.io/badge/Status-Applied-brightgreen)

---

## 📌 Project Overview

Provisioned **complete AWS cloud infrastructure using Terraform Infrastructure as Code (IaC)**. Created 5 AWS resources automatically using code — zero manual clicking in AWS Console!

> ⚡ **One command creates everything: `terraform apply`**

---

## 🏗️ Architecture Diagram

```
┌──────────────────────────────────────────────────────────┐
│                  Terraform Code                           │
│            (main.tf + provider.tf + outputs.tf)          │
└─────────────────────┬────────────────────────────────────┘
                      │ terraform apply
                      ▼
┌──────────────────────────────────────────────────────────┐
│              AWS Region: ap-south-1 (Mumbai)             │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │              VPC: sayali-vpc                       │  │
│  │           CIDR Block: 10.0.0.0/16                 │  │
│  │                                                    │  │
│  │   ┌──────────────────────────────────────────┐    │  │
│  │   │    Subnet: sayali-subnet                  │    │  │
│  │   │    CIDR: 10.0.1.0/24                      │    │  │
│  │   │    AZ: ap-south-1a                        │    │  │
│  │   └──────────────────────────────────────────┘    │  │
│  │                                                    │  │
│  │   ┌──────────────────────────────────────────┐    │  │
│  │   │   Security Group: sayali-sg               │    │  │
│  │   │   Inbound: Port 22 (SSH), 80 (HTTP)       │    │  │
│  │   │   Outbound: All traffic                   │    │  │
│  │   └──────────────────────────────────────────┘    │  │
│  └────────────────────────────────────────────────────┘  │
│                         │                                │
│                         │                                │
│  ┌──────────────────────▼───────────────────────────┐   │
│  │    Internet Gateway: sayali-igw                   │   │
│  │    (Connects VPC to Internet)                     │   │
│  └───────────────────────────────────────────────────┘   │
│                                                          │
│  ┌───────────────────────────────────────────────────┐   │
│  │    S3 Bucket: sayali-devops-bucket-2026           │   │
│  │    (Object Storage)                               │   │
│  └───────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
terraform-aws-infra/
│
├── provider.tf          # AWS provider & Terraform configuration
├── main.tf              # AWS resources definition
├── outputs.tf           # Output values after apply
├── .gitignore           # Excludes sensitive files
├── screenshots/
│   ├── terraform-init.png    # terraform init output
│   ├── terraform-plan.png    # terraform plan output
│   ├── terraform-apply.png   # terraform apply success
│   └── aws-resources.png     # Resources in AWS Console
└── README.md            # This file
```

---

## ☁️ AWS Resources Created

| # | Resource | Name | Details | ID |
|---|----------|------|---------|-----|
| 1 | VPC | sayali-vpc | CIDR: 10.0.0.0/16 | vpc-085e6252131192f51 |
| 2 | Subnet | sayali-subnet | CIDR: 10.0.1.0/24, AZ: ap-south-1a | subnet-001979bc8e5ff14d3 |
| 3 | Internet Gateway | sayali-igw | Attached to sayali-vpc | igw-05c349d1ab7eb5da7 |
| 4 | Security Group | sayali-sg | Ports: 22, 80 inbound | sg-0aa78a91f70ca8235 |
| 5 | S3 Bucket | sayali-devops-bucket-2026 | Region: ap-south-1 | sayali-devops-bucket-2026 |

---

## 🛠️ Tech Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Terraform | v1.x | Infrastructure as Code tool |
| AWS Provider (HashiCorp) | v5.100.0 | AWS resource management |
| AWS CLI | v2.x | AWS authentication |
| AWS EC2 | Ubuntu 26.04 | Execution environment |
| Git | Latest | Version control |

---

## ✅ Prerequisites

```bash
✅ AWS Account with IAM user
✅ AWS Access Key & Secret Key
✅ EC2 instance (Ubuntu 26.04 LTS) OR local machine
✅ Internet connection
```

---

## 📄 Source Code

### provider.tf

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}
```

### main.tf

```hcl
# ==========================================
# VPC - Virtual Private Cloud
# ==========================================
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "sayali-vpc"
  }
}

# ==========================================
# Subnet - Network subdivision inside VPC
# ==========================================
resource "aws_subnet" "main" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "ap-south-1a"

  tags = {
    Name = "sayali-subnet"
  }
}

# ==========================================
# Internet Gateway - Internet access for VPC
# ==========================================
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "sayali-igw"
  }
}

# ==========================================
# Security Group - Firewall rules
# ==========================================
resource "aws_security_group" "main" {
  name        = "sayali-sg"
  description = "Allow web traffic"
  vpc_id      = aws_vpc.main.id

  # Allow SSH access
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Allow HTTP access
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Allow all outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "sayali-sg"
  }
}

# ==========================================
# S3 Bucket - Object storage
# ==========================================
resource "aws_s3_bucket" "main" {
  bucket = "sayali-devops-bucket-2026"

  tags = {
    Name = "sayali-bucket"
  }
}
```

### outputs.tf

```hcl
output "vpc_id" {
  description = "ID of the created VPC"
  value       = aws_vpc.main.id
}

output "subnet_id" {
  description = "ID of the created Subnet"
  value       = aws_subnet.main.id
}

output "s3_bucket_name" {
  description = "Name of the created S3 bucket"
  value       = aws_s3_bucket.main.bucket
}
```

### .gitignore

```bash
# Terraform files to exclude from Git
.terraform/                    # Provider binaries (too large)
terraform.tfstate              # State file (contains sensitive data)
terraform.tfstate.backup       # State backup
*.tfvars                       # Variable files (may contain secrets)
.terraform.lock.hcl            # Lock file
```

---

## 🚀 Deployment Steps

### Step 1: Create Project Directory

```bash
mkdir terraform-aws-infra && cd terraform-aws-infra
```

### Step 2: Install Terraform

```bash
sudo snap install terraform --classic
terraform --version
# Expected: Terraform v1.x.x
```

### Step 3: Install AWS CLI

```bash
sudo snap install aws-cli --classic
aws --version
# Expected: aws-cli/2.x.x
```

### Step 4: Get AWS Access Keys

```
1. Login to AWS Console
2. Click your name (top right)
3. Click Security Credentials
4. Scroll to Access Keys section
5. Click Create Access Key
6. Select: Command Line Interface (CLI)
7. Tick confirmation checkbox
8. Click Create Access Key
9. COPY BOTH KEYS IMMEDIATELY!
   (Download CSV file as backup)
```

### Step 5: Configure AWS CLI

```bash
aws configure
```

```
AWS Access Key ID:     YOUR_ACCESS_KEY
AWS Secret Access Key: YOUR_SECRET_KEY
Default region name:   ap-south-1
Default output format: json
```

### Step 6: Test AWS Connection

```bash
aws s3 ls
# No error = Successfully connected to AWS ✅
```

### Step 7: Create Terraform Files

```bash
# Create provider.tf
cat > provider.tf << 'EOF'
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}
EOF

# Create main.tf
cat > main.tf << 'EOF'
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = { Name = "sayali-vpc" }
}

resource "aws_subnet" "main" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "ap-south-1a"
  tags = { Name = "sayali-subnet" }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  tags = { Name = "sayali-igw" }
}

resource "aws_security_group" "main" {
  name   = "sayali-sg"
  vpc_id = aws_vpc.main.id
  ingress { from_port=22; to_port=22; protocol="tcp"; cidr_blocks=["0.0.0.0/0"] }
  ingress { from_port=80; to_port=80; protocol="tcp"; cidr_blocks=["0.0.0.0/0"] }
  egress  { from_port=0;  to_port=0;  protocol="-1";  cidr_blocks=["0.0.0.0/0"] }
  tags = { Name = "sayali-sg" }
}

resource "aws_s3_bucket" "main" {
  bucket = "sayali-devops-bucket-2026"
  tags = { Name = "sayali-bucket" }
}
EOF

# Create outputs.tf
cat > outputs.tf << 'EOF'
output "vpc_id" {
  value = aws_vpc.main.id
}
output "subnet_id" {
  value = aws_subnet.main.id
}
output "s3_bucket_name" {
  value = aws_s3_bucket.main.bucket
}
EOF
```

### Step 8: Initialize Terraform

```bash
terraform init
```

Expected Output:
```
Initializing the backend...
Initializing provider plugins...
- Installing hashicorp/aws v5.100.0...
- Installed hashicorp/aws v5.100.0 (signed by HashiCorp)

Terraform has been successfully initialized! ✅
```

### Step 9: Preview Infrastructure

```bash
terraform plan
```

Expected Output:
```
Terraform will perform the following actions:
  # aws_vpc.main will be created
  # aws_subnet.main will be created
  # aws_internet_gateway.main will be created
  # aws_security_group.main will be created
  # aws_s3_bucket.main will be created

Plan: 5 to add, 0 to change, 0 to destroy ✅
```

### Step 10: Apply Infrastructure

```bash
terraform apply
```

Type `yes` when prompted:
```
Do you want to perform these actions?
Enter a value: yes
```

Expected Output:
```
aws_vpc.main: Creating...
aws_vpc.main: Creation complete after 1s [id=vpc-085e6252131192f51] ✅
aws_internet_gateway.main: Creating...
aws_internet_gateway.main: Creation complete after 1s ✅
aws_subnet.main: Creating...
aws_subnet.main: Creation complete after 1s ✅
aws_s3_bucket.main: Creating...
aws_s3_bucket.main: Creation complete after 2s ✅
aws_security_group.main: Creating...
aws_security_group.main: Creation complete after 2s ✅

Apply complete! Resources: 5 added, 0 changed, 0 destroyed ✅

Outputs:
s3_bucket_name = "sayali-devops-bucket-2026"
subnet_id      = "subnet-001979bc8e5ff14d3"
vpc_id         = "vpc-085e6252131192f51"
```

### Step 11: Push to GitHub

```bash
# Create .gitignore first!
cat > .gitignore << 'EOF'
.terraform/
terraform.tfstate
terraform.tfstate.backup
*.tfvars
.terraform.lock.hcl
EOF

git init
git add .
git commit -m "Added Terraform AWS Infrastructure as Code"
git branch -M main
git remote add origin https://YOUR_TOKEN@github.com/sayalimagdum0712/terraform-aws-infra.git
git push -u origin main
```

---

## 📊 Terraform Output Values

```bash
terraform output
```

```
s3_bucket_name = "sayali-devops-bucket-2026"
subnet_id      = "subnet-001979bc8e5ff14d3"
vpc_id         = "vpc-085e6252131192f51"
```

---

## 🔧 Useful Terraform Commands

```bash
# Initialize working directory
terraform init

# Preview changes (no actual changes)
terraform plan

# Apply infrastructure
terraform apply

# Apply without confirmation prompt
terraform apply -auto-approve

# Destroy all infrastructure
terraform destroy

# Show current state
terraform show

# List all resources in state
terraform state list

# View specific resource
terraform state show aws_vpc.main

# Format code
terraform fmt

# Validate configuration
terraform validate

# View outputs
terraform output
```

---

## 📸 Screenshots

> Add screenshots in `screenshots/` folder:
> - `screenshots/terraform-init.png` → Successful init output
> - `screenshots/terraform-plan.png` → Plan: 5 to add
> - `screenshots/terraform-apply.png` → Apply complete!
> - `screenshots/aws-console.png` → Resources visible in AWS Console

---

## ⚠️ Troubleshooting

| Issue | Fix |
|-------|-----|
| AWS credentials error | Run `aws configure` again |
| Region error | Change `ap-south-1` to your region |
| Bucket name taken | Change S3 bucket name (must be globally unique) |
| Large file push error | Add `.terraform/` to `.gitignore` |
| Permission denied | Check IAM user has required permissions |
| S3 bucket attribute error | Use `.bucket` not `.name` in outputs.tf |

---

## 💡 Terraform Concepts Explained

```
Infrastructure as Code (IaC):
→ Write infrastructure using code files
→ Version control infrastructure
→ Reproducible environments

terraform init:
→ Downloads required providers
→ Sets up working directory

terraform plan:
→ Shows what WILL be created
→ Like a preview before buying

terraform apply:
→ Actually creates resources on AWS
→ Everything provisioned in minutes

terraform destroy:
→ Deletes all created resources
→ Saves costs when not needed
```

---

## 🎯 Key Learnings

- Terraform HCL (HashiCorp Configuration Language)
- AWS VPC, Subnet, Internet Gateway concepts
- Security Group rules (inbound/outbound)
- S3 bucket creation and management
- Terraform state management
- Infrastructure as Code best practices
- Sensitive file management with .gitignore

---

## 👩‍💻 Author

**Sayali Magdum**
- 🔗 GitHub: [@sayalimagdum0712](https://github.com/sayalimagdum0712)
- 💼 LinkedIn: [Sayali Magdum](https://linkedin.com/in/sayalimagdum)
- 🎯 Role: Aspiring Cloud & DevOps Engineer

---

## 📅 Project Date
June 29, 2026
