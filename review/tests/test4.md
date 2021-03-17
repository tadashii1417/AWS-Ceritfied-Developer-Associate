**Repositories are automatically encrypted at rest**

Data in AWS CodeCommit repositories is encrypted in transit and at rest. When data is pushed into an AWS CodeCommit repository (for example, by calling git push), AWS CodeCommit encrypts the received data as it is stored in the repository.

*A data analytics company with its IT infrastructure on the AWS Cloud wants to build and deploy its flagship application as soon as there are any changes to the source code.*

**Keep the source code in an AWS CodeCommit repository and start AWS CodePipeline whenever a change is pushed to the CodeCommit repository**
**Keep the source code in an Amazon S3 bucket and start AWS CodePipeline whenever a file in the S3 bucket is updated**

AWS CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates. CodePipeline automates the build, test, and deploy phases of your release process every time there is a code change, based on the release model you define.

**Use parallel scans**
By default, the Scan operation processes data sequentially. Amazon DynamoDB returns data to the application in 1 MB increments, and an application performs additional Scan operations to retrieve the next 1 MB of data. The larger the table or index being scanned, the more time the Scan takes to complete. To address these issues, the Scan operation can logically divide a table or secondary index into multiple segments, with multiple application workers scanning the segments in parallel.

To make use of a parallel Scan feature, you will need to run multiple worker threads or processes in parallel. Each worker will be able to scan a separate partition of a table concurrently with the other workers.

**Amazon SQS supports dead-letter queues**, which other queues (source queues) can target for messages that can't be processed (consumed) successfully. Dead-letter queues are useful for debugging your application or messaging system because they let you isolate problematic messages to determine why their processing doesn't succeed. 

The redrive policy specifies the source queue, the dead-letter queue, and the conditions under which Amazon SQS moves messages from the former to the latter if the consumer of the source queue fails to process a message a specified number of times. When the ReceiveCount for a message exceeds the maxReceiveCount for a queue, Amazon SQS moves the message to a dead-letter queue (with its original message ID). For example, if the source queue has a redrive policy with maxReceiveCount set to 5, and the consumer of the source queue receives a message 6 times without ever deleting it, Amazon SQS moves the message to the dead-letter queue.

*The dead-letter queue of a FIFO queue must also be a FIFO queue. Similarly, the dead-letter queue of a standard queue must also be a standard queue.*

*When your client applications first load they capture the load balancer DNS name and then resolve the IP address for the load balancer so that they can directly reference the underlying IP.
It is observed that the client applications work well but unexpectedly stop working after a while.*

**The load balancer is highly available and its public IP may change. The DNS name is constant**
When your load balancer is created, it receives a public DNS name that clients can use to send requests. The DNS servers resolve the DNS name of your load balancer to the public IP addresses of the load balancer nodes for your load balancer. Never resolve the IP of a load balancer as it can change with time. You should always use the DNS name.

**Use Enhanced Fanout feature of Kinesis Data Streams**
Amazon Kinesis Data Streams (KDS) is a massively scalable and durable real-time data streaming service. KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events.

By default, the 2MB/second/shard output is shared between all of the applications consuming data from the stream. You should use enhanced fan-out if you have multiple consumers retrieving data from a stream in parallel. With enhanced fan-out developers can register stream consumers to use enhanced fan-out and receive their own 2MB/second pipe of read throughput per shard, and this throughput automatically scales with the number of shards in a stream.

**"Use a cron job on the instances that pushes the EC2 RAM statistics as a Custom metric into CloudWatch"**

The Amazon CloudWatch Monitoring Scripts for Amazon Elastic Compute Cloud (Amazon EC2) Linux-based instances demonstrate how to produce and consume Amazon CloudWatch custom metrics. These Perl scripts comprise a fully functional example that reports memory, swap, and disk space utilization metrics for a Linux instance. You can set a cron schedule for metrics reported to CloudWatch and report memory utilization to CloudWatch every x minutes.

**"Extract RAM statistics from the standard CloudWatch metrics for EC2 instances"** - Amazon EC2 sends metrics to Amazon CloudWatch. By default, each data point covers the 5 minutes that follow the start time of activity for the instance. If you've enabled detailed monitoring, each data point covers the next minute of activity from the start time. The standard CloudWatch metrics don't have any metrics for memory utilization details.

**Store the secret as SecureString in SSM Parameter Store**
With AWS Systems Manager Parameter Store, you can create SecureString parameters, which are parameters that have a plaintext parameter name and an encrypted parameter value. Parameter Store uses AWS KMS to encrypt and decrypt the parameter values of Secure String parameters. Also, if you are using customer-managed CMKs, you can use IAM policies and key policies to manage to encrypt and decrypt permissions. To retrieve the decrypted value you only need to do one API call.

**Encrypt first with KMS then store in SSM Parameter store** - This could work but will require two API calls to get the decrypted value instead of one. So this is not the right option.

*You do not want to store AWS secret and access keys onto the mobile devices and need all the calls to DynamoDB made with a different identity per mobile device.*

**"Cognito Identity Pools"**

Amazon Cognito identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and received a token. Identity pools provide AWS credentials to grant your users access to other AWS services.
**"IAM"** - This is not a good solution because it would require you to have an IAM user for each mobile device which is not a good practice or manageable way of handling deployment.

**Use client-side encryption with an AWS KMS managed customer master key (CMK)**

Server-Side Encryption – Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
Client-Side Encryption – Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools.
As the company wants to make sure that the files are encrypted before they are sent to Amazon S3, therefore you should use client-side encryption.
To enable client-side encryption, you have the following options:
Use a customer master key (CMK) stored in AWS Key Management Service (AWS KMS).
Use a master key you store within your application.
As the use-case mentions that the encryption keys need to be managed by an in-house security team but the key itself should be stored on AWS, therefore you must use the customer master key (CMK) stored in AWS Key Management Service (AWS KMS).

**You have AWS Route 53 policies to automatically distribute weighted traffic to the API resources located at URL api.global.com. What is an alternative way of distributing traffic to a web application?**

**ELB**
Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. Route 53 failover policy is similar to an ELB in that when using failover routing, it lets you route traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy.
CloudFront - Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds.