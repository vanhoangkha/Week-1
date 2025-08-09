# Lab 010: Security Best Practices

## Objectives
- Enable CloudTrail logging
- Configure strong password policy
- Set up billing alerts
- Review Trusted Advisor security recommendations

## Prerequisites
- AWS account với administrative access
- Completed Lab 005 (IAM Security Setup)

## Step 1: Enable CloudTrail (Enhanced Setup)
1. **CloudTrail Console** → Create trail
2. **Trail name**: `comprehensive-audit-trail`
3. **Apply trail to all regions**: ✅ Yes
4. **Management events**: ✅ Read/Write events
5. **Data events**: Configure for S3 buckets
6. **Insights events**: ✅ Enable (for unusual activity patterns)

### S3 Configuration:
```bash
# Create dedicated S3 bucket for CloudTrail
aws s3 mb s3://your-org-cloudtrail-logs-$(date +%s) --region us-east-1

# Enable versioning
aws s3api put-bucket-versioning --bucket your-bucket-name --versioning-configuration Status=Enabled

# Configure lifecycle policy
cat > lifecycle-policy.json << 'EOF'
{
    "Rules": [
        {
            "ID": "CloudTrailLogRetention",
            "Status": "Enabled",
            "Transitions": [
                {
                    "Days": 30,
                    "StorageClass": "STANDARD_IA"
                },
                {
                    "Days": 90,
                    "StorageClass": "GLACIER"
                }
            ]
        }
    ]
}
EOF

aws s3api put-bucket-lifecycle-configuration --bucket your-bucket-name --lifecycle-configuration file://lifecycle-policy.json
```

## Step 2: Configure IAM Password Policy
1. **IAM Console** → Account settings
2. **Password policy** → Set requirements:
   - Minimum length: 12 characters
   - ✅ Require uppercase letters
   - ✅ Require lowercase letters  
   - ✅ Require numbers
   - ✅ Require symbols
   - ✅ Allow users to change password
   - Password expiration: 90 days
   - Prevent password reuse: 5 passwords
   - ✅ Require administrator reset if expired

### Using CLI:
```bash
aws iam update-account-password-policy \
  --minimum-password-length 12 \
  --require-symbols \
  --require-numbers \
  --require-uppercase-characters \
  --require-lowercase-characters \
  --allow-users-to-change-password \
  --max-password-age 90 \
  --password-reuse-prevention 5 \
  --hard-expiry
```

## Step 3: Enable AWS Config
1. **AWS Config Console** → Get started
2. **Resource types**: ✅ Record all resources
3. **Amazon S3 bucket**: Create new bucket
4. **Amazon SNS topic**: Create new topic
5. **AWS Config role**: Create new role

### Key Rules to Enable:
```bash
# Enable important Config rules
aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "root-mfa-enabled",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "ROOT_MFA_ENABLED"
  }
}'

aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "iam-password-policy",
  "Source": {
    "Owner": "AWS", 
    "SourceIdentifier": "IAM_PASSWORD_POLICY"
  }
}'

aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "cloudtrail-enabled",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "CLOUD_TRAIL_ENABLED"
  }
}'
```

## Step 4: Set Up GuardDuty
1. **GuardDuty Console** → Get Started
2. **Enable GuardDuty** → Enable
3. **Settings** → Configure:
   - Sample findings: Generate
   - Finding export: Configure S3 bucket
   - Malware protection: Enable for EC2

### Create Custom Threat List:
```bash
# Create IP threat list
cat > threat-ips.txt << 'EOF'
192.0.2.0/24
198.51.100.0/24
203.0.113.0/24
EOF

# Upload to S3
aws s3 cp threat-ips.txt s3://your-security-bucket/threat-lists/

# Add threat list to GuardDuty
aws guardduty create-threat-intel-set \
  --detector-id YOUR_DETECTOR_ID \
  --name "Custom-Threat-IPs" \
  --format TXT \
  --location s3://your-security-bucket/threat-lists/threat-ips.txt \
  --activate
```

## Step 5: Security Hub Setup
1. **Security Hub Console** → Enable Security Hub
2. **Security standards**: Enable all available standards:
   - AWS Foundational Security Standard
   - CIS AWS Foundations Benchmark
   - PCI DSS
3. **Integrations**: Enable GuardDuty, Config, Inspector

## Step 6: Create Security Monitoring Dashboard
### CloudWatch Dashboard:
```bash
# Create security metrics dashboard
aws cloudwatch put-dashboard --dashboard-name "Security-Monitoring" --dashboard-body '{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          ["AWS/Events", "SuccessfulInvocations", "RuleName", "GuardDutyFindings"]
        ],
        "period": 300,
        "stat": "Sum",
        "region": "us-east-1",
        "title": "GuardDuty Findings"
      }
    }
  ]
}'
```

