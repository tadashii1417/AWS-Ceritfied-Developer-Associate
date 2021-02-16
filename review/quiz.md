**1. I have an ASG and an ALB, and I setup my ASG to get health status of instances thanks to my ALB. One instance has just been reported unhealthy. What will happen?**

A. The ASG will keep the instance running and re-start the application
B. The ASG will detach the EC2 instance from the group, and leave it running
C. The ASG will terminate the EC2 Instance

ANS: C

**2. You would like to expose a **fixed static IP** to your end-users for compliance purposes, so they can write firewall rules that will be stable and approved by regulators. Which Load Balancer should you use?**

A. Application Load Balancer with Elastic IP attached to it
B. NLB
C. CLB

ANS: B
Note:
- Elastic IP is not feasible with ALB
- Network Load Balancers expose a public static IP, whereas an Application or Classic Load Balancer exposes a static DNS (URL)

**3. A web application hosted in EC2 is managed by an ASG. You are exposing this application through an Application Load Balancer. The ALB is deployed on the VPC with the following CIDR: 192.168.0.0/18. How do you configure the EC2 instance security group to ensure only the ALB can access the port 80?**

A. Open up the EC2 security group on port 80 to 0.0.0.0/0
B. Open up the EC2 security group on port 80 to 192.168.0.0/18
C. Open up the EC2 security on port 80 to the ALB's security group

ANS: C
- This is the most secure way of ensuring only the ALB can access the EC2 instances. Referencing by security groups in rules is an extremely powerful rule and many questions at the exam rely on it. Make sure you fully master the concepts behind it!

![LBSG](./images/LBSG.png)

**4. Your application load balancer is hosting 3 target groups with hostnames being users.example.com, api.external.example.com, and checkout.example.com. You would like to expose HTTPS traffic for each of these hostnames. How do you configure your ALB SSL certificates to make this work?**

A. SNI
B. Use a wildcard SSL certificate
C. Use an HTTP to HTTPS rule

ANS: SNI
- SNI (Server Name Indication) is a feature allowing you to expose multiple SSL certs if the client supports it. Read more here: https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/

**5. You are running an application in 3 AZ, with an Auto Scaling Group and a Classic Load Balancer. It seems that the traffic is not evenly distributed amongst all the backend EC2 instances, with some AZ being overloaded. Which feature should help distribute the traffic across all the available EC2 instances?**

A. Stickiness
B. Cross Zone Load Balancing
C. Target Group Routing Rules.
D. HTTPS termination.

ANS: B

**6. An application is deployed with an Application Load Balancer and an Auto Scaling Group. Currently, the scaling of the Auto Scaling Group is done manually and you would like to define a scaling policy that will ensure the average number of connections to your EC2 instances is averaging at around 1000. Which scaling policy should you use?**

A. Simple Scaling Policy
B. Step Scaling Policy
C. Target Tracking
D. Scheduled Scaling

**7. An application is deployed with an Application Load Balancer and an Auto Scaling Group. Currently, the scaling of the Auto Scaling Group is done manually and you would like to define a scaling policy that will ensure the average number of connections to your EC2 instances is averaging at around 1000. Which scaling policy should you use?**

Target Tracking Scaling
Simple Scaling
Step Scaling
Scheduled Actions

ANS: A

------------------------------------

**8. You are running a high-performance database that requires an IOPS of 210,000 for its underlying filesystem. What do you recommend?**

A. Use an EBS gp2 drive
B. Use an EBS io1 drive
C. Use an EC2 Instance Store
D. Use EFS

ANS: C
Is running a DB on EC2 instance store possible? It is possible to run a database on EC2. It is also possible to use instance store, but there are some considerations to have. The data will be lost if the instance is stopped, but it can be restarted without problems. One can also set up a replication mechanism on another EC2 instance with instance store to have a standby copy. One can also have back-up mechanisms. It's all up to how you want to set up your architecture to validate your requirements. In this case, *it's around IOPS, and we build an architecture of replication and back up around it*
B: max of io1 is 64000 IOPS

--------------------------------------------------------
**9. My company would like to have a MySQL database that is going to be available even in case of a disaster in the AWS Cloud. I should setup**

A. Read replicas
B. Encryption
C. Multi AZ

ANS: C, Multi AZ = disaster

**10. Our RDS database struggles to keep up with the demand of the users from our website. Our million users mostly read news, and we don't post news very often. Which solution will NOT help fix this problem?**

A. ElastiCache
B. Read replica
C. Multi AZ

ANS: C

**11. Which RDS Classic (not Aurora) feature does not require us to change our SQL connection string?**

A. Read replica
B. Multi AZ

ANS: A
Read Replicas add new endpoints for databases to read from and therefore we must change our application to have the list of these endpoints in order to balance the read load and connect to the databases.

Multi AZ keeps the same connection string regardless of which database is up. Read Replicas imply we need to reference them individually in our application as each read replica will have its own DNS name

**12. You want to ensure your Redis cluster will always be available**

A. Read replica
B. Multi AZ

ANS: B

**13. You have a requirement to use TDE (Transparent Data Encryption) on top of KMS. Which database technology does NOT support TDE on RDS?**

A. PostgreSQL
B. MS SQL Server
C. Oracle

ANS: A

**14. Which RDS database technology does NOT support IAM authentication?**

A. Oracle
B. PostgresSQL
C. MySQL

ANS: A

**15.You would like to ensure you have a database available in another region if a disaster happens to your main region. Which database do you recommend?**

A. RDS with Read Replicas in another AZ
B. RDS with Multi AZ
C. Aurora Read Replicas in another AZ
D. Aurora Global Database

ANS: D
Global Databases allow you to have cross region replication

**16. You are serving web pages for a very dynamic website and you have a requirement to keep latency to a minimum for every single user when they do a read request. Writes can take longer to happen. Which caching strategy do you recommend?**

A. Cache aside
B. Write-through
C. TTL

ANS: B
A. cache miss read 3 -> fail
B. this has longer writes, but the reads are quick and the data is always updated in the cache
