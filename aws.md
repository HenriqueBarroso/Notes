
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
* **Data Transfer OUT**: DData transfer IN is free (yay)

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
