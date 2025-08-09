# Lab 011: Architecture Design Exercise

## Objectives
- Design simple 3-tier web application
- Apply AWS Well-Architected principles
- Estimate costs using AWS Calculator
- Present và discuss solutions

## Prerequisites
- Understanding of AWS core services
- Basic web application architecture knowledge
- Access to AWS Pricing Calculator

## Scenario
**Company**: TechStart Vietnam  
**Application**: E-commerce website  
**Requirements**:
- Expected 1000 concurrent users
- 24/7 availability required
- Global customer base (focus on Asia-Pacific)
- Budget constraint: $500/month initially
- Must be scalable for future growth

## Step 1: Requirements Analysis
### Functional Requirements:
- Web frontend for product catalog
- User authentication và registration
- Shopping cart functionality
- Payment processing
- Order management
- Admin dashboard

### Non-Functional Requirements:
- **Availability**: 99.9% uptime
- **Performance**: Page load < 3 seconds
- **Scalability**: Handle traffic spikes (Black Friday)
- **Security**: PCI DSS compliance for payments
- **Cost**: Stay within budget constraints

## Step 2: Architecture Design

### Tier 1: Presentation Layer
**Services to Consider**:
- Amazon CloudFront (CDN)
- Application Load Balancer (ALB)
- Amazon S3 (static content)

**Design Decision**:
```
Frontend: React.js SPA hosted on S3
CDN: CloudFront for global distribution
Load Balancer: ALB for high availability
```

### Tier 2: Application Layer
**Services to Consider**:
- Amazon EC2
- AWS Lambda
- Amazon ECS/EKS
- AWS Elastic Beanstalk

**Design Decision**:
```
Compute: EC2 instances với Auto Scaling
Container: ECS for microservices
API Gateway: For REST APIs
```

### Tier 3: Data Layer
**Services to Consider**:
- Amazon RDS
- Amazon DynamoDB
- Amazon ElastiCache
- Amazon S3

**Design Decision**:
```
Primary DB: RDS MySQL (Multi-AZ)
Cache: ElastiCache Redis
File Storage: S3 for product images
```

## Step 3: Apply Well-Architected Principles

### 1. Operational Excellence
```
- Infrastructure as Code: CloudFormation templates
- Monitoring: CloudWatch + X-Ray for tracing
- Automation: CI/CD pipeline với CodePipeline
- Documentation: Architecture diagrams và runbooks
```

### 2. Security
```
- Network: VPC với private/public subnets
- Access: IAM roles và policies
- Data: Encryption at rest và in transit
- Monitoring: GuardDuty + CloudTrail
```

### 3. Reliability
```
- Multi-AZ deployment
- Auto Scaling Groups
- Database backups và point-in-time recovery
- Health checks và automatic failover
```

### 4. Performance Efficiency
```
- CloudFront for content delivery
- ElastiCache for database caching
- Auto Scaling based on metrics
- Right-sizing instances
```

### 5. Cost Optimization
```
- Reserved Instances for predictable workloads
- Spot Instances for batch processing
- S3 lifecycle policies
- Regular cost reviews
```

### 6. Sustainability
```
- Use managed services to reduce overhead
- Optimize resource utilization
- Choose efficient instance types
- Implement auto-shutdown for dev environments
```

## Step 4: Detailed Architecture Diagram

### Create Architecture Using Draw.io hoặc Lucidchart:
```
Internet Gateway
    ↓
CloudFront (Global CDN)
    ↓
Application Load Balancer
    ↓
┌─────────────────────────────────────┐
│ Public Subnet (AZ-1a)  │ Public Subnet (AZ-1b) │
│ NAT Gateway            │ NAT Gateway            │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ Private Subnet (AZ-1a) │ Private Subnet (AZ-1b) │
│ EC2 Web Servers        │ EC2 Web Servers        │
│ Auto Scaling Group     │ Auto Scaling Group     │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ Private Subnet (AZ-1a) │ Private Subnet (AZ-1b) │
│ RDS MySQL (Primary)    │ RDS MySQL (Standby)    │
│ ElastiCache Redis      │ ElastiCache Redis      │
└─────────────────────────────────────┘
```

## Step 5: Cost Estimation

### Using AWS Pricing Calculator:
1. **Go to**: https://calculator.aws/
2. **Add services** với following specifications:

#### Compute (EC2):
```
- Instance Type: t3.medium
- Quantity: 2 instances (minimum)
- Operating System: Amazon Linux
- Utilization: 100%
- Pricing Model: On-Demand initially
```

