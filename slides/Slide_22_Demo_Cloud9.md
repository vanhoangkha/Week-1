# Demo: AWS Cloud9

## Lab 000049: Bắt đầu với AWS Cloud9 ☁️💻

### 1. Tạo Cloud9 Environment
**AWS Cloud9** là cloud-based IDE với integrated terminal

#### Setup Steps:
1. **Cloud9 Console** → Create environment
2. **Environment details**:
   - Name: `my-development-environment`
   - Description: `Development environment for AWS learning`
3. **Environment settings**:
   - Instance type: t2.micro (Free Tier eligible)
   - Platform: Amazon Linux 2
   - Cost-saving: Hibernate after 30 minutes
4. **Network settings**: Default VPC và subnet

### 2. Tính năng cơ bản 🛠️

#### Code Editor
- **Syntax highlighting** cho multiple languages
- **Code completion** và IntelliSense
- **File explorer** với project structure
- **Multiple tabs** cho different files

#### Integrated Terminal
- **Full Linux terminal** với sudo access
- **Pre-installed tools**: git, docker, node, python
- **AWS CLI** đã configured với IAM permissions
- **Multiple terminal tabs**

#### Collaboration Features
- **Real-time collaboration** với team members
- **Share environment** với read/write access
- **Chat functionality** built-in

### 3. Sử dụng AWS CLI 💻

#### Basic Commands Demo:
```bash
# Check AWS CLI version
aws --version

# Get current identity
aws sts get-caller-identity

# List S3 buckets
aws s3 ls

# List EC2 instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name]' --output table
```

#### Development Workflow:
1. **Write code** trong editor
2. **Test locally** trong terminal
3. **Deploy to AWS** using CLI/SDK
4. **Debug và iterate**

## Cloud9 vs CloudShell 🤔
- **Cloud9**: Full IDE với persistent environment
- **CloudShell**: Browser terminal với temporary sessions
- **Use Cloud9**: Cho development projects
- **Use CloudShell**: Cho quick CLI tasks

---

© Amazon Web Services, Inc. or its Affiliates.
