- Deployment (12 questions - 50%)
- Security (14 questions - 71%)
- Refactoring (4 questions - 100%)
- Deployment with AWS Service (26 questions - 81%)
- Monitoring and TroubleShouting (9 question)


- [ ] Domain 1: Deployment 22%
- [ ] Domain 2: Security 26%
- [ ] Domain 3: Development with AWS Services 30%
- [ ] Domain 4: Refactoring 10%
- [ ] Domain 5: Monitoring and Troubleshooting 12%

Chapter notes:

- [ ] IAM + EC2
- [ ] ELB + ASG
- [ ] EBS & EFS
- [ ] RDS, Aurora, ElastiCache
- [ ] Route 53
- [ ] VPC Fundamental
- [ ] S3
- [ ] CLI, SDK, IAM Roles & Policies
- [ ] Advanced S3 & Athena
- [ ] CloudFront
- [X] **ECS, ECR & Fargate**
- [X] **Beanstalk**
- [ ] **CICD: CodeCommit, CodePipeline, CodeBuild, CodeDeploy**
- [X] **CloudFormation**
- [ ] **CloudWatch, X-Ray & CloudTrail**
- [ ] SQS, SNS, Kinesis
- [ ] **Lambda**
- [ ] DynamoDB
- [ ] **API Gateway**
- [ ] SAM
- [ ] Cognito
- [ ] Others: Step Function, AppSync
- [ ] Advanced Identity
- [ ] AWS Security


- SQS
    - VisibilityTimeout
    - DelayQueue
    
+ Deployment
    - deploy sử dụng Elastic Beanstack, Cloud Formation, CodeDeploy, CodePipeline
    - các deployment strategies: all-at-once, rolling, blue-green,...
    - CD/CI workflow với CodeCommit, CodeBuild, CodeDeploy, CodePipeline
    - Container services với ECS

+ Security: in-rest, on-transit encryption, AWS KMS sử dụng master key, envelope key.    
+ Development with AWS Services
    - Học kỹ về APIGateway, Lambda (serverless architect)
    - DynamoDB (thiết kế, index của noSQL DB), query vs scan, cách tính Read/Write Provisioned Throughput
    - DAX, Elastic Cache, các caching strategies: lazy loading, write-through
    - SQS, SNS, Kinesis: pull, push, poll
    - S3 (Access management, CORS)
    - Cognito (Federated Identities, User pool, Identity pool, STS)
    - Exponential Backoff, một số interface đơn giản của SNS, DynamoDB API,...
    
+ Refactoring: các scenario migrate to AWS
+ Monitoring & Troubleshooting
    - X-Ray monitoring serverless services
    - CloudWatch vs CloudTrail vs Config
    