## Step 7: Set Up Security Alerts
### SNS Topic for Security Alerts:
```bash
# Create SNS topic
aws sns create-topic --name security-alerts

# Subscribe email
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:ACCOUNT-ID:security-alerts \
  --protocol email \
  --notification-endpoint security-team@company.com

# Create EventBridge rule for GuardDuty findings
aws events put-rule \
  --name "GuardDuty-High-Severity" \
  --event-pattern '{
    "source": ["aws.guardduty"],
    "detail-type": ["GuardDuty Finding"],
    "detail": {
      "severity": [7.0, 8.0, 8.9]
    }
  }'

# Add SNS target
aws events put-targets \
  --rule "GuardDuty-High-Severity" \
  --targets "Id"="1","Arn"="arn:aws:sns:us-east-1:ACCOUNT-ID:security-alerts"
```

## Step 8: Trusted Advisor Security Review
1. **Trusted Advisor Console** → Security
2. **Review all security checks**:
   - Security Groups - Specific Ports Unrestricted
   - IAM Use
   - MFA on Root Account
   - IAM Access Key Rotation
   - CloudTrail Logging

### Automate Trusted Advisor Checks:
```bash
# Get Trusted Advisor checks (requires Business/Enterprise support)
aws support describe-trusted-advisor-checks --language en

# Get specific security check results
aws support describe-trusted-advisor-check-result \
  --check-id "Pfx0RwqBli" \
  --language en
```

## Step 9: Access Analyzer Setup
1. **IAM Console** → Access Analyzer
2. **Create analyzer**:
   - Name: `organization-analyzer`
   - Type: Account analyzer
   - Zone of trust: Current account

## Step 10: Security Compliance Checklist
### Create automated compliance check:
```bash
#!/bin/bash
# Security compliance check script

echo "=== AWS Security Compliance Check ==="

# Check MFA on root account
echo "1. Checking root MFA status..."
aws iam get-account-summary | jq '.SummaryMap.AccountMFAEnabled'

# Check password policy
echo "2. Checking password policy..."
aws iam get-account-password-policy

# Check CloudTrail status
echo "3. Checking CloudTrail status..."
aws cloudtrail describe-trails | jq '.trailList[].IsMultiRegionTrail'

# Check Config status
echo "4. Checking Config status..."
aws configservice describe-configuration-recorders

# Check GuardDuty status
echo "5. Checking GuardDuty status..."
aws guardduty list-detectors

# Check for public S3 buckets
echo "6. Checking for public S3 buckets..."
aws s3api list-buckets --query 'Buckets[].Name' --output text | while read bucket; do
  public=$(aws s3api get-bucket-acl --bucket $bucket --query 'Grants[?Grantee.URI==`http://acs.amazonaws.com/groups/global/AllUsers`]' --output text)
  if [ ! -z "$public" ]; then
    echo "WARNING: Bucket $bucket has public access"
  fi
done

echo "=== Compliance Check Complete ==="
```

## Verification Tasks
- [ ] CloudTrail enabled với multi-region logging
- [ ] Strong password policy configured
- [ ] AWS Config enabled với key security rules
- [ ] GuardDuty enabled và configured
- [ ] Security Hub enabled với standards
- [ ] Security monitoring dashboard created
- [ ] Security alert notifications configured
- [ ] Trusted Advisor security checks reviewed
- [ ] Access Analyzer configured
- [ ] Compliance check script created và tested

## Security Monitoring Routine
### Daily:
- Review GuardDuty findings
- Check Security Hub compliance score
- Monitor CloudTrail unusual activities

### Weekly:
- Review Trusted Advisor recommendations
- Analyze Access Analyzer findings
- Update threat intelligence lists

### Monthly:
- Rotate access keys
- Review IAM permissions
- Update security policies
- Conduct access reviews

## Incident Response Preparation
```bash
# Create incident response runbook
cat > incident-response.md << 'EOF'
# Security Incident Response Runbook

## Immediate Actions (0-15 minutes)
1. Assess severity level
2. Isolate affected resources
3. Notify security team
4. Document timeline

## Investigation (15-60 minutes)
1. Review CloudTrail logs
2. Check GuardDuty findings
3. Analyze network traffic
4. Identify root cause

## Containment (1-4 hours)
1. Disable compromised accounts
2. Rotate credentials
3. Apply security patches
4. Update security groups

## Recovery (4-24 hours)
1. Restore from backups
2. Verify system integrity
3. Monitor for reoccurrence
4. Update security measures
EOF
```

## Next Steps
- Implement automated remediation
- Set up security training program
- Regular security assessments
- Penetration testing schedule
