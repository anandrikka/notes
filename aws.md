# AWS

I'm preparing for the AWS Associate Architect Certification, these notes will serve me as a quick lookup for all the topics

AWS (Amazon Web Services) is an IaaS (Infrastructure as a Service) company that offers an infrastructure for building applications.

AWS is divided into regions, ex: `us-east-1`, `eu-west-1` and each region will have multiple availability zones (at least 2 to 3). These availability zones are marked as `a, b, c, etc...`

Certification touches on a lot of topics, but they can be categorized into the following sections

* Compute
  * EC2
  * Lambda
  * Elastic Beanstalk
* Storage
  * S3
  * EBS (Elastic Block Storage)
  * EFS (Files)
  * FSx
  * Storage Gateway
* Databases
  * RDS
  * DynamoDB
  * Redshift
* Networking
  * VPC
  * Direct Connect
  * Route 53
  * API Gateway
  * AWS Global Accelerator

### Identity Access Management (IAM)

IAM is global, not regional level.

Permission -> Permission to access a resource (like read, write, etc...)

Policy -> A Policy is a JSON document with a list of permissions. There are 2 types inline policies (Added directly to user/group, not recommended) & managed policies.

Group -> A collection of users to which we can apply policies to

Role -> It's a combination of policies + Trust (Someone has to assume the role to get the permissions)
