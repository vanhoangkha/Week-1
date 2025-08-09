# Cloud Service Models

## Infrastructure as a Service (IaaS)
• **Định nghĩa**: Cung cấp hạ tầng máy tính ảo hóa qua internet
• **AWS Services**: Amazon EC2, Amazon VPC, Amazon EBS
• **Khách hàng quản lý**: Operating System, Runtime, Applications
• **AWS quản lý**: Physical hardware, Hypervisor, Network infrastructure

## Platform as a Service (PaaS)  
• **Định nghĩa**: Cung cấp platform để phát triển và deploy ứng dụng
• **AWS Services**: AWS Elastic Beanstalk, AWS Lambda, Amazon RDS
• **Khách hàng quản lý**: Applications, Data
• **AWS quản lý**: Runtime, Operating System, Infrastructure

## Software as a Service (SaaS)
• **Định nghĩa**: Cung cấp ứng dụng hoàn chỉnh qua internet
• **AWS Services**: Amazon WorkMail, Amazon Chime, Amazon QuickSight
• **Khách hàng quản lý**: User access, Data input
• **AWS quản lý**: Everything else

## Responsibility Matrix
| Component | IaaS | PaaS | SaaS |
|-----------|------|------|------|
| Applications | Customer | Customer | AWS |
| Data | Customer | Customer | Shared |
| Runtime | Customer | AWS | AWS |
| Operating System | Customer | AWS | AWS |
| Infrastructure | AWS | AWS | AWS |
