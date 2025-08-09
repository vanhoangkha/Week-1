# AWS Security Fundamentals

## AWS Shared Responsibility Model
• **AWS Responsibility (Security OF the Cloud)**:
  - Physical security của data centers
  - Hardware và software infrastructure
  - Network controls và host operating system patching
  - Hypervisor và managed services

• **Customer Responsibility (Security IN the Cloud)**:
  - Guest operating system updates và security patches
  - Application software và utilities
  - Configuration của AWS security groups
  - Identity và access management

## AWS Identity and Access Management (IAM)
• **Users**: Individual people hoặc applications
• **Groups**: Collection của users với shared permissions
• **Roles**: Set của permissions có thể assume bởi users/services
• **Policies**: JSON documents định nghĩa permissions

### IAM Best Practices
• **Principle of Least Privilege**: Chỉ cấp minimum permissions cần thiết
• **Use Groups**: Assign permissions to groups, add users to groups
• **Enable MFA**: Multi-Factor Authentication cho sensitive operations
• **Rotate credentials**: Thay đổi passwords và access keys thường xuyên
• **Use roles**: Cho EC2 instances và cross-account access

## Root Account Protection
• **Không sử dụng root account** cho daily activities
• **Enable MFA** cho root account
• **Create IAM admin user** cho administrative tasks
• **Secure root credentials**: Store safely, don't share
• **Monitor root usage**: Set up CloudTrail alerts

## AWS CloudTrail
• **Purpose**: Log tất cả API calls trong AWS account
• **Benefits**:
  - Security analysis và compliance auditing
  - Track changes to AWS resources
  - Troubleshooting operational issues
• **Best Practices**:
  - Enable cho tất cả regions
  - Store logs trong S3 với encryption
  - Set up log file integrity validation

## Network Security
• **Security Groups**: Virtual firewalls cho EC2 instances
  - Stateful: Return traffic automatically allowed
  - Default: Deny all inbound, allow all outbound
• **Network ACLs**: Subnet-level firewalls
  - Stateless: Must explicitly allow return traffic
  - Default: Allow all traffic
