
# AWS Notes

## What is cloud computing

Three types of cloud, private, public and hybrid.

### Type of cloud computing

* **IaaS** (Infrastructure as a service)
	* Provide building blocks for cloud IT
	* Provides networking, computers, data storage page
	* Highest level of flexibility
	* ex: Amazon EC2, GCP, Azure, Digital Ocean


* **PaaS** (Platform as a service)
	* Removes the need to manage underlying infrasctructure
	* Focus on deployment and management of applications
	* ex: Elastic Beanstalk (AWS), Heroku, Google App Engine (GCP), Windows Azure
	

* **SaaS** (Software as a Service)
	* Completed product that is run and managed by the service provider
	* ex: AWS services like Machine Leearning, Gmails, Dropbox, etc..

#### Price of cloud

AWS has 3 princing fundamentals:

* **Compute**: Pay for compute time
* **Storage**: Pay for data stored in the cloud
* **Data Transfer OUT**: Data transfer IN is free (yay)

### AWS Cloud Overview

#### AWS Regions

* AWS has regions all around the world, names be us-east-1, eu-west-3.
* A region is a **cluster of data centers**

#### AWS Availability Zones

Each region has **many** availability zones (usually 3, min is 2, max is 6)

### AWS Point of Presence

* Amazon has 216 points of presence
* Content is delivered to end users with lower latency

## IAM - Identity and Access Management

* IAM is a global service. that manages users and groups.
 * When creating an AWS account IAM by default creates a **root** account, **that shouldn't be used or shared**
* Users are people in our organization and can be grouped
* Groups can only contain users not other groups
* Users can be part of **different groups**
* Users don't need to be part of any group
* Users or groups can be assigned  JSON documents called **policies**

### IAM MFA Overview

#### Password policy

In AWS we can setup a password policy, that sets a minimum length, require specific character types, etc..

#### MFA - Multi Factor Authentication

This is a **VERY** recommended security measure that should be implemented  specially for root accounts and IAM low level accounts.
	
Which devices are supported:
* Virtual MFA device
	* Google Authenticator (Phone only)
	* Authy (Multi-device)
* Universal 2nd factor (U2F) Security key
	* Yubikey
* Hardware Key Fob MFA Device


#### How to access AWS

* AWS Management console (protecteed by password + MFA)
* AWS CLI: Protected by access
* AWS Software Developer KIT for code: protected by access

Access keys are secret, like a password, don't share.

#### IAM Roles

IAM Roles are used by AWS services to perfom actions on our behalf. They are similar to user account by are not meant to be used by physical people.

#### IAM Guidelines & Best Practices

* Don't use the root account except for AWS account setup
* One Physical users = One AWS user
* Assign users to groups and assign permissions to groups
* Create a strong password policy
* Use and enforce the sue of Multi Factor Authentication (MFA)
* Create and use Roles for giving permissions to AWS services
* Use Access Keys for programmatic Access (CLI/SDK)
* Audit permissions of your account with Credentials Report
* NEVER SHARE IAM users & Access Keys


## EC2 - Elastic Compute Cloud

EC2 = Infrastructure as a Service

It mainly consists in the capability of:
* Renting virtual machines EC2
* Storing data on virtual drives (EBS)
* Distributing load across machines (ELB)
* Scaling services with auto-scaling group (ASG)

### Instances

* AMIs a template that contains all software configuration.
* When you restart an instance, the public ip will also probably change

### Security Groups (like firewall)

* Security groups control how the traffic is allowed into or out of our EC2 instances
* Security groups only contain**allow** rules
* Security groups rules can be reference by IP or by security group

### EC2 Instance Roles

When adding IAM Roles to an instance, it ALLOWS that instance to perform actions as it they where from an authorized user.

* Never use `aws` credentials inside an instance . Reason being is that anyone who can access that instance can view the current installed credentials.

### EC2 Instances Purchasing Options

#### On-demand instances
* For short workload predictable pricing
* Pay for what you use
* No long term commitment
* Recommended for short-term and un-interrupted workloads, where we can't predict how the application will behave


#### Reserved (MININIUM ON YEAR)
* Reserved instances: long work loads
* Convertible reserved instances: long workloads with flexibles instances
* Schedule reserved instances: eg: Every Monday between 3 and 8pm
* Up to 75% discount compared to on-demand
* Reservation period: 1 year = + discount | 3 years = +++ discount
* Purchasing options: no upfront | partial upfront = + | All upfront = ++ discount
* Recommended for steady-state usage applications (eg: DB)

#### Spot instances: short workloads
* cheap, can lose instances (less reliable) almost 90% discount
* Instances that you can "lose" at any point of time if our max price is less than the current spot price
* Useful for:
	* Batch jobs
	* Data analysis
	* Image processing
	* Any distributed workloads
	* Workloads with flexible start and end time
* NOT good for:
	* Critical jobs or databases

#### Dedicated Hosts: book an entire physical server
* Helps with compliance requirements
* Reduce costs by allowing you to use your existing server-bound software licenses
* Requires at least 3 year period reservation
* More expensive


#### EC2 Dedicated instances
* Instances running on hardware that's dedicated to you
* May share hardware with other instance in the same account
* No control over instance placement (can move hardware after stop/start)

## ELB & ASG (Elastic LB & Auto Scaling Groups)

There are to kinds of scalability:
 * Vertical Scalability
 * Horizontal scalability (=elasticity)

#### Scalability VS Elasticity (vs Agility)

* Scalability: ability to accommodate a larger load by making the hardware stronger (scale up) or by adding nodes (scale out)

* Elasticity: Once a system is scalable, elasticity means that there will be some "auto-scaling" so that the system can scale based on the load. This is "cloud-friendly": pay-per-use, match demand, optimize costs.

* Agility: Ability to deploy new resources a click away.

## ELB (Elastic Load Balancing)

AWS offers 3 kinds of loads balancers:
* Application Load Balancer (HTTP/ HTTPS only) - Layer 7
* Network Load Balancer (ultra-high performance, allows for TCP) - Layer 4
* Classic Load Balancer (Slow retiring) - Layer 4 & 7

### Auto Scaling Groups (ASG)

* It's a system that can scale out or scale in servers quickly depending on the load.
* The goal of ASG is:
	* Scale out (add ec2 instances) to match an increased load
	* Scale in (remove ec2 instances) to match a decreased load
	* Ensure we have a minimum and a maximum number of machines running
	* Automatically register new instances to a load balancer
	* Replace unhealthy instances

