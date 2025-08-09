# AWS Cost Management Tools

## AWS Cost Explorer
• **Purpose**: Visualize và analyze AWS spending patterns
• **Key Features**:
  - View costs by service, region, account
  - Filter by time period (daily, monthly, yearly)
  - Forecast future costs based on usage trends
  - Create custom reports và save them

• **Use Cases**:
  - Identify cost spikes và unusual spending
  - Track cost optimization efforts
  - Budget planning và forecasting
  - Chargeback to departments/projects

## AWS Trusted Advisor
• **Purpose**: Provide recommendations across 5 categories
• **Categories**:
  1. **Cost Optimization**: Idle resources, reserved instance recommendations
  2. **Performance**: Service limits, over-utilized instances
  3. **Security**: Security groups, IAM use, MFA
  4. **Fault Tolerance**: Backup configurations, multi-AZ deployments
  5. **Service Limits**: Usage approaching service limits

• **Access Levels**:
  - **Basic/Developer**: 7 core checks
  - **Business/Enterprise**: All checks + API access

## Cost Allocation Tags
• **Purpose**: Track costs by department, project, environment
• **Types**:
  - **AWS Generated**: aws:createdBy, aws:cloudformation:stack-name
  - **User Defined**: Department, Project, Environment, Owner

• **Best Practices**:
  - Establish tagging strategy trước khi deploy
  - Use consistent naming conventions
  - Automate tagging với CloudFormation/Terraform
  - Regular audit để ensure compliance

## Reserved Instances & Savings Plans
• **Reserved Instances**:
  - 1 hoặc 3 year commitments
  - Up to 75% savings compared to On-Demand
  - Types: Standard, Convertible, Scheduled

• **Savings Plans**:
  - Flexible pricing model
  - Compute Savings Plans: EC2, Lambda, Fargate
  - EC2 Instance Savings Plans: Specific instance families

## AWS Budgets
• **Types**:
  - **Cost Budgets**: Track spending against budget
  - **Usage Budgets**: Track usage metrics
  - **Reservation Budgets**: Track RI/SP utilization
  - **Savings Plans Budgets**: Track SP utilization

• **Alerting**: Email/SNS notifications khi exceed thresholds
