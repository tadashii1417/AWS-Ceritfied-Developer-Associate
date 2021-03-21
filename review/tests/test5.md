- AWS **CloudWatch**
  - ***Metrics***: Collect and track key metrics
    - With `detailed monitoring`, you get data `every 1 minute` (default is `5 minutes`)
  - ***Logs***: Collect, monitor, analyze and store log files, **They never expire by default**
  - ***Events***: Send notifications when certain events happen in your AWS
  - ***Alarms***: React in real-time to metrics / events

- **CloudWatch Event Rules**:
  - Trigger for pull request updates (created / updated / deleted / commented)
  - Commit comment events
  - CloudWatch Event Rules goes into an SNS topic committed in the code?)
![sadfsdf](../images/cw-note.jpeg)

**You can use Amazon CloudWatch Events to detect and react to changes in the state of a pipeline, stage, or action**. Then, based on rules you create, CloudWatch Events invokes one or more target actions when a pipeline, stage, or action enters the state you specify in a rule. For the given use-case, you can set up a rule that detects pipeline changes and invokes an AWS Lambda function.

- CodePipeline state changes happen in ***AWS CloudWatch Events***, which can in return create SNS notifications.
  - Ex: you can create events for failed pipelines
  - Ex: you can create events for cancelled stages 

**Use the Lambda console to configure a trigger that invokes the Lambda function with CodePipeline as the event source** - You cannot create a trigger with CodePipeline as the event source via the Lambda Console.
**Use the CodePipeline console to set up a trigger for the Lambda function** - CodePipeline console cannot be used to configure a trigger for a Lambda function.

*like to host a static HTML5 website on the cloud and be able to access it via domain www.mycompany.com. You have created a bucket in (S3), enabled website hosting, and set the index.html as the default page. When you test the domain www.mycompany.com you get the following error: 'HTTP response code 403 (Access Denied)'. What can you do to resolve this error?*
**Create a bucket policy**
Bucket policy is an access policy option available for you to grant permission to your Amazon S3 resources. It uses JSON-based access policy language.
If you want to configure an existing bucket as a static website that has public access, you must edit block public access settings for that bucket. You may also have to edit your account-level block public access settings.
**Create an IAM role** - This will not help because IAM roles are attached to services and in this case, we have public users.
**Enable CORS** - CORS defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. Here we are not dealing with cross domains.

**Enable DynamoDB Accelerator (DAX) for DynamoDB and CloudFront for S3**
DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for Amazon DynamoDB that delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.
DAX is tightly integrated with DynamoDB—you simply provision a DAX cluster, use the DAX client SDK to point your existing DynamoDB API calls at the DAX cluster, and let DAX handle the rest. Because DAX is API-compatible with DynamoDB, you don't have to make any functional application code changes. DAX is used to natively cache DynamoDB reads.
CloudFront is a content delivery network (CDN) service that delivers static and dynamic web content, video streams, and APIs around the world, securely and at scale. By design, delivering data out of CloudFront can be more cost-effective than delivering it from S3 directly to your users.
When a user requests content that you serve with CloudFront, their request is routed to a nearby Edge Location. If CloudFront has a cached copy of the requested file, CloudFront delivers it to the user, providing a fast (low-latency) response. If the file they’ve requested isn’t yet cached, CloudFront retrieves it from your origin – for example, the S3 bucket where you’ve stored your content.
So, you can use CloudFront to improve application performance to serve static content from S3.

**Enable ElastiCache Redis for DynamoDB and CloudFront for S3**
Amazon ElastiCache for Redis is a blazing fast in-memory data store that provides sub-millisecond latency to power internet-scale real-time applications. Amazon ElastiCache for Redis is a great choice for real-time transactional and analytical processing use cases such as caching, chat/messaging, gaming leaderboards, geospatial, machine learning, media streaming, queues, real-time analytics, and session store.
*Although, you can integrate Redis with DynamoDB, it's much more involved from a development perspective. For the given use-case, you should use DAX which is a much better fit.*

