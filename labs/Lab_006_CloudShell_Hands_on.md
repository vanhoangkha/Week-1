# Lab 006: CloudShell Hands-on

## Objectives
- Access CloudShell từ AWS Console
- Run basic AWS CLI commands
- Create và manage files
- Install additional tools

## Prerequisites
- AWS account với IAM user permissions
- Basic command line knowledge

## Step 1: Access CloudShell
1. **Login** to AWS Management Console
2. **Look for** CloudShell icon (terminal icon) trong top navigation bar
3. **Click** CloudShell icon
4. **Wait** for environment to initialize (~30 seconds)
5. **Verify** you see bash prompt: `[cloudshell-user@ip-xxx-xxx-xxx-xxx ~]$`

## Step 2: Basic AWS CLI Commands
```bash
# Check AWS CLI version
aws --version

# Verify current identity
aws sts get-caller-identity

# List S3 buckets
aws s3 ls

# List EC2 instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]' --output table

# Get current region
aws configure get region

# List available regions
aws ec2 describe-regions --query 'Regions[*].RegionName' --output table
```

## Step 3: File Management
```bash
# Check current directory
pwd

# List files (should be empty initially)
ls -la

# Create a directory structure
mkdir -p projects/aws-scripts
mkdir -p projects/terraform
mkdir -p projects/cloudformation

# Create a simple script
cat > projects/aws-scripts/list-resources.sh << 'EOF'
#!/bin/bash
echo "=== AWS Resources Summary ==="
echo "S3 Buckets:"
aws s3 ls
echo ""
echo "EC2 Instances:"
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name]' --output table
echo ""
echo "IAM Users:"
aws iam list-users --query 'Users[*].UserName' --output table
EOF

# Make script executable
chmod +x projects/aws-scripts/list-resources.sh

# Run the script
./projects/aws-scripts/list-resources.sh
```

## Step 4: Working với Git
```bash
# Configure git (replace với your info)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Clone a sample repository
git clone https://github.com/aws-samples/aws-cli-examples.git

# Explore the repository
cd aws-cli-examples
ls -la
cat README.md
```

## Step 5: Working với JSON và jq
```bash
# Get EC2 instance information as JSON
aws ec2 describe-instances > instances.json

# Use jq to parse JSON
cat instances.json | jq '.Reservations[].Instances[] | {InstanceId, State, InstanceType}'

# Get only running instances
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" | jq '.Reservations[].Instances[] | .InstanceId'

# Create a formatted report
aws ec2 describe-instances | jq -r '.Reservations[].Instances[] | "\(.InstanceId) \(.State.Name) \(.InstanceType)"' > instance-report.txt
cat instance-report.txt
```

## Step 6: File Upload/Download
1. **Create a local file** trên máy tính của bạn với nội dung:
```
This is a test file from my local machine
Created for CloudShell lab
```

2. **Upload file** to CloudShell:
   - Click **Actions** → **Upload file**
   - Select your local file
   - File sẽ appear trong current directory

3. **Download file** from CloudShell:
   - Create a file: `echo "CloudShell generated file" > download-test.txt`
   - Click **Actions** → **Download file**
   - Enter filename: `download-test.txt`

## Step 7: Install Additional Tools
```bash
# Install Python packages (user level)
pip3 install --user boto3 requests

# Verify installation
python3 -c "import boto3; print('boto3 version:', boto3.__version__)"

# Create a simple Python script
cat > s3-list.py << 'EOF'
import boto3

s3 = boto3.client('s3')
response = s3.list_buckets()

print("S3 Buckets:")
for bucket in response['Buckets']:
    print(f"  - {bucket['Name']} (Created: {bucket['CreationDate']})")
EOF

# Run Python script
python3 s3-list.py
```

## Step 8: Persistent Storage Test
```bash
# Create a file trong home directory
echo "This file should persist across sessions" > persistent-test.txt
date >> persistent-test.txt

# Check storage usage
du -sh ~
df -h /home/cloudshell-user
```

## Step 9: Multiple Tabs
1. **Open new tab**: Click **+** button
2. **Run different commands** trong each tab
3. **Switch between tabs** để see different sessions

## Verification Tasks
- [ ] Successfully accessed CloudShell
- [ ] Ran AWS CLI commands successfully
- [ ] Created directory structure và files
- [ ] Used git to clone repository
- [ ] Parsed JSON với jq
- [ ] Uploaded và downloaded files
- [ ] Installed Python packages
- [ ] Created Python script using boto3
- [ ] Verified persistent storage
- [ ] Used multiple tabs

## Best Practices Learned
- Files trong home directory persist across sessions
- Use organized directory structure
- Leverage pre-installed tools
- Use jq for JSON parsing
- Install additional packages as needed
- Take advantage of multiple tabs

## Cleanup
```bash
# Optional: Clean up test files
rm -f instances.json instance-report.txt download-test.txt persistent-test.txt
rm -rf aws-cli-examples
```

## Next Steps
- Explore other pre-installed tools
- Create more complex scripts
- Use CloudShell for daily AWS tasks
- Integrate với your development workflow
