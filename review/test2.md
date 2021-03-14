*A business has their test environment built on Amazon EC2 configured on General purpose SSD volume.
At which gp2 volume size will their test environment hit the max IOPS?*

- **GP2 (SSD**): General purpose SSD volume that balances price and performance for a
wide variety of workloads
  - 1 GiB - 16 TiB 
  - • Small gp2 volumes can burst IOPS to 3000 
  - • Max IOPS is **16,000**
  - • **3 IOPS per GB**, means at 5,334GB we are at the max IOPS

- **IO1 (SSD)**: Highest-performance SSD volume for mission-critical low-latency or high- throughput workloads
  - Critical business applications that require sustained IOPS performance, or
more than 16,000 IOPS per volume (gp2 limit)
  - Large database workloads, such as: MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
  - 4 GiB - 16 TiB
  - IOPS is provisioned (PIOPS) – MIN 100 - MAX 64,000 (Nitro instances) else MAX 32,000 (other instances)
  - The maximum ratio of provisioned IOPS to requested volume size (in GiB) is **50:1** for io1 volumes, and 500:1 for io2 volumes

- **ST1 (HDD)**: Low cost HDD volume designed for frequently accessed, throughput- intensive workloads
- **SC1 (HDD)**: Lowest cost HDD volume designed for less frequently accessed workloads
- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)
- When in doubt always consult the AWS documentation – it’s good!
- Only GP2 and IO1 can be used as boot volumes

**ASG Launch configuration**

By default, basic monitoring is enabled when you create a launch template or when you use the AWS Management Console to create a launch configuration. **Detailed monitoring is enabled by default when you create a launch configuration using the AWS CLI or an SDK.**

EC2 Auto Scaling groups are regional constructs. They can span Availability Zones, but not AWS regions.

**AWS Management Console might have been used to create the launch configuration**. This could be the reason behind only the basic monitoring taking place.


- AWS **CloudWatch**:
  - *Metrics*: Collect and track key metrics
    - With `detailed monitoring`, you get data `every 1 minute` (default is `5 minutes`)
  - *Logs*: Collect, monitor, analyze and store log files, **They never expire by default**
  - *Events*: Send notifications when certain events happen in your AWS
  - ***Alarms***: React in real-time to metrics / events
- AWS **X-Ray**:
  - Troubleshooting *application performance and errors*
  - Distributed tracing of microservices
- AWS **CloudTrail**:
  - Internal monitoring of *API calls* being made
  - Audit changes to AWS Resources by your users, history of events / API calls made within your AWS Account
  *- If a resource is deleted in AWS, look into CloudTrail first!*

\* *CloudWatch Custom **METRICS***

- Metric resolution (StorageResolution API parameter – two possible value):
  - **Standard**: 1 minute (`60s`)
  - **High Resolution**: `1s` – Higher cost
- Use API call `PutMetricData`
- Use exponential back off in case of throttle errors

\* AWS CloudWatch **ALARM**

- Alarm states:OK, INSUFFICIENT_DATA, ALARM
- ***High resolution** custom metrics*: 10sec or 30sec

--------

CloudFormation currently supports the following parameter types:

```
String – A literal string
Number – An integer or float
List<Number> – An array of integers or floats
CommaDelimitedList – An array of literal strings that are separated by commas
AWS::EC2::KeyPair::KeyName – An Amazon EC2 key pair name
AWS::EC2::SecurityGroup::Id – A security group ID
AWS::EC2::Subnet::Id – A subnet ID
AWS::EC2::VPC::Id – A VPC ID
List<AWS::EC2::VPC::Id> – An array of VPC IDs
List<AWS::EC2::SecurityGroup::Id> – An array of security group IDs
List<AWS::EC2::Subnet::Id> – An array of subnet IDs
```

*However, the deployment part to Elastic Beanstalk is taking a very long time due to resolving dependencies on all of your 100 target EC2 instances.
Which of the following actions should you take to improve performance with limited code changes?*

**"Bundle the dependencies in the source code during the last stage of CodeBuild"**

AWS CodeBuild is a fully managed build service. There are no servers to provision and scale, or software to install, configure, and operate.

A typical application build process includes phases like preparing the environment, updating the configuration, downloading dependencies, running unit tests, and finally, packaging the built artifact.

