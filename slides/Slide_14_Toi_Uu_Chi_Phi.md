# Tối ưu hóa chi phí trên AWS

## Chiến lược tối ưu hóa chi phí 💰

### 1. Right-sizing Resources
- **Lựa chọn cấu hình phù hợp** với workload
- **Monitor utilization** và adjust accordingly
- **Use CloudWatch metrics** để đánh giá performance

### 2. Leverage Pricing Models
- **Reserved Instances**: 1-3 năm commitment, tiết kiệm 75%
- **Savings Plans**: Flexible pricing cho compute
- **Spot Instances**: Tiết kiệm 90% cho fault-tolerant workloads

### 3. Automation & Scheduling
- **Auto Scaling**: Tự động tăng giảm resources
- **Scheduled scaling**: Bật tắt resources theo lịch
- **Lambda functions**: Serverless để giảm idle time

### 4. Storage Optimization
- **S3 Storage Classes**: Standard, IA, Glacier, Deep Archive
- **Lifecycle policies**: Tự động chuyển đổi storage classes
- **Delete unused snapshots** và volumes

## AWS Cost Management Tools 🛠️

### AWS Cost Explorer
- **Visualize spending patterns** theo service, region, account
- **Forecast future costs** based on usage trends
- **Create custom reports** và save them
- **Identify cost spikes** và unusual spending

### AWS Trusted Advisor
- **Cost optimization recommendations**
- **Idle resources detection**
- **Reserved Instance recommendations**
- **Right-sizing suggestions**

### AWS Budgets
- **Set cost và usage budgets**
- **Email/SNS alerts** khi exceed thresholds
- **Track Reserved Instance utilization**
- **Department-level budget tracking**

### Cost Allocation Tags
- **Track costs by department/project**
- **Consistent tagging strategy**
- **Automated tagging** với CloudFormation

---

© Amazon Web Services, Inc. or its Affiliates.
