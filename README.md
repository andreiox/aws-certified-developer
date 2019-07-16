# aws-certified-developer

Study/Notes for AWS Certified Developer Associate Exam

## **IAM**

- Divided in Users, Groups, Roles, Policies
- Policies defines what users, groups and roles can and cannot do

## **EC2**

- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS)
- Distributing load across machines (ELB)
- Scaling the services using an auto-scaling group (ASG)

### **Security Groups**

Like a firewall to EC2 instances

! **Regulates**

- Access to ports
- Authorised IP ranges
- Control inbound and outbound network

! **Good 2 know**

- Segurity Groups has a N to N relationship with Instances
- Locked down to a region
- Does live outside of the instance. If the traffic is blocked then the instance won't see it
- Good to maintain one separate group for SSH access
- If connection timeout for the instance > security group problem
- If connection refused > application problem
- default: all inbound is blocked, all outbound is allowed
- Security groups can allow/disallow other security groups, not only IPs

### **EC2 User Data**

- Launch commands when a machine starts
- The script is only run once at the instance first start
- EC2 user data is used to automate tasks such as: installing updates, installing software, download files, etc
- Script runs with root user
- Lembrar de colocar #!/bin/bash na primeira linha

### **EC2 Instance Launch Types**

! **EC2 On Demand**

- Pay for what you use (billing per second, after the first minute)
- Has the highest cost but no upfront payment
- No long term commitment

! **EC2 Reserved Instances**

- Up to 75% discount compared to On Demand
- Pay upfront with long term commitment
- Reservation period can be 1 or 3 years
- Reserve a specific instance type

!! **Convertible Reserved Instance**

- Can change the EC2 instance type
- Up to 54% discount

!! **Schedule Reserved Instances**

- Launch within time window you reserve
- When you require a fraction of day/week/month

! **EC2 Spot Instances**

- Can get a discount of up to 90% compared to On Demand
- You bid a price and get the instance as long as its under the price
- Price varies based on offer and demand
- Spot instances are reclaimed with a 2 minute notification warning when the spot price goas above your bid
- Used for batch jobs, Big Data Analysis, or workloads that are resilient to failures
- Not great for critical jobs or databases

! **EC2 Dedicated Hosts**

- Physical dedicated EC2 server
- Full control of EC2 instance placement
- Visibility into the sockets/physical cores of the hardware
- Allocated for 3 year period reservation
- More expensive
- Useful for software that have complicated licensing model
- For companies that have strong regulatory or compliance needs

! **EC2 Dedicated Instances**

- Instances running on hardware that's dedicated
- May share hardware with other instances in same account
- No control over placement

### **EC2 Pricing**

- EC2 instances prices (per hour) varies based on: region, instance type, on demand vs spot vs reserved vs dedicated host, linux vs windows vs etc
- You are billed by the second, with a minimum of 60 seconds
- Other factors too such as storage, data transfer, fixed IP, public addresses, load balancing
- Instance that is stopped is not charged

### **AMI**

- Base images such as: ubuntu, fedora, redhat, windows, etc
- Can be customized at runtime using EC2 User Data

! **Custom AMI**

- Used when every machine uses the same user data
- Faster boot time
- Configure all machines with same packages, configurations, etc
- **AMI are built for a specific AWS region**

### **EC2 Instances Overview**

Instances have 5 distinct characteristics:

- RAM (type, amount, generation)
- CPU (type, make, requency, generation, cores)
- I/O (disk performance, EBS optimisations)
- Network (bandwidth, latency)
- GPU

! **Burstable Instances (T2)**

- Burst means that overall, the instance has OK CPU performance
- But when the machine needs to process something unexpected (a spike in load for example), it can burst, and CPU can be VERY good
- If it bursts, it utilizes "burst credits"
- If all credits are gone, the CPU becomes BAD
- If machine sop bursting, credits are accumulated over time

! **T2 Unlimited**

- Its possible to have and "unlimited burst credit balance"
- You pay extra money if you go over your credit

### **EC2 Checklist**

- Know how to SSH into EC2 (change .pem permissions)
- Know how to properly use security groups
- Know the fundamental differences between private, public and elastic IP
- Know how to use User Data
- Know that you can build custom AMI
- Know that EC2 instances are billed by the second and can be easily created and thrown away

## **Load Balancing**

Load balancers are servers that forward internet traffic to multiple servers (instances) downstream (like a middleware)

### **Why use a load balancer?**

- Spread load across multiple downstream instances
- Expose a single point of access to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### **Why use a EC2 Load Balancer?**

- ELB is a managed load balancer
- AWS guarantees that it will be working
- AWS takes care of upgrades, maintenance, high availability
- AWS provides only a few configuration knobs
- It costs less to setup your own load balancer but it will be more effort from you
- It is integrated with many AWS offerings/services

### **Types of load balancer on AWS**

- Classic Load Balancer (v1 - old) - 2009
- Application Load Balancer (v2 - new) - 2016
- Network Load Balancer (v2 - new) - 2017

Overall, it is recommended to use the newer/v2 laod balancers as they provide more features.

It is possible to setup internal (private) or external (public) ELBs.

### **Health Checks**

- Crucial for Load Balancers
- They enable the laod balancer to know if instances it forwards traffic to are available
- Health check is done on a port and a route (/health is common)

### **Application Load Balancer (v2)**

Application load balancers (Layer 7) allow to do:

- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (ex: containers)
- Load balancing based on route in URL
- Load balancing based on hostname in URL

In comparison, we would need to create one Classic Load Balancer per application before. This is expensive and inefficient.

! **Good 2 know**