Downloading dependencies is a critical phase in the build process. These dependent files can range in size from a few KBs to multiple MBs. Because most of the dependent files do not change frequently between builds, you can noticeably reduce your build time by caching dependencies.

This will allow the code bundle to be deployed to Elastic Beanstalk to have both the dependencies and the code, hence speeding up the deployment time to Elastic Beanstalk

**"Bundle the dependencies in the source code in CodeCommit"** - This is not best practice and could make the CodeCommit repository huge.

**"Store the dependencies in S3"** - S3 can be used as a storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk however there would still be a time lag to retrieve the files.

**"Create a custom platform for Elastic Beanstalk"** - This is a more advanced feature that requires code changes, so does not fit the use-case.

*Which of the following AWS database engines can be configured with IAM Database Authentication? (Select two)*

You can authenticate to your DB instance using A**WS Identity and Access Management (IAM)** database authentication. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token. An authentication token is a unique string of characters that Amazon RDS generates on request. Each token has a lifetime of 15 minutes. You don't need to store user credentials in the database, because authentication is managed externally using IAM.

**RDS MySQL - IAM database authentication works with MySQL and PostgreSQL.
RDS PostGreSQL - IAM database authentication works with MySQL and PostgreSQL.**

Exam tip:

- IAM-based authentication (*IAM database authentication*) can be used to login into RDS MySQL & PostgreSQL
- **RDS Encryption**
  - If the master is not encrypted, the read replica cannot be encrypted.
  - Transparent Data Encryption (TDE) only available for **Oracle & SQL Server.**

- **RDS Authentication**
• IAM database authentication works with **MySQL and PostgreSQL**
• You don’t need a password, just an authentication token obtained through IAM & RDS API calls
• Auth token has a lifetime of 15 minutes

*A diagnostic lab stores its data on DynamoDB. The lab wants to backup a particular DynamoDB table data on Amazon S3, so it can download the S3 backup locally for some operational use.
Which of the following options is not feasible?*

## Dynamo Backup
**How can I back up a DynamoDB table to Amazon S3?**

DynamoDB offers two built-in backup methods:

- **On-demand:** Create backups when you choose.
- **Point-in-time recovery:** Enable automatic, continuous backups.

Both of these methods use Amazon S3. However, *you don't have access to the S3 buckets* that are used for these backups. T**he DynamoDB Export to S3 feature is the easiest way to create backups** that you can download locally or use in another AWS service. If you need more customization, use AWS Data Pipeline, Amazon EMR, or AWS Glue instead.

**Use the DynamoDB on-demand backup capability to write to Amazon S3 and download locally** - This option is not feasible for the given use-case. DynamoDB has two built-in backup methods (On-demand, Point-in-time recovery) that write to Amazon S3, but you will not have access to the S3 buckets that are used for these backups.

**Use AWS Data Pipeline to export your table to an S3 bucket in the account of your choice and download locally** - This is the easiest method. This method is used when you want to make a one-time backup using the lowest amount of AWS resources possible. Data Pipeline uses Amazon EMR to create the backup, and the scripting is done for you. You don't have to learn Apache Hive or Apache Spark to accomplish this task.

**Use Hive with Amazon EMR to export your data to an S3 bucket and download locally** - Use Hive to export data to an S3 bucket. Or, use the open-source emr-dynamodb-connector to manage your own custom backup method in Spark or Hive. These methods are the best practice to use if you're an active Amazon EMR user and are comfortable with Hive or Spark. These methods offer more control than the Data Pipeline method.

**Use AWS Glue to copy your table to Amazon S3 and download locally** - Use AWS Glue to copy your table to Amazon S3. This is the best practice to use if you want automated, continuous backups that you can also use in another service, such as Amazon Athena.

*As a Developer Associate, which of the following reserved instance types you would select to provide **capacity reservations**?*

When you purchase a Reserved Instance for a specific Availability Zone, it's referred to as a Zonal Reserved Instance. Zonal Reserved Instances provide capacity reservations as well as discounts.

**Zonal Reserved Instances** - A zonal Reserved Instance provides a capacity reservation in the specified Availability Zone. Capacity Reservations enable you to reserve capacity for your Amazon EC2 instances in a specific Availability Zone for any duration. This gives you the ability to create and manage Capacity Reservations independently from the billing discounts offered by Savings Plans or regional Reserved Instances.

