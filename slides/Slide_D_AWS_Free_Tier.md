# AWS Free Tier Deep Dive

## Always Free (Không giới hạn thời gian)
• **AWS Lambda**: 1 million requests/month + 400,000 GB-seconds compute
• **Amazon DynamoDB**: 25 GB storage + 25 read/write capacity units
• **Amazon SNS**: 1 million publishes + 100,000 HTTP/HTTPS deliveries
• **Amazon SES**: 62,000 emails/month khi gửi từ EC2
• **AWS CloudWatch**: 10 custom metrics + 10 alarms

## 12 Months Free (Từ ngày tạo account)
• **Amazon EC2**: 750 hours/month của t2.micro instance
• **Amazon S3**: 5 GB standard storage + 20,000 GET + 2,000 PUT requests
• **Amazon RDS**: 750 hours/month của db.t2.micro + 20 GB storage
• **Amazon CloudFront**: 50 GB data transfer out + 2 million HTTP requests
• **Elastic Load Balancing**: 750 hours + 15 GB data processing

## Trials (Thời gian ngắn)
• **Amazon SageMaker**: 250 hours/month của ml.t2.medium notebook (2 months)
• **Amazon Redshift**: 750 hours/month của dc2.large node (2 months)
• **Amazon Inspector**: 90-day trial
• **AWS Config**: 2,500 configuration items recorded/month (first 3 months)

## Best Practices để tránh charges
• **Set up Billing Alerts**: Cảnh báo khi gần hết free tier
• **Use AWS Budgets**: Track usage theo service
• **Monitor regularly**: Check AWS Free Tier usage page
• **Understand limits**: Biết rõ giới hạn của từng service
• **Clean up resources**: Xóa resources không dùng
• **Use Cost Explorer**: Analyze spending patterns

## Common Pitfalls
• **Data transfer charges**: Outbound data transfer có phí
• **Storage classes**: Chỉ S3 Standard free, không phải IA hoặc Glacier
• **Multiple instances**: 750h total, không phải per instance
• **Regional limits**: Free tier áp dụng cho specific regions
