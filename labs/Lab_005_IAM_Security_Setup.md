# Lab 005: IAM Security Setup

## Objectives
- Create IAM admin user với MFA
- Create developer group với limited permissions  
- Test access với different users
- Review CloudTrail logs

## Prerequisites
- AWS account với root access
- Mobile device cho MFA setup

## Step 1: Enable MFA cho Root Account
1. **Login** với root account credentials
2. **Navigate** to IAM console → Dashboard
3. **Click** "Add MFA" trong Security Status section
4. **Choose** "Virtual MFA device"
5. **Scan QR code** với authenticator app (Google Authenticator, Authy)
6. **Enter** 2 consecutive MFA codes
7. **Verify** MFA is enabled

## Step 2: Create Admin User
1. **Go to** IAM → Users → Add User
2. **User name**: `admin-user`
3. **Access type**: ✅ AWS Management Console access
4. **Console password**: ✅ Custom password (create strong password)
5. **Require password reset**: ❌ (uncheck)
6. **Next: Permissions**
7. **Attach existing policies**: Select `AdministratorAccess`
8. **Next: Tags** (optional)
9. **Next: Review** → Create User
10. **Download** credentials CSV file

## Step 3: Setup MFA cho Admin User
1. **Login** với admin-user credentials
2. **Go to** IAM → Users → admin-user
3. **Security credentials** tab
4. **Assigned MFA device** → Manage
5. **Virtual MFA device** → Continue
6. **Setup** MFA similar to root account
7. **Test login** với MFA

## Step 4: Create Developer Group
1. **IAM** → Groups → Create New Group
2. **Group name**: `Developers`
3. **Attach Policy**: Create custom policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "cloudwatch:GetMetricStatistics",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:TerminateInstances",
                "ec2:StopInstances",
                "s3:DeleteBucket",
                "s3:DeleteObject"
            ],
            "Resource": "*"
        }
    ]
}
```

## Step 5: Create Developer User
1. **Create user**: `developer-user`
2. **Add to group**: Developers
3. **Test permissions**: Login và verify limited access

## Step 6: Enable CloudTrail
1. **CloudTrail** console → Create trail
2. **Trail name**: `security-audit-trail`
3. **Apply to all regions**: Yes
4. **S3 bucket**: Create new bucket
5. **Log file encryption**: Enable
6. **Create trail**

## Step 7: Test và Verify
1. **Login** với different users
2. **Try various actions** để test permissions
3. **Check CloudTrail** logs trong S3
4. **Verify MFA** requirements

## Verification Checklist
- [ ] Root account has MFA enabled
- [ ] Admin user created với full permissions và MFA
- [ ] Developer group created với limited permissions
- [ ] Developer user can access allowed services only
- [ ] CloudTrail logging all activities
- [ ] All credentials stored securely

## Cleanup (Optional)
- Keep IAM users for future labs
- CloudTrail can be disabled if not needed
- Document all created resources