**Regional**: When you purchase a Reserved Instance for a Region, it's referred to as a regional Reserved Instance.

**Zonal**: When you purchase a Reserved Instance for a specific Availability Zone, it's referred to as a zonal Reserved Instance.

**Regional Reserved Instances** - When you purchase a Reserved Instance for a Region, it's referred to as a regional Reserved Instance. A regional Reserved Instance does not provide a capacity reservation.

![dsdf](./images/reserved-instance.jpg)

*Which of the following actions would make sure that only the last updated value of any item is used in the application?*

Use ConsistentRead = true while doing GetItem operation for any item

DynamoDB supports eventually consistent and strongly consistent reads.

**Eventually Consistent Reads**

When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

**Strongly Consistent Reads**

When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful.

DynamoDB uses eventually consistent reads by default. Read operations (such as GetItem, Query, and Scan) provide a ConsistentRead parameter. If you set this parameter to true, DynamoDB uses strongly consistent reads during the operation. As per the given use-case, to make sure that only the last updated value of any item is used in the application, you should use strongly consistent reads by setting ConsistentRead = true for GetItem operation.

*ASG: One of the 10 EC2 instances has been reported as unhealthy.
Which of the following actions will take place?*

**The ASG will terminate the EC2 Instance**
To maintain the same number of instances, Amazon EC2 Auto Scaling performs a periodic health check on running instances within an Auto Scaling group. When it finds that an instance is unhealthy, it terminates that instance and launches a new one. Amazon EC2 Auto Scaling creates a new scaling activity for terminating the unhealthy instance and then terminates it. Later, another scaling activity launches a new instance to replace the terminated instance.

**The ASG will detach the EC2 instance from the group, and leave it running** - The goal of the auto-scaling group is to get rid of the bad instance and replace it

**The ASG will keep the instance running and re-start the application** - The ASG does not have control of your application

**The ASG will format the root EBS drive on the EC2 instance and run the User Data again** - This will not happen, the ASG cannot assume the format of your EBS drive, and User Data only runs once at instance first boot.

**Use Envelope Encryption and reference the data as file within the code**

While AWS KMS does support sending data up to 4 KB to be encrypted directly, envelope encryption can offer significant performance benefits. When you encrypt data directly with AWS KMS it must be transferred over the network. Envelope encryption reduces the network load since only the request and delivery of the much smaller data key go over the network. The data key is used locally in your application or encrypting AWS service, avoiding the need to send the entire block of data to AWS KMS and suffer network latency.

AWS Lambda environment variables can have a maximum size of 4 KB. Additionally, the direct 'Encrypt' API of KMS also has an upper limit of 4 KB for the data payload. To encrypt 1 MB, you need to use the Encryption SDK and pack the encrypted file with the lambda function.

*they have decided to re-use their SSH key pairs for the different instances across AWS Regions.*

**Generate a public SSH key from a private SSH key. Then, import the key into each of your AWS Regions**

Here is the correct way of reusing SSH keys in your AWS Regions:
1. Generate a public SSH key (.pub) file from the private SSH key (.pem) file.
2. Set the AWS Region you wish to import to.
3. Import the public SSH key into the new Region.

**Encrypt the private SSH key and store it in the S3 bucket to be accessed from any AWS Region** - Storing private key to Amazon S3 is possible. But, this will not make the key accessible for all AWS Regions, as is the need in the current use case.

Amazon Kinesis Data Firehose is a fully managed service for delivering real-time streaming data to destinations such as Amazon Simple Storage Service **(Amazon S3), Amazon Redshift, Amazon Elasticsearch Service (Amazon ES), and Splunk**. With Kinesis Data Firehose, you don't need to write applications or manage resources. You configure your data producers to send data to Kinesis Data Firehose, and it automatically delivers the data to the destination that you specified.

**Amazon ElastiCache with Amazon S3 as backup** - Amazon ElastiCache is a fully managed in-memory data store, compatible with Redis or Memcached. ElastiCache is NOT a supported destination for Amazon Kinesis Data Firehose.

Continuous integration is a DevOps software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run.

