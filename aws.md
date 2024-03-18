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

### Simple Storage Service (S3)

S3 is one of the oldest services offered by AWS. S3 is used for storing objects like images, videos, files, etc... It can also be used for hosting static websites, but it can't be used for dynamic websites that need database connections.

By default, S3 is private. However, we can just policies either at the bucket level or at the object level (ACLs - Access Control List) to make them public.

* Unlimited storage
* Max object size (5 TB)
* Universal Namespace (Global, not per region)
* Versioning (Multiple versions of the object)

There are different tier offerings for S3. S3 is 99.9% available and 99.9 (11 9's) durable. So it's almost impossible to lose an object once uploaded into S3. It's stored across at least 2-3 AZ for redundancy.

<table><thead><tr><th width="288">Tier</th><th>Description</th></tr></thead><tbody><tr><td>S3 Standard</td><td>This is the default Tier.</td></tr><tr><td>S3 Standard - Infrequent Access</td><td>Similar to S3 standard, but for rapid access</td></tr><tr><td>S3 Standard - Infrequent Access One Zone</td><td>The data is saved in only a single zone, and it's cheaper than S3 - IA</td></tr><tr><td>S3 Intelligent Tier</td><td>Intelligently switches the tier to save the maximum on S3 tiering, there is a fee charged for every 1000 objects managed.</td></tr><tr><td>S3 Glacier</td><td><ul><li>Instant Retrieval</li><li>Flexible Retrieval (a couple of minutes to 12 minutes)</li><li>Deep Archive (12hr - 48hr)</li></ul></td></tr></tbody></table>

S3 Object lock and Glacier Vault lock blocks deleting the objects. It's mostly required for Governance Mode (Restricted access to delete objects) & Compliance mode (Not even a root account can delete objects). Versioning is enabled for object locks. WORM (Write Once Read Many)

#### Encryption

* Encryption in transit (HTTPS)
* Encryption at rest (SSE - Server Side Encryption)
  * SSE - S3 (AWS managed keys)
  * SSE - KMS (Key Management Services)
  * SSE - Customer (Customer manages the keys)

SSE - S3 is the default encryption and every bucket in s3 is by default encrypted. In a put request, we could add `x-amz-server-side-encryption` a header to encrypt objects at rest, accepted values are `AES256 & aws:kms` . We can also enforce encryption using bucket policies.

#### Performance

We can increase the performance of the S3 bucket by having multiple prefixes (folders). The more layered and structured our bucket is, it's more performant. So by default AWS allows

`3500` upload & `5500` download `request/sec/prefix` so we could achieve a lot of throughput if we have a lot of prefixes in our bucket.

KMS encryption is restricted for upload/download requests and they are region-specific. They allow anywhere from 5500 - 30000 req/sec and also we cannot increase the quota.

#### Replication

* Versioning must be enabled
* Objects in the existing bucket are not automatically replicated, we have to choose.
* Deleted markers are not automatically replicated, we have to choose.

### Elastic Compute Cloud (EC2)