**Enable DAX for DynamoDB and ElastiCache Memcached for S3**
**Enable ElastiCache Redis for DynamoDB and ElastiCache Memcached for S3**
Amazon ElastiCache for Memcached is a Memcached-compatible in-memory key-value store service that can be used as a cache or a data store. Amazon ElastiCache for Memcached is a great choice for implementing an in-memory cache to decrease access latency, increase throughput, and ease the load off your relational or NoSQL database.
ElastiCache cannot be used as a cache to serve static content from S3, so both these options are incorrect.

*As such, you would like to have a unified account to view all the traces. What should you in your X-Ray daemon set up to make this work?*

AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components.
AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components.

**Create a role in the target unified account and allow roles in each sub-account to assume the role**
**Configure the X-Ray daemon to use an IAM instance role**

The X-Ray agent can assume a role to publish data into an account different from the one in which it is running. This enables you to publish data from various components of your application into a central account.
X-Ray can also track requests flowing through applications or services across multiple AWS Regions.
**Create a user in the target unified account and generate access and secret keys**
**Configure the X-Ray daemon to use access and secret keys**

These two options combined together would work but wouldn't be a best-practice security-wise. Therefore these are not correct.
Enable Cross Account collection in the X-Ray console - This is a made-up option and has been added as a distractor.

*The application will act as a backend and will perform (CRUD) operations such as create, read, update and delete as well as **inner joins.***
**Amazon Relational Database Service (Amazon RDS**) makes it easy to set up, operate, and scale a relational database with support for transactions in the cloud. A relational database is a collection of data items with pre-defined relationships between them. RDS supports the most demanding database applications. You can choose between two SSD-backed storage options: one optimized for high-performance Online Transaction Processing (OLTP) applications, and the other for cost-effective general-purpose use.
DynamoDB - RDS can run expensive joins which DynamoDB does not support. DynamoDB is a better choice for scaling by storing complex hierarchical data within a single item.

*You would like to run the X-Ray daemon for your Docker containers deployed using AWS **Fargate**.*
**Deploy the X-Ray daemon agent as a sidecar container**
In Amazon ECS, create a Docker image that runs the X-Ray daemon, upload it to a Docker image repository, and then deploy it to your Amazon ECS cluster. You can use port mappings and network mode settings in your task definition file to allow your application to communicate with the daemon container.
As we are using AWS Fargate, we do not have control over the underlying EC2 instance and thus we can't deploy the agent on the EC2 instance or run an X-Ray agent container as a daemon set (only available for ECS classic).

![sdf](../images/x-ray-2.jpeg)

**Provide the correct IAM task role to the X-Ray container**
For Fargate, we can only provide IAM roles to tasks, which is also the best security practice should we use EC2 instances.

*Incorrect options:*
Deploy the X-Ray daemon agent as a daemon set on ECS - As explained above, since we are using AWS Fargate, we do not have control over the underlying EC2 instance and thus we can't run an X-Ray agent container as a daemon set.
Deploy the X-Ray daemon agent as a process on your EC2 instance
Provide the correct IAM instance role to the EC2 instance

**As we are using AWS Fargate, we do not have control over the underlying EC2 instance, so both these options are incorrect.**

*You would like your Elastic Beanstalk environment to expose an HTTPS endpoint and an HTTP endpoint. The HTTPS endpoint should be used to get in-flight encryption between your clients and your web servers, while the HTTP endpoint should only be used to redirect traffic to HTTPS and support URLs starting with http://.*

**Assign an SSL certificate to the Load Balancer**
This ensures that the Load Balancer can expose an HTTPS endpoint.
**Open up port 80 & port 443**
This ensures that the Load Balancer will allow both the HTTP (80) and HTTPS (443) protocol for incoming connections
**Configure your EC2 instances to redirect HTTP traffic to HTTPS**
This ensures traffic originating from HTTP onto the Load Balancer forces a redirect to HTTPS by the EC2 instances before being correctly served, thus ensuring the traffic served is fully encrypted.