Continuous delivery is a software development practice where code changes are automatically prepared for a release to production. A pillar of modern application development, continuous delivery expands upon continuous integration by deploying all code changes to a testing environment and/or a production environment after the build stage.

*A developer needs to automate software package deployment to both Amazon EC2 instances and virtual servers running on-premises, as part of continuous integration and delivery that the business has adopted.*

**AWS CodeDeploy** - AWS CodeDeploy is a fully managed "deployment" service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers. AWS CodeDeploy makes it easier for you to rapidly release new features, helps you avoid downtime during application deployment, and handles the complexity of updating your applications. This is the right choice for the current use case.

**AWS Elastic Beanstalk** - AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS. You can simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. At the same time, you retain full control over the AWS resources powering your application and can access the underlying resources at any time.

**Beanstalk** - With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications. Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

*A new recruit is trying to configure what an Amazon EC2 should do when it interrupts a Spot Instance.*

A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Your Spot Instance runs whenever capacity is available and the maximum price per hour for your request exceeds the Spot price. Any instance present with unused capacity will be allocated.

You can specify that Amazon EC2 should do one of the following when it interrupts a Spot Instance:
Stop the Spot Instance
Hibernate the Spot Instance
Terminate the Spot Instance
The **default** is to terminate Spot Instances when they are interrupted.

Reboot the Spot Instance - This is an invalid option.

**One instance is charged at one hour of Reserved Instance usage and the other two instances are charged at two hours of On-Demand usage**
A Reserved Instance billing benefit can apply to a maximum of 3600 seconds (one hour) of instance usage per clock-hour. You can run multiple instances concurrently, but can only receive the benefit of the Reserved Instance discount for a total of 3600 seconds per clock-hour; instance usage that exceeds 3600 seconds in a clock-hour is billed at the On-Demand rate.

`ECS_ENABLE_TASK_IAM_ROLE`

This configuration item is used to enable IAM roles for tasks for containers with the bridge and default network modes.

`ECS_ENGINE_AUTH_DATA` - This refers to the authentication data within a Docker configuration file, so this is not the correct option.

`ECS_AVAILABLE_LOGGING_DRIVERS` - The Amazon ECS container agent running on a container instance must register the logging drivers available on that instance with this variable. This configuration item refers to the logging driver.

