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

Scalability:

- **Vertical** scalability:
  - Increase size of the instance
  - Services that can scale vertically: **RDS**, **ElasticCache**.
- **Horizontal** scalability:
  - Increase the number of instances
  - Used in distributed system

Why use a load balancer ?

- Single point of access (DNS)
- Health check, SSL termiation (HTTPS)
- Seperate public traffic from private traffic.

Types of load balanceer:

- **Classic Load Balancer**: HTTP, HTTPS, TCP
  - TCP (layer 4), HTTP (layer 7)
  - Fixed hostname

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
  - ***Note***: The application (ex: EC2 instance) don't see the IP of the client directly, the true IP is inserted in the header ***X-Forwarded-For***, any other information like Port, proto, etc.

- **Network Load Balancer**: TCP & UDP & TLS (secure TCP)
  - Forward TCP & UDP traffic to your instances.
  - Extremely high performance: milions of request
  - Low latency
  - NLB has ***one static IP per AZ***.

Good to know about Load balancer:

- LBs can be scale but not instanteneously.
  - Troubleshooting
    - 4xx: client induced errors.
    - 5xx: application induced errors.
    - 503: at capacity or no registerd target.

Load balancer Stickiness:

- Same client always redirected to the same instance.
- Only work for **CLB & ALB**
- Make sure the session data of user is saved.
- May bring imbalance to the load.

Cross-zone Load Balancing

- With Cr