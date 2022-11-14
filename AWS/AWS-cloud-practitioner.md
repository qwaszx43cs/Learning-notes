## Cloud computing



### Cloud Computing definition

On-demand availability of computer system resources, especially data storage(cloud storage) and computing power, without direct active management by the user



### Four points of considerations when choosing the AWS region:

- latency
- compliance with data governmance and legal requirements
- proximity to customer, available services and features of Region
- pricing



### AWS Regions are composed of ?

AWS Regions are composed of **two or more AS(Availability Zones)**



### The distribution of responsibilities for security in the AWS Cloud

The Shared Responsibility Model(云计算责任共担模式)



(点击查看🔗)[https://www.synopsys.com/blogs/software-security/shared-responsibility-model-cloud-security/]



## IAM 



**IAM**: Identity and Access Management (**Global Service**)

**Root account**: created by default, shouldn't be used and shared

**Users**: people within your organization, and can be grouped

**Groups**: groups can only contain users, not other groups

![image-20221019091632707](/Users/sky/Library/Application Support/typora-user-images/image-20221019091632707.png)



### Policies

policies: **Users** and **Groups** can be assigned JSON documents

![image-20221019092351103](/Users/sky/Library/Application Support/typora-user-images/image-20221019092351103.png)



**Least privilige principle**: In AWS, you apply least privilige pricinple: Don't give more permission than a user need

#### structure

![image-20221019135445899](/Users/sky/Library/Application Support/typora-user-images/image-20221019135445899.png)



#### password policy

##### MFA

MFA(multi factor authentication): to protect your root user and IAM user

MFA = password + security device you own

**main benefit of MFA**: Even your account password is stolen or hacked, your account is not compromised.



MFA devices options in AWS

- virtual MFA device 
  - google authenticator
  - Authy
  - ...
- 3rd party physical device



#### Access Key, CLI and SDK

**Access Key**:

**SDK**: enables user to access and manage AWS services programmatically



###  Cloudshell

 

### IAM Roles



### Security Tools

	- IAM Credentials Report(account-level)
	- IAM Access Advisor(user-level)



### IAM Best Practice

- Do not use root account except for AWS setup
- One physical user = one AWS user
- Assign users to groups and assign permissions to groups
- use strong password policy
- use and enforce MFA(multi factors Authentation)
- create and use Roles for giving permissions to AWS services
- get Access Keys for Programmatic Access(CLI/SDK)
- Audit permissions of your account with the IAM credential Report
- Never share IAM users and Access Keys



### IAM Summary

![image-20221020143753278](/Users/sky/Library/Application Support/typora-user-images/image-20221020143753278.png)





## EC2 



### Amazon EC2

EC2 = elastic compute cloud = infrastrcuture as a service

it consists of :

- Renting virtual machines(ec2)

- Storing data on virtual drives(EBS)

- Distributing load across machines(ELB)

- scaling the service using an auto-scaling group(ASG)

  

### EC2 sizing and configuration options

- Operation System(OS)

  **Centos7 AMI版本清单 (Link)**[https://wiki.centos.org/Cloud/AWS#head-25abbf7b526d7fa3f58b5b8ad31aa4a16377769b]

- Computer Power & core (cpu)

- RAM

- storage space

  - Network-attached storage(EBS & EFS)
  - Hardware(EC2 instance Store)

- Network card

- Firewall rules

- Bootstrap scripts: EC2 user Data



### EC2 User Data

```sh
#!/bin/bash
# Use this for your user data(script from top to bottom)
# install httpd
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```



### EC2 Instance Types

#### Overview

> m5.2xlarge

- m: instance class

- 5: generation (AWS improve them over time)
- 2xlarge: size within the instance class

####  General Purpose



#### Compute Optimized

C6g, C6gn, C5, C5a, C5n, C4..



#### Memory Optimized

 

#### Storage Optimized

 



[EC2 Instance types](https://instances.vantage.sh/)



### Security Groups

security groups control how traffic is allowed in or out of AWS ec2 Instances (firewalls)

Security Groups only contain **allow** rules

they regulate:

- Access to ports
- Authorised IP ranges -IPV4 and IPV6



#### Good to know

- locked down to a region/vpc combination
- *Its good to maintain one separate security group for SSH access*
- All inbound traffic is **blocked** by default
- All outbound traffic is authorised by default



### Classic Ports to know

- SSH: 22
- FTP: 21
- SFTP: 22
- HTTP: 80
- HTTPS: 443
- MYSQL: 3306



### SSH



### EC2 Instance Connect



### EC2 Instance Role



#### EC2 Instance purchasing Options

- On-demand instance
- Reserved(1 & 3 years) instance
- savings plans
- spot instances
- dedicated hosts
- dedicated instances
- capacity reservations

#### Shared Responsibility Model for EC2

![image-20221026135043381](/Users/sky/Library/Application Support/typora-user-images/image-20221026135043381.png)





## EBS - EC2 Instance Storage

EBS : Elastic Block Store



#### Delete on termination

root EBS deleted by default when EC2 instance determinated



#### Snapshots

- features
  - EBS Snapshots archive(for addtional pricing)
  - recycle bin for EBS Snapshots



#### Recycle bins



### AMI

AMI: Amazon Machine Image



AMI Process:

1. start an EC2 instance and customize it
2. stop the instance(for data intergrity)
3. build an AMI - this will also create EBS snapshots
4. Launch instances from other AMIs





#### EC2 Image builder



#### EC2 Instance Store

local EC2 Instance store: high performance hardware attached volume for EC2 Instance



#### EFS

EFS: elastic file system

EFS works with **Linux **EC2 instances in mutil-AZ



EFS-IA(Elastic file system - infrequent access)

EFS-IA: storage class that is cost-optimized for files not accessed every day





#### Shared responsibility model for EC2 storage





#### FSX

FSX built on Windows File Server



#### Summary

- EBS volumes:
  - network drives attached to one EC2 instance at a time
  - mapped to an Avalibility Zone
  - use snapshots for backups/ transferring EBS to another AZ
- AMI
  - a ready to use EC2 instance with my customization
- EC2 Instance builder
  - automatically build, test, ditribute AMI
- EC2 Instance store
  - a high performance hardware disk attached to EC2 instance
  - lost if EC2 instance stopped or terminated

- EFS
  - network file system (Linux) could attached to hundreds of EC2 Instances in a region
- EFS-IA
  - cost-optimized storage class for infrequent access files
- FSX for windows: network file system for windows
- FSx for Lustre: high performance computing Linux file system





## ELB & ASG

Scalability, High Availability



Scalability:

- Vertical Scalability: increase size of instance  (scale up / down)
- Horizontal Scalability: increase number of instance

High Availability: Run instances for the same app across multi AZ



### ELB

ELB: managed load balancer

Three kinds of load balancers in AWS:

- Application load balancer(HTTP/HTTPS only) - layer 7 (ALB)
- Network load balancer(ultra-high performance - allows for tcp) - layer4
- classic load balancer



#### ASG

ASG: Auto Scaling Groups

Scaling Strategies:

- Manual Scaling: update the size of ASG manually
- Dynamic Scaling: 
  - Simple /Step Scaling:
    - When a cloudwatch alarm is triggered(CPU > 70%), add 2 units to capacity
    - When a cloudwatch alarm is triggered(CPU < 30%), remove 1
  - Target tracking Scaling
  - Scheduled Scaling
  - Predictive Scaling



## S3

#### Overview

#### S3 uses cases

- backup and storage
- disaster recovery
- Archive
- Hybrid Cloud Storage
- Application hosting
- Media hosting
- Data lakes & big data analytics
- Software delivery
- Static website



### Security

- User-based
  - IAM Policies
- Resource-based
  - Bucket Policies - Bucket wide rules from the S3 console - allows cross account
  - Object Access Control List(ACL)
  - Bucket Access Control List(ACL)



### S3 Bucket Policies

![image-20221108153947795](/Users/sky/Library/Application Support/typora-user-images/image-20221108153947795.png)



#### S3 Bucket Version