*Your company has developers worldwide with access to the company's Amazon Simple Storage Service (S3) buckets. The objects in the buckets are encrypted at the server-side but need more flexibility with access control, auditing, rotation, and deletion of keys. You would also like to limit who can use the key.*
**SSE-KMS**
You have the following options for protecting data at rest in Amazon S3:
Server-Side Encryption – Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
Client-Side Encryption – Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools.
Server-Side Encryption with Customer Master Keys (CMKs) stored in AWS Key Management Service (SSE-KMS) is similar to SSE-S3. SSE-KMS provides you with an audit trail that shows when your CMK was used and by whom. Additionally, you can create and manage customer-managed CMKs or use AWS managed CMKs that are unique to you, your service, and your Region.

![asdf](../images/encruption.jpeg)

**SSE-C** When retrieving objects encrypted server-side with SSE-C, you must provide the same encryption key as part of your request. Amazon S3 first verifies that the encryption key you provided matches, and then decrypts the object before returning the object data to you
**SSE-S3** - When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates. So this option is incorrect.

*You are running a public DNS service on an EC2 instance where the DNS name is pointing to the IP address of the instance. You wish to upgrade your DNS service but would like to do it without any downtime.*
**Route 53** is a DNS managed by AWS, but nothing prevents you from running your own DNS (it's just a software) on an EC2 instance. The trick of this question is that it's about EC2, running some software that needs a fixed IP, and not about Route 53 at all.
**Elastic IP**
DNS services are identified by a public IP, so you need to use Elastic IP.
**Provide a static private IP** - If you provide a private IP it will not be accessible from the internet, so this option is incorrect.
**Use Route 53** - Route 53 is a DNS service from AWS but the use-case talks about offering a DNS service using an EC2 instance, so this option is incorrect.

*AWS SQS FIFO queues to get the ordering of messages on a per user_id basis. On top of this, you would like to make sure that duplicate messages should not be sent to SQS as this would cause application failure.*

**The message deduplication ID** is the token used for the deduplication of sent messages. If a message with a particular message deduplication ID is sent successfully, any messages sent with the same message deduplication ID are accepted successfully but aren't delivered during the 5-minute deduplication interval.
**MessageGroupId** - The message group ID is the tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are always processed one by one, in a strict order relative to the message group (however, messages that belong to different message groups might be processed out of order).
**ReceiveRequestAttemptId** - This parameter applies only to FIFO (first-in-first-out) queues. The token is used for deduplication of ReceiveMessage calls. If a networking issue occurs after a ReceiveMessage action, and instead of a response you receive a generic error, you can retry the same action with an identical ReceiveRequestAttemptId to retrieve the same set of messages, even if their visibility timeout has not yet expired.

*Mobile app users will register and be given permissions to access AWS resources. Which technology would you recommend for subscribing users to messages?*
**SNS**
Amazon SNS enables message filtering and fanout to a large number of subscribers, including serverless functions, queues, and distributed systems. Additionally, Amazon SNS fans out notifications to end users via mobile push messages, SMS, and email.
Amazon SNS follows the 'publish-subscribe' (pub-sub) messaging paradigm, with notifications being delivered to clients using a 'push' mechanism that eliminates the need to periodically check or 'poll' for new information and updates.
**SQS** - SQS is a distributed queuing system. Messages are not pushed to receivers. Receivers have to poll SQS to receive messages

**Your application needs to renew the credentials after 1 hour when they expire**
AWS Security Token Service (AWS STS) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users). By default, AWS Security Token Service (STS) is available as a global service, and all AWS STS requests go to a single endpoint at https://sts.amazonaws.com.
Credentials that are created by using account credentials can range from 900 seconds (15 minutes) up to a maximum of 3,600 seconds (1 hour), with a default of 1 hour. Hence you need to renew the credentials post expiry.

[X] **Versioning can be enabled only for a specific folder**

The versioning state applies to all (never some) of the objects in that bucket. The first time you enable a bucket for versioning, objects in it are thereafter always versioned and given a unique version ID.

**Deleting a file is a recoverable operation** - Correct, when you delete an object (file), Amazon S3 inserts a delete marker, which becomes the current object version and you can restore the previous version.