`ECS_CLUSTER` - This refers to the ECS cluster that the ECS agent should check into. This is passed to the container instance at launch through Amazon EC2 user data.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow for use of this Key",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:role/UserRole"
            },
            "Action": [
                "kms:GenerateDataKeyWithoutPlaintext",
                "kms:Decrypt"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Allow for EC2 Use",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:role/UserRole"
            },
            "Action": [
                "kms:CreateGrant",
                "kms:ListGrants",
                "kms:RevokeGrant"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                "kms:ViaService": "ec2.us-west-2.amazonaws.com"
            }
        }
    ]
}
```

**The first statement provides a specified IAM principal the ability to generate a data key and decrypt that data key from the CMK when necessary** - To create and use an encrypted Amazon Elastic Block Store (EBS) volume, you need permissions to use Amazon EBS. The key policy associated with the CMK would need to include these. The above policy is an example of one such policy.

In this CMK policy, the first statement provides a specified IAM principal the ability to generate a data key and decrypt that data key from the CMK when necessary. These two APIs are necessary to encrypt the EBS volume while it’s attached to an Amazon Elastic Compute Cloud (EC2) instance.

**The second statement in this policy provides the specified IAM principal the ability to create, list, and revoke grants for Amazon EC2**. Grants are used to delegate a subset of permissions to AWS services, or other principals, so that they can use your keys on your behalf. In this case, the condition policy explicitly ensures that only Amazon EC2 can use the grants. Amazon EC2 will use them to re-attach an encrypted EBS volume back to an instance if the volume gets detached due to a planned or unplanned outage. These events will be recorded within AWS CloudTrail when, and if, they do occur for your auditing.

**no limit**: There are no message limits for storing in SQS, but 'in-flight messages' do have limits. Make sure to delete messages after you have processed them. There can be a maximum of approximately 120,000 inflight messages (received from a queue by a consumer, but not yet deleted from the queue).

**EBS volumes are AZ locked**
EFS is global

An Amazon EBS volume is a durable, block-level storage device that you can attach to your instances. After you attach a volume to an instance, you can use it as you would use a physical hard drive. EBS volumes are flexible. For current-generation volumes attached to current-generation instance types, you can dynamically increase size, modify the provisioned IOPS capacity, and change volume type on live production volumes.

When you create an EBS volume, it is automatically replicated within its Availability Zone to prevent data loss due to the failure of any single hardware component. You can attach an EBS volume to an EC2 instance in the same Availability Zone.

Create an IAM role with S3 access in Account B and set Account A as a trusted entity. Create another role (instance profile) in Account A and attach it to the EC2 instances in Account A and add an inline policy to this role to assume the role from Account B

You can give EC2 instances in one account ("account A") permissions to assume a role from another account ("account B") to access resources such as S3 buckets. You need to create an IAM role in Account B and set Account A as a trusted entity. Then attach a policy to this IAM role such that it delegates access to Amazon S3 like so -
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::awsexamplebucket1",
                "arn:aws:s3:::awsexamplebucket1/*",
                "arn:aws:s3:::awsexamplebucket2",
                "arn:aws:s3:::awsexamplebucket2/*"
            ]
        }
    ]
}
```
Then you can create another role (instance profile) in Account A and attach it to the EC2 instances in Account A and add an inline policy to this role to assume the role from Account B like so -
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::AccountB_ID:role/ROLENAME"
        }
    ]
}
```

**Cognito Sync**
Amazon Cognito Sync is an AWS service and client library that enables cross-device syncing of application-related user data. You can use it to synchronize user profile data across mobile devices and the web without requiring your own backend. The client libraries cache data locally so your app can read and write data regardless of device connectivity status. When the device is online, you can synchronize data, and if you set up push sync, notify other devices immediately that an update is available.

*per the new requirements, the deployment process should be able to change permissions for deployed files as well as verify the deployment success*

**Define an appspec.yml file in the root directory:** An AppSpec file must be a YAML-formatted file named appspec.yml and it must be placed in the root of the directory structure of an application's source code.

The AppSpec file is used to

- Map the source files in your application revision to their destinations on the instance.
- Specify custom permissions for deployed files.
- Specify scripts to be run on each instance at various stages of the deployment process.

During deployment, the CodeDeploy agent looks up the name of the current event in the hooks section of the AppSpec file. If the event is not found, the CodeDeploy agent moves on to the next step. If the event is found, the CodeDeploy agent retrieves the list of scripts to execute. The scripts are run sequentially, in the order in which they appear in the file. The status of each script is logged in the CodeDeploy agent log file on the instance.

If a script runs successfully, it returns an exit code of 0 (zero). If the CodeDeploy agent installed on the operating system doesn't match what's listed in the AppSpec file, the deployment fails.

**Define a buildspec.yml file in the root directory** - This is a file used by AWS CodeBuild to run a build. This is not relevant to the given use case.

**Configure Application Auto Scaling to manage Lambda provisioned concurrency on a schedule**

Concurrency is the number of requests that a Lambda function is serving at any given time. If a Lambda function is invoked again while a request is still being processed, another instance is allocated, which increases the function's concurrency.

Due to a spike in traffic, when Lambda functions scale, this causes the portion of requests that are served by new instances to have higher latency than the rest. To enable your function to scale without fluctuations in latency, use provisioned concurrency. By allocating provisioned concurrency before an increase in invocations, you can ensure that all requests are served by initialized instances with very low latency.

You can configure Application Auto Scaling to manage provisioned concurrency on a schedule or based on utilization. Use scheduled scaling to increase provisioned concurrency in anticipation of peak traffic. To increase provisioned concurrency automatically as needed, use the Application Auto Scaling API to register a target and create a scaling policy.

**Amazon recommends that you use multipart uploading for the following use-cases:**

If you're uploading large objects over a stable high-bandwidth network, use multipart uploading to maximize the use of your available bandwidth by uploading object parts in parallel for multi-threaded performance.

If you're uploading over a spotty network, use multipart uploading to increase resiliency to network errors by avoiding upload restarts.

You need to use multi-part upload for large files: In general, when your object size reaches 100 MB, you should consider using multipart uploads instead of uploading the object in a single operation.