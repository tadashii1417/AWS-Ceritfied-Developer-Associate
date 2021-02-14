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
