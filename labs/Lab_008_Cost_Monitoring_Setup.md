# Lab 008: Cost Monitoring Setup

## Objectives
- Configure AWS Cost Explorer
- Create cost allocation tags
- Set up department-based cost tracking
- Generate cost reports và budgets

## Prerequisites
- AWS account với billing access
- IAM permissions for billing và cost management

## Step 1: Enable Cost Explorer
1. **Navigate** to AWS Billing Console
2. **Cost Explorer** → Get Started
3. **Enable Cost Explorer** (may take up to 24 hours for data)
4. **Explore** default reports:
   - Monthly costs by service
   - Daily costs
   - Reserved Instance utilization

## Step 2: Create Cost Allocation Tags
1. **Billing Console** → Cost Allocation Tags
2. **Create tags** for tracking:

### User-Defined Tags to Create:
```
Department: IT, Marketing, Sales, HR
Project: WebApp, DataPipeline, Analytics
Environment: Production, Staging, Development
Owner: john.doe, jane.smith, team.lead
CostCenter: CC001, CC002, CC003
```

3. **Activate tags**:
   - Select all created tags
   - Click "Activate"
   - Note: Takes 24 hours to appear trong reports

## Step 3: Tag Existing Resources
```bash
# Using AWS CLI trong CloudShell
# Tag an S3 bucket
aws s3api put-bucket-tagging --bucket YOUR_BUCKET_NAME --tagging 'TagSet=[
  {Key=Department,Value=IT},
  {Key=Project,Value=WebApp},
  {Key=Environment,Value=Production},
  {Key=Owner,Value=admin-user}
]'

# Tag EC2 instances (if any exist)
aws ec2 create-tags --resources i-1234567890abcdef0 --tags Key=Department,Value=IT Key=Project,Value=WebApp

# Verify tags
aws s3api get-bucket-tagging --bucket YOUR_BUCKET_NAME
```

## Step 4: Create Cost Budget
1. **AWS Budgets** → Create budget
2. **Budget type**: Cost budget
3. **Budget details**:
   - Name: `Monthly-IT-Department-Budget`
   - Period: Monthly
   - Budget amount: $100
   - Budget scope: 
     - Filter by tags: Department = IT

4. **Configure alerts**:
   - Alert 1: 80% of budget ($80)
   - Alert 2: 100% of budget ($100)
   - Alert 3: 120% of budget ($120)
   - Email: your-email@company.com

## Step 5: Create Usage Budget
1. **Create budget** → Usage budget
2. **Service**: Amazon EC2-Instance
3. **Usage type**: Running Hours
4. **Unit**: Hours
5. **Amount**: 750 hours (Free Tier limit)
6. **Filters**: Instance Type = t2.micro
7. **Alerts**: 80%, 100% thresholds

## Step 6: Cost Explorer Reports
### Create Custom Report 1: Monthly Cost by Department
1. **Cost Explorer** → Create report
2. **Report name**: Monthly Cost by Department
3. **Time range**: Last 3 months
4. **Granularity**: Monthly
5. **Group by**: Tag (Department)
6. **Save report**

### Create Custom Report 2: Service Cost Breakdown
1. **Create report**: Service Cost Analysis
2. **Group by**: Service
3. **Filter**: Remove $0 costs
4. **Chart type**: Bar chart
5. **Save report**

## Step 7: Set Up Cost Anomaly Detection
1. **Cost Anomaly Detection** → Create monitor
2. **Monitor name**: IT-Department-Anomaly
3. **Monitor type**: Linked account
4. **Dimension**: Service
5. **Match options**: Department tag = IT
6. **Alert subscription**:
   - Threshold: $10
   - Frequency: Daily
   - Recipients: your-email@company.com

## Step 8: Create Cost Dashboard
1. **Cost Explorer** → Saved reports
2. **Create dashboard** combining:
   - Monthly costs by service
   - Daily costs trend
   - Cost by department
   - Top 5 services by cost

## Step 9: Set Up Billing Alerts (CloudWatch)
```bash
# Create SNS topic for billing alerts
aws sns create-topic --name billing-alerts --region us-east-1

# Subscribe email to topic
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:ACCOUNT-ID:billing-alerts --protocol email --notification-endpoint your-email@company.com

# Create CloudWatch alarm for billing
aws cloudwatch put-metric-alarm \
  --alarm-name "Billing Alert - $50" \
  --alarm-description "Alert when charges exceed $50" \
  --metric-name EstimatedCharges \
  --namespace AWS/Billing \
  --statistic Maximum \
  --period 86400 \
  --threshold 50 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=Currency,Value=USD \
  --evaluation-periods 1 \
  --alarm-actions arn:aws:sns:us-east-1:ACCOUNT-ID:billing-alerts \
  --region us-east-1
```

## Step 10: Generate Cost Report
1. **Cost và Usage Reports** → Create report
2. **Report name**: Detailed-Monthly-Report
3. **Time granularity**: Daily
4. **Report versioning**: Overwrite existing report
5. **Enable**: Resource IDs, Split cost allocation data
6. **S3 bucket**: Create new bucket for reports
7. **Report path prefix**: cost-reports/
8. **Compression**: GZIP

## Verification Tasks
- [ ] Cost Explorer enabled và showing data
- [ ] Cost allocation tags created và activated
- [ ] Resources tagged appropriately
- [ ] Cost budget created với alerts
- [ ] Usage budget created
- [ ] Custom Cost Explorer reports saved
- [ ] Cost anomaly detection configured
- [ ] CloudWatch billing alarms set up
- [ ] Cost và Usage Reports configured
- [ ] Email notifications working

## Best Practices Implemented
- **Consistent tagging strategy** across all resources
- **Multiple budget types** (cost và usage)
- **Proactive alerting** at different thresholds
- **Regular reporting** for cost analysis
- **Anomaly detection** for unusual spending
- **Granular tracking** by department/project

## Sample Cost Analysis Queries
```bash
# Get cost by service for last month
aws ce get-cost-and-usage \
  --time-period Start=2024-01-01,End=2024-01-31 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=DIMENSION,Key=SERVICE

# Get cost by tag (Department)
aws ce get-cost-and-usage \
  --time-period Start=2024-01-01,End=2024-01-31 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=TAG,Key=Department
```

## Ongoing Maintenance
- **Weekly**: Review cost trends và anomalies
- **Monthly**: Analyze detailed cost reports
- **Quarterly**: Review và adjust budgets
- **Annually**: Optimize tagging strategy
- **Continuous**: Monitor alerts và take action
