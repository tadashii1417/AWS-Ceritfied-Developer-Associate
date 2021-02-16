# Revision

## Section 3: IAM + EC2

- If your application is **not accessible (time out)**, then it's a **security group** issue.
- If your application give a **"connection refused" error**, then it's an **application error** or it's not launched.
- Overall, try to avoid using Elastic IP:
  - They often reflect poor architecture desicion.
  - Instead, use a random public IP and register a DNS name to it.
  - use Load balancer is a good option.

- EC2 Instance Launch Types
  - **On demand**: short workload, *highest price*
=> Recommended for short-term and un-interrupted workloads.

  - **Reserved**: long workload
    - Convertible reserved instances:
    - Scheduled reserved instances:
  - **Spot instance**: short workload, for cheap, lose instances(less reliable) if your max price is less than the current price.
=> Useful for workloads that are resilient to failure: batch jobs, data analysis, ...

  - **Dedicated Instances**: share instance with no one, may share hardware with other instances; no hardware control (instance placement)
  - **Dedicated Hosts**: book an entire physical server, control everything.
=> Recommended for software that have complicated licensing model (BYOL - Bring your own license); companies that have strong regulatory & compliance needs.

- Elastic Network Interface (ENI):
  - Logical component in a VPC that represent a virtual network card.
  - It have:
    - Primary private IPv4, one or more secondary IPv4
    - One Elastic IP per private IPv4
    - One public IP + MAC address
  - Create ENI and attach them on EC2 instances for failover.

- Other notes:
  - SSH into EC2 (remember to change .pem file permissions)
  - ...

---------------

## Section 4: ELB + ASG

### 4.1 Elastic Load Balancer

\* *Scalability:*

- **Vertical** scalability:
  - Increase size of the instance
  - Services that can scale vertically: **RDS**, **ElasticCache**.
- **Horizontal** scalability:
  - Increase the number of instances
  - Used in distributed system

\* *Why use a load balancer ?*

- Single point of access (DNS)
- Health check, SSL termination (HTTPS)
- Seperate public traffic from private traffic.

\* *Types of load balanceer:*

- **Classic Load Balancer**: HTTP, HTTPS, TCP
  - TCP (layer 4), HTTP (layer 7)
  - Fixed hostname
  - Can only have one SSL certificate.

- **Application Load Balancer**: HTTP, HTTPS, WebSocket
  - HTTP (layer 7)
  - Fixed hostname
  - Support HTTP/2 & WebSocket, redirect from HTTP to HTTPS
  - Routing table based on path, hostname, query string.
  - Great for micro services & container-based application
  - Has a ***port mapping feature*** to redirect to a dynamic port in ECS
  - **Targets** are:
    - EC2 instances
    - ECS tasks
    - Lambda function
    - IP addresses -> *Must be private IPs*

  - Support multiple listeners with multiple SSL Cert, Use SNI to make it work.

  - ***Note***: The application (ex: EC2 instance) don't see the IP of the client directly, the true IP is inserted in the header ***X-Forwarded-For***, any other information like Port, proto, etc.

- **Network Load Balancer**: TCP & UDP & TLS (secure TCP)
  - Forward TCP & UDP traffic to your instances.
  - Extremely high performance: milions of request
  - Low latency
  - NLB has *one static IP **per** AZ*, support assigning **Elastic IP** (**Only NLB have this.)**
  - Used for extreme performance, TCP & UDP trafic.
  - Support multiple listeners with multiple SSL Cert, Use SNI to make it work.

\* *Good to know about Load balancer:*

- LBs can be scale but not instanteneously.
  - Troubleshooting
    - 4xx: client induced errors.
    - 5xx: application induced errors.
    - 503: at capacity or no registerd target.

\* *Load balancer **Stickiness**:*

- be able that same client always redirected to the same instance.
- Only work for **CLB & ALB**
- Make sure the session data of user is saved.
- May bring imbalance to the load.

\* *Cross-zone Load Balancing*

- each load balancer instance *distribute evenly accross all instances* in all AZ.
- ALB: by default always on CZB, can't be disabled.
- CLB & NLB: disabled by default.

\* *SSL - Server Name Indication (SNI)*

- SNI solves the problem of **loading multiple SSL certificates onto one web server** (to serve multiple websites)
- requires the client to indicate the hostname of the target server then find & return correct SSL cert.
- Only work for new gen (ALB&NLB), CloudFront.

### 4.3 Auto Scaling Group

