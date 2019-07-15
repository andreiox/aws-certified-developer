# aws-certified-developer

Study for AWS Certified Developer Associate Exam

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

## **References**

- [Udemy Course - Ultimate AWS Certified Developer Associate 2019](https://www.udemy.com/aws-certified-developer-associate-dva-c01/)
