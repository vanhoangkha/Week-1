# Demo: AWS Budgets

## Lab 000007: Bắt đầu với AWS Budgets 💰

### 1. Tạo Cost Budget
**Mục đích**: Theo dõi tổng chi phí hàng tháng

#### Setup Steps:
1. **AWS Budgets Console** → Create budget
2. **Budget type**: Cost budget
3. **Budget details**:
   - Name: `Monthly-Total-Budget`
   - Period: Monthly
   - Budget amount: $50 (hoặc theo nhu cầu)
4. **Configure alerts**:
   - 80% threshold: $40
   - 100% threshold: $50
   - 120% threshold: $60
5. **Email notifications**: Nhập email address

### 2. Tạo Usage Budget
**Mục đích**: Theo dõi usage của specific service

#### EC2 Usage Budget:
- **Service**: Amazon EC2-Instance
- **Usage type**: Running Hours
- **Unit**: Hours
- **Amount**: 750 hours (Free Tier limit)
- **Filters**: Instance Type = t2.micro

### 3. Tạo Reservation Budget
**Mục đích**: Track Reserved Instance utilization

#### Setup:
- **Budget type**: Reservation budget
- **Service**: Amazon EC2
- **Utilization threshold**: 80%
- **Coverage threshold**: 80%

### 4. Tạo Savings Plans Budget
**Mục đích**: Monitor Savings Plans utilization

#### Configuration:
- **Budget type**: Savings Plans budget
- **Utilization threshold**: 85%
- **Coverage threshold**: 90%

## Budget Best Practices 💡
- **Start với conservative amounts**
- **Set multiple alert thresholds**
- **Review và adjust monthly**
- **Use tags** để track specific projects
- **Enable SNS notifications** cho automation

---

© Amazon Web Services, Inc. or its Affiliates.