- ASGs use **Launch configurations** or **Launch Template** (newer)
- IAM roles attached to an ASG will get assigned to EC2 instances
- **Scaling Policies**
  - *Target Tracking Scaling*
    - Most simple and easy to setup
    - Example: I want the average ASG CPU to stay at around 40%
  - *Simple/Step Scaling*
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
  - *Scheduled Actions*
    - Anticipate a scaling based on known usage patterns
    - Example: increase the min capacity to 10 at 5 pm on Fridays
- The cooldown period helps to ensure that your Auto Scaling group doesn't launch or terminate additional instances before the previous scaling activity takes effect

## 5.EC2 Storage Section

### 5.1 EBS Volumn

- It’s a network drive (i.e. not a physical drive)
- It can be detached from an EC2 instance and attached to another one quickly
- It’s **locked** to an Availability Zone (AZ)
  - *To move a volume across, you first need to snapshot it*
- EBS Volumes come in 4 types
  - **GP2** (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads
    - Recommended for most workload
    - System boot volumes
    - Virtual desktops
    - Low-latency
    - Development & test env.
  
  - **IO1** (SSD): Highest-performance SSD volume for mission-critical low-latency or high- throughput workloads
    - Critical business applications that require performance.
  - **ST1** (HDD): Low cost HDD volume designed for frequently accessed, throughput- intensive workloads
    - Streaming workloads requiring consistent, fast throughput at low price.
  - **SC1** (HDD): Lowest cost HDD volume designed for less frequently accessed workloads
    - data that is infrequent access.
    - scenarios where the lowest storage cost is important.
  - ***Only** GP2 and IO1 can be used as boot volumes*
- ***Note***: EBS can only be attached to only one EC2 instance.

### 5.2 EFS Elastic File System

- Managed NFS (Network File System) that can be mounted on **many EC2.**
- Work with EC2 instances in multi-AZ
- Highly available, *pay per use.*
- Encryption at rest using KMS

![EFS](images/efs.png)
- Note:
  - EBS only mounted into a single EC2
  - EFS can be mounted to many
  - Need to add permission on EC2 instances to access EFS.

- Storage Tiers
  - **Standard**: for frequently accessed files
  - **Infrequent access (EFS-IA)**: cost to retrieve files, lower price to store
  - The idea is: after N days, if not use then move to EFS-IA to save cost.

|                          | EBS                                                                                                                               | EFS                     |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------|
| Number of EC2  instances | 1                                                                                                                                 | n                       |
| Scope                    | AZ                                                                                                                                | Global                  |
| Types                    | - GP2: General Purpose Volumes <br/>- IO1: Provisioned IOPS (expensive)<br/> - ST1: Optimized HDD<br/> - SC1: Cold HDD, Infrequent accessd data. | - Standard<br/> - EFS-1A     |
| Notes                    | - Root EBS get terminated by default if  EC2 instance gets terminated.                                                            | - Only work for  linux. |

- Difference between: **EFS**(Multi-AZ), **EBS**(Single AZ) vs **Instance Store**(maximum of IO, ephemeral drive.)

## 6. AWS Fundamentals

### 6.1 RDS Overview

- Allow create DB that:
  - Postgres, MySQL, Oracle, Microsoft SQL Server
  - MariaDB
  - Aurora
- Continuous backups and restore to specific timestamp.
- Maintainance window for upgrades
- Read replica
- Multi AZ setup for disaster Recovery
- Scaling capability (*vertical & horizontal*)
- **BUT** you can't SSH into your instances.

- RDS Read Replica: 
  - within AZ-free, Cross AZ, Cross Region.
  - Replication is ASYNC, so reads are consistent
  - Application must **update the connection string** to leverage
  
- **RDS Multi AZ(Disaster Recovery)**
  - SYNC replication
  - One DNS name – automatic app failover to standby
  - Increase availability
  - Failover in case of loss of AZ, loss of network, instance or storage failure
  - Not used for scaling
  - Note:The Read Replicas be setup as Multi AZ for Disaster Recovery (DR). Overally, multi AZ simply use for backup, just stay there, no one read, no one write in case of failover.

- **RDS Encryption**
  - If the master is not encrypted, the read replica cannot be encrypted.
  - Transparent Data Encryption (TDE) only available for **Oracle & SQL Server.**

- **RDS Authentication**
• IAM database authentication works with **MySQL and PostgreSQL**
• You don’t need a password, just an authentication token obtained through IAM & RDS API calls
• Auth token has a lifetime of 15 minutes

### 6.2 Aurora

- Postgres & MySQL are both supported by Aurora
- Cloud optimized, performance improvement RDS
- 6 copies of your data accross 3 AZs.
- Support for **Cross Region Replication**
- Simply: Replication, Self-healing, Auto Expanding

- **Aurora Serverless:**
  - Automated database instantiation and auto scaling based on actual usage
  - Good for **infrequent**, intermittent or **unpredictable** workloads
  - No capacity planning needed
  - Pay per second, can be more cost effective

- **Global Aurora:**
  - Aurora Cross Region Read Replicas, Useful for disaster recovery
  - 1 Primary Region (read / write), Up to 5 secondary (read-only) regions.

### 6.3 ElastiCache

- Use cases: User session store.

| Redis                                                                                                                                                                        | Memcached (minify version)                                                                                                          |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| • Multi AZ with Auto-Failover<br>• Read Replicas to scale reads<br>and have high availability<br>• Data Durability using AOF<br>persistence<br>• Backup and restore features | • Multi-node for partitioning of<br>data (sharding)<br>• Non persistent<br>• No backup and restore<br>• Multi-threaded architecture |

- Caching design patterns:

| Lazy Loading / Cache-Aside / Lazy Population                                                                                                                                                                                                                                                                       | Write-Through<br>add or update cache when database is updated.                                                                                                                                                                                             |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pros:<br>• Only requested data is cached (isn’t filled up with unused data)<br>• Node failures are not fatal <br>(just increased latency to warm the cache)<br>• Improve read performance<br><br>Cons:<br>• Cache miss penalty that results in **3 round trips**.<br>• **Stale data**: data can be updated in the database but outdated in the cache| Pros:<br>• Data in cache is **never stale**, reads are quick<br>• Write penalty vs Read penalty <br>(each write requires **2 calls**)<br><br>Cons:<br>• Missing Data until it is added / updated in the DB<br>• **Cache churn** – a lot of the data will never be read |

```py
# Lazy Loading/Cache-aside/Lazy Population
# Note: when get data from db.
def get_user(user_id):
    record = cache.get(user_id)
    if record is None:
        record = db.query("select * from ...")
        cache.set(user_id, record)
    
    return record
```

```py
# Write through
# Note: when database is updated
def save_user(id, values):
    record = db.query("update users ...")
    cache.set(id, record)
    return record
```

- Write through is usually combined with Lazy Loading
- Setting a TTL is usually not a bad idea, **except** when you’re using Write- through. Set it to a sensible value for your application


## 7. Route 53

- is a global service, is a Managed DNS
- The most commons records are:
  - **A** hostname to IPv4
  - **AAAA** IPv6
  - **CNAME** hostname to hostname
  - **Alias** hostname to AWS resource

- DNS Records **TTL**:
  - TTL is a way to cache response of a DNS query (IP) to not overload the DNS.
  - TTL is mandatory for each DNS record

- Remember:
  - Root domain: ex: mydomain.com
  - Non-root domain: ex blabal.mydomain.com
  
- **CNAME vs Alias**

| CNAME                                         | Alias                                                  |
|-----------------------------------------------|--------------------------------------------------------|
| • Hostname to hostname<br>• ONLY FOR NON-ROOT | • Hostname to AWS resource<br>• ROOT & NON-ROOT domain |

- **Routing policies:**
  - **Simple** Routing policy:
    - hostname to hostname
    - can't attach health checks
    - if multiple values are returned, a random one is chosen by the client
  - **Weighted** Routing Policy:
    - Helpful to test 1% on new version
    - Split traffic between two regions
    - Can be associated with Health check
  - **Latency** Routing Policy:
    - Redirect to the server that has the least latency close to us
    - Helpful when latency is a priority
    - minimizing the response time from your servers to your users
    - Latency is evaluated in terms of user to designated AWS Region
  - **Failover** Routing Policy:
    - Have Primary & Secondary version
    - Health check is mandatory in Primary, but not required in secondary.
  - **Geo Location** Routing Policy
    - based on user location
    - Restrict access from some country for ex.
  - **Multi Value** Routing Policy
    - Use when routing traffic to multiple resources
    - Want to associate a Route 53 health checks with records
    - Multi Value is not a substitute for having an ELB, but do some kind of LB.

- Famous domain name registrar: GoDaddy, Google Domains, ... & Route53