- Stickiness can be enabled at the target group level. Same client goes to the same instance. It is generated by the ALB (not the application)
- Supports HTTP/HTTPS & Websockets protocols
- The application server dont see the IP of the client directly. It will be in the X-Forwarded-For, X-Forwarded-Port, X-Forwarded-Proto headers

### **Network Load Balancer (v2)**

Network Load Balancer (Layer 4) allow to do:

- Foward TCP traffic to your instances
- Handle millions of requests per second
- Support for static IP or elastic IP
- Less latency ~100ms (vs 400ms for ALB)

Network Load Balancer are mostly used for extreme performance and should not be the default load balancer you choose.

Overall, the creation process is the same as ALB

### **Load Balancer Good 2 know**

- Classic Load Balancers are Deprecated. Use ALB for HTTP/HTTPS & Websocket and NLB for TCP
- CLB and ALB support SSL certificates and provide SSL termination
- All Load Balancers have health check capability
- ALB can route on based on hostname/path
- ALB is a grat fit with ECS (Docker)
- Any Load Balancer has static host name. Do not resolve and use underlying IP
- Load Balancers can scale but not instantaneously - contact AWS for a "warm-up" so that they scale
- NLB direcly see the client IP
- 4xx errors are client induced errors
- 5xx errors are application induced errors
- Load balancer Errors 503 means at capacity or no registered target
- If the Load Balancer cant connect to application, check security groups

## **Auto Scaling Group**

The goal is to:

- Scale out (add EC2 instances) to match an increased load
- Scale in (remove EC2 instances) to match a decreaased load
- Ensure we have a minimum and a maximum number of machines running
- Automatically Register new instances to a load balancer

Attributes:

- Launch configuration (AMI + instance type, user data, ebs, security groups, ssh key pair)
- Min size/Max size/Initial Capacity
- Network + Subnets Information
- Load Lalancer Information
- Scaling Policies

### **Auto Scaling Alarms**

- It is possible to scale an ASG based on CloudWatch alarms
- An Alarm monitors a metric (suc as Average CPU usage)
- Metrics are computed for the overall ASG instances
- Based on the alarm we can create scale-out and scale-in policies

### **Auto Scaling New Rules**

- Target Average CPU usage
- Number of requests on the ELB per instance
- Average Network In
- Average Network Out

### **Auto Scaling based on Custom Metric**

It is possible to auto scale based on custom metric (ex: number of connected users)

1. Send custom metric from application on EC2 to CloudWatch (PutMetric API)
2. Create CloudWatch alarm to react to low/high values
3. Use the CloudWatch alarm as the scaling policy for ASG

### **ASG Summary**

- Scaling policies can be on CPU, Network... and can ever be on custom metrics or based on a schedule
- ASGs use Launch configurations and you update an ASG by providing a new launch configuration
- IAM roles attached to an ASG will get assigned to EC2 instances
- ASG are free
- Having instances under an ASG means that if they get terminated for whatever reason, the ASG will restart them. Extra safety
- ASG can terminate instances market as unhealthy by an LB (and hence replace them)

## **EBS Volume**

An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run. It allows your instances to persist data.

Its a network drive (not a physical drive).

- It uses the network to communicate the instance, which means there might be a bit of latency
- It can be detached from a EC2 instance and attached to another one quickly
- Its locked to an Availability Zone (an EBS Volume in us-east-1a cannot be attached to us-east-1b)
- To move a volume across, you need to snapshot it
- Have a provisioned capacity (size in GBs, and IOPS)
- Billed for all the provisioned capacity
- Can increase capacity of the drive over time

### **EBS Volume Types**

- GP2 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads
- IO1 (SSD): Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads
- ST1 (HDD): Low cost HDD volume designed for frequently access, throughput-intensive workloads
- SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads

### **EBS Volume Resizing**

Feb 2017: You can resize the EBS volumes in:

- Size (any volume type)
- IOPS (only IO1)

After resizing an EBS volume, you need to repartition your drive

### **EBS Snapshots**

- EBS Volumes can be backed up using "snapshots"
- Snapshots only take the actual space of the blocks on the volume. If you snapshot a 100GB drive that only has 5GB of data, then the snapshot will only be 5GB

Snapshots are used for:

- Backups
- Volume migration (resizing a volume down, changing the volume type, encrypt a volume)

### **EBS Encryption**

When you create an encrypted EBS volume, you get the following:

- Data at rest is encrypted inside the volume
- All the data in flight moving between the instance and the volume is encrypted
- All snapshots from a encrypted volume are encrypted
- All volumes created from a encrypted snapshot are encrypted

Attributes:

- Encryption and decription are handle transparently
- Encryption has a minimal impact on latency
- EBS Encryption leverages keys from KMS (AES-256)
- Copying an unencrypted snapshot allows encryption

### **EBS vs Instance Store**

- Some instance do not come with Root EBS volumes
- Instead, they como with "Instance Store"
- Instance store is physically attached to the machine
- You got better I/O performance out of that
- But on termination, instance store is lost, you cant resize the instance store and backups must be operated by the user
- Overall, EBS-backed instances should fir most applications workloads. Except if you need extremely high performance and have extremely good devops engineers you would use Instance Store

### **EBS Summary**

- EBS can be attached to only one instance at a time
- EBS are locked at the AZ (Availability Zone) level
- Migrating an EBS volume across AZ means first backing it up (snapshot), then recreating it in the other AZ
- EBS backups use IO and you shouldnt run them while your application is handling a lot of traffic
- Root EBS Volumes of instances get terminated by default if the EC2 isntance gets terminated (can be disabled)

## **References**

- [Udemy Course - Ultimate AWS Certified Developer Associate 2019](https://www.udemy.com/aws-certified-developer-associate-dva-c01/)