#### Load Balancer:
```
- Type: Application Load Balancer
- Number of LCUs: 1
- Data processed: 1 GB/hour
```

#### Database (RDS):
```
- Engine: MySQL
- Instance: db.t3.micro
- Multi-AZ: Yes
- Storage: 20 GB General Purpose SSD
```

#### Storage (S3):
```
- Standard Storage: 50 GB
- Requests: 10,000 GET, 1,000 PUT
```

#### CDN (CloudFront):
```
- Data Transfer Out: 100 GB/month
- HTTP Requests: 1,000,000/month
```

#### Cache (ElastiCache):
```
- Node Type: cache.t3.micro
- Number of Nodes: 1
```

### Expected Monthly Cost Breakdown:
```
EC2 Instances (2x t3.medium): ~$60
RDS MySQL (Multi-AZ): ~$25
Application Load Balancer: ~$20
ElastiCache: ~$15
S3 Storage: ~$5
CloudFront: ~$10
Data Transfer: ~$15
NAT Gateway: ~$45
Total: ~$195/month
```

## Step 6: Alternative Cost-Optimized Design

### For Budget-Conscious Approach:
```
Frontend: S3 + CloudFront
Backend: Lambda + API Gateway (serverless)
Database: DynamoDB (serverless)
Authentication: Cognito
File Storage: S3
```

### Serverless Cost Estimation:
```
Lambda: ~$20/month
API Gateway: ~$15/month
DynamoDB: ~$25/month
Cognito: ~$5/month
S3 + CloudFront: ~$15/month
Total: ~$80/month
```

## Step 7: Implementation Plan

### Phase 1 (Month 1-2): MVP
- [ ] Set up basic 3-tier architecture
- [ ] Deploy simple web application
- [ ] Configure monitoring và logging
- [ ] Implement basic security measures

### Phase 2 (Month 3-4): Enhancement
- [ ] Add auto scaling
- [ ] Implement caching layer
- [ ] Set up CI/CD pipeline
- [ ] Add comprehensive monitoring

### Phase 3 (Month 5-6): Optimization
- [ ] Performance tuning
- [ ] Cost optimization
- [ ] Security hardening
- [ ] Disaster recovery setup

## Step 8: Risk Assessment

### Technical Risks:
- **Single point of failure**: Mitigated by Multi-AZ deployment
- **Performance bottlenecks**: Addressed with caching và CDN
- **Security vulnerabilities**: Regular security audits

### Business Risks:
- **Cost overrun**: Implement budget alerts và monitoring
- **Scalability issues**: Design for horizontal scaling
- **Vendor lock-in**: Use standard technologies where possible

## Step 9: Presentation Template

### Slide Structure:
1. **Problem Statement** (2 minutes)
2. **Architecture Overview** (5 minutes)
3. **Well-Architected Principles** (3 minutes)
4. **Cost Analysis** (3 minutes)
5. **Implementation Plan** (2 minutes)
6. **Q&A** (5 minutes)

### Key Points to Cover:
- Why this architecture?
- How does it meet requirements?
- What are the trade-offs?
- How will it scale?
- What's the total cost of ownership?

## Step 10: Peer Review Checklist

### Architecture Review:
- [ ] Meets all functional requirements
- [ ] Addresses non-functional requirements
- [ ] Follows Well-Architected principles
- [ ] Cost-effective solution
- [ ] Scalable và maintainable
- [ ] Security best practices applied
- [ ] Disaster recovery considered

### Presentation Review:
- [ ] Clear problem statement
- [ ] Logical flow of information
- [ ] Visual aids effective
- [ ] Time management good
- [ ] Handles questions well

## Bonus Challenges

### Advanced Scenarios:
1. **Global Expansion**: How would you modify for worldwide deployment?
2. **Compliance**: Add GDPR compliance requirements
3. **Microservices**: Break down into microservices architecture
4. **Hybrid Cloud**: Include on-premises integration
5. **AI/ML**: Add recommendation engine using SageMaker

### Cost Optimization Exercise:
- Redesign for 50% cost reduction
- Maintain same performance requirements
- Document trade-offs made

## Learning Outcomes
After completing this lab, you should be able to:
- Design scalable 3-tier web applications on AWS
- Apply Well-Architected Framework principles
- Estimate và optimize AWS costs
- Present technical solutions effectively
- Evaluate architectural trade-offs
- Plan implementation strategies

## Next Steps
- Implement the designed architecture
- Set up monitoring và alerting
- Conduct load testing
- Optimize based on real-world usage
- Document lessons learned