*Which environment variable can be used by AWS X-Ray SDK to ensure that the daemon is correctly discovered on ECS?*
**AWS_XRAY_DAEMON_ADDRESS**
Set the host and port of the X-Ray daemon listener. By default, the SDK uses 127.0.0.1:2000 for both trace data (UDP) and sampling (TCP). Use this variable if you have configured the daemon to listen on a different port or if it is running on a different host.
**AWS_XRAY_TRACING_NAME** - This sets a service name that the SDK uses for segments.
**AWS_XRAY_CONTEXT_MISSING** - This should be set to LOG_ERROR to avoid throwing exceptions when your instrumented code attempts to record data when no segment is open.
**AWS_XRAY_DEBUG_MODE** - This should be set to TRUE to configure the SDK to output logs to the console, instead of configuring a logger.

**Docker multi-container platform**
Docker is a container platform that allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Multicontainer Docker platform if you need to run multiple containers on each instance. The Multicontainer Docker platform does not include a proxy server. Elastic Beanstalk uses Amazon Elastic Container Service (Amazon ECS) to coordinate container deployments to multi-container Docker environments.
**Docker single-container platform** - Docker is a container platform that allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Single Container Docker platform if you only need to run a single Docker container on each instance in your environment. The single container platform includes an Nginx proxy server.
**Custom Platform** - Elastic Beanstalk supports custom platforms. A custom platform provides more advanced customization than a custom image in several ways. A custom platform lets you develop an entirely new platform from scratch, customizing the operating system, additional software, and scripts that Elastic Beanstalk runs on platform instances. This flexibility enables you to build a platform for an application that uses a language or other infrastructure software, for which Elastic Beanstalk doesn't provide a managed platform. Compare that to custom images, where you modify an Amazon Machine Image (AMI) for use with an existing Elastic Beanstalk platform, and Elastic Beanstalk still provides the platform scripts and controls the platform's software stack. Besides, with custom platforms, you use an automated, scripted way to create and maintain your customization, whereas with custom images you make the changes manually over a running instance.

**S3 Select** enables applications to retrieve only a subset of data from an object by using simple SQL expressions. By using S3 Select to retrieve only the data needed by your application, you can achieve drastic performance increases in many cases you can get as much as a 400% improvement.
**S3 Inventory** - Amazon S3 inventory is one of the tools Amazon S3 provides to help manage your storage. You can use it to audit and report on the replication and encryption status of your objects for business, compliance, and regulatory needs.
**S3 Analytics** - By using Amazon S3 analytics storage class analysis you can analyze storage access patterns to help you decide when to transition the right data to the right storage class. This new Amazon S3 analytics feature observes data access patterns to help you determine when to transition less frequently accessed STANDARD storage to the STANDARD_IA (IA, for infrequent access) storage class.
**S3 Access Logs** - Server access logging provides detailed records for the requests that are made to a bucket. Server access logs are useful for many applications. For example, access log information can be useful in security and access audits. It can also help you learn about your customer base and understand your Amazon S3 bill.
Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)

**Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS)**

Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS) is similar to SSE-S3, but with some additional benefits and charges for using this service. There are separate permissions for the use of a CMK that provides added protection against unauthorized access of your objects in Amazon S3. SSE-KMS also provides you with an audit trail that shows when your CMK was used and by whom. Additionally, you can create and manage customer managed CMKs or use AWS managed CMKs that are unique to you, your service, and your Region.

**When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key**. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates. Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256), to encrypt your data
`aws ecs create-service --service-name ecs-simple-service --task-definition ecs-demo --desired-count 10`
To create a new service you would use this command which creates a service in your default region called ecs-simple-service. The service uses the ecs-demo task definition and it maintains 10 instantiations of that task.

Time To Live (**TTL**) for DynamoDB allows you to define when items in a table expire so that they can be automatically deleted from the database. TTL is provided at no extra cost as a way to reduce storage usage and reduce the cost of storing irrelevant data without using provisioned throughput. With TTL enabled on a table, you can set a timestamp for deletion on a per-item basis, allowing you to limit storage usage to only those records that are relevant.