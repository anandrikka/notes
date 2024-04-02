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

> KMS encryption is restricted for upload/download requests and they are region-specific. They allow anywhere from 5500 - 30000 req/sec and also we cannot increase the quota.

#### Replication

* Versioning must be enabled
* Objects in the existing bucket are not automatically replicated, we have to choose.
* Deleted markers are not automatically replicated, we have to choose.

### Elastic Compute Cloud (EC2)

EC2 is a virtual machine with processor, RAM, n/w card, and SSD. There are 4 types of EC2 instances available for purchase

* On-Demand
  * Default EC2 Instance type
  * Charged by sec/hr
  * It can be spun anytime
  * Usually priced higher than reserved and spot
* Reserved
  * Can get up to 72% off and can be served for 3-years
* Spot
  * Unused resources for sale.
  * You can get as high as 90% off.
  * We set a maximum price for the instance, if there is an instance available below that price it is provisioned for use.
  * We should use these instances for stateless, fault-tolerant, and flexible applications.
  * We can put in 4 types of requests
    * One time
    * Persistent
    * valid from
    * valid till
  * Spot fleets
    * It's a smart way of assigning instances, basically a combination of spot + on-demand instances to meet the capacity needs.
    * Capacity Optimized
    * Diversified
    * Lowest price
* Dedicated
  * There is no multi-tenancy, the hardware is dedicated.
  * The most costliest option.
  * Usually used for licensing and compliance.

#### Networking Options

* ENI (Elastic Network Interface)
  * Default n/w adapter and should suffice for the most number of use cases
* EN (Enhanced Networking)
  * It's the fastest option if we need a lot of I/O networking speeds
  * 10 Gbps - 100 Gbps
  * There are 2 types
    * ENA (Elastic Network Adapter) - More advanced and always prefer this n/w.
    * VF (Virtual Interface) - It's legacy
* EFA (Elastic Fabric Adapter)
  * High-performance n/w speeds
  * Bypasses the kernel layer
  * Usually used for machine learning

#### Placement Groups

* Cluster
  * Within same AZ
  * Needed if we require low latency b/w machines
* Spread
  * Completely different hardware
  * Used for fault tolerance and disaster recovery. Eg: RDS
* Partition
  * Different racks, independent n/w, and power
  * Used in big data applications like Hadoop, Cassandra, etc...

#### AWS Outposts

AWS directly to our data center.

Hybrid cloud and fully managed infrastructure within the data center.

There are 2 types

* Outpost Racks (Larger spaces)
* Outpost Servers(Small Spaces)

### Elastic Block Storage (EBS)

* Virtual hard disk, highly available, and highly scalable
* SSD, GP2 (General Purpose 2), GP3 (General Purpose 3), HDD
* Can take Snapshots
  * Exists in S3
  * They are taken point in time
  * Incremental, save only diff
* Encryption
  * Using AWS KMS, Customer keys
  * Encrypted at rest, while in transit
  * How to convert unencrypted EBS to encrypted
    * Take a snapshot.
    * Create a copy of the snapshot with encryption on.
    * Create an AMI image from the encrypted snapshot.
    * Use the AMI to launch a new encrypted instance.

#### Elastic File Storage (EFS)

* Managed NFS that can be shared across multiple instances
* Standard & In-frequent access tiers
* Only for Linux machines, and are not supported for Windows.

FSx( File System for Windows), FSx for luster (High-performance computing)

### Relational Database Services (RDS)

#### Multi-AZ

* An exact copy for disaster recovery
* RDS will automatically failover without any changes to the DNS

#### Read Replicas

* Cannot be used for disaster recovery, they can be in completely different AZ, or regions
* Can be promoted as databases, stop replication
* For increased performance

#### Aurora

* MYSQL & Postgres compatible AWS alternatives (proprietary)
* Better performance.
* Min 10GB with a max 128TB.
* Compute can scale to 96 vCPU & 768 GB RAM.
* 2 copies of data in an AZ and a minimum of 3 AZs which gives us 6 copies of data.
* Can handle loss of up to 2 copies of data without affecting the write availability and up to 3 copies for read availability.
* Self-healing (repairs are made automatically).
* It can have up to 15 replicas.
* Automated backups (Enabled by default).
* You can share snapshots with other accounts.
* Serverless

#### Dynamo DB

* No SQL DB (proprietary).
* Single millisecond latency.
* Stored on SSD.
* Spread across 3 geographically distinct data centers.
* Eventual read consistency (by default), Strong consistency ( compromise the latency)
* DynamoDB Accelerator (DAX)
  * in-memory cache.
  * microsecond latency.
  * No need to maintain a cache.
* Pay per request.
* Encrypt at rest (KMS)
* ACID (Atomic Consistency Isolated Durable)
  * 1 or more tables within a single AWS account and region.
  * 25 items or 4 MB per transaction.
* Streams
  * Time ordered seq of item-level changes in a table
  * Stored for 24 hours
  * Inserts, updates, and deletes (similar to bin logs)
  * Combine with lambda functions for similar processing to triggers in SQL databases.
* Global Tables
  * Multi-Master and Multi-Region replication
  * Globally distributed
  * Based on Streams
  * Multi-region redundancy for DR
  * No application rewrites
  * Replication latency under 1 sec

MongoDB to AWS using DocumentDB

#### Apache Cassandra

* Distributed database (Big data solutions)
* Amazon Keyspaces (Amazon-managed Cassandra database)
  * Serverless

#### Graph databases

Data is stored as graph nodes and relationships

Neptune (AWS-managed graph databases)

#### Amazon Quantum Ledger Database (QLDB)

No SQL database, immutable, transparent, and crypto-verifiable transaction logs

* Cryptocurrencies
* Shipping
* Financial transactions

#### Amazon Timestream

Time Series Data (Logged over a series of times, allowing tracking data)

Examples:

* IoT
* Weather
* Analytics

### Virtual Private Cloud (VPC)

Virtual Private Cloud is an isolated n/w within AWS for our account. Anything going in and out of this n/w can be controlled by us.

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* A subnet can only be created within one AZ. A subnet cannot be spanned across multiple AZs.
* A subnet mask should be b/w `/16 and /28`
* When a VPC is created a default Security group, NACL, and Route table are created by default
* 5 IP addresses are not available in each CIDR block
  * n/w address
  * 3 addresses are reserved by AWS for the router, future use, etc...
  * broadcast address
* Route tables
  * Allow to communicate with other resources within the subnet
  * The route tables are associated with subnets.
  * Route tables can attached to targets which could be IGW, NAT Gateway for internet access, or other kinds of gateways or n/w's.
* NAT gateway is a way to access the internet from private subnets, instead of connecting directly to the internet (which has 2-way communication),  A NAT gateway is created in the public subnet, and all the traffic from this route table is redirected to the NAT gateway for internet access, it's only one-way communication (meaning only our hosts can initiate a request, not anyone from the internet)

#### Security Groups

They are the last line of defense to protect resources. Security groups allow us to restrict the type of access to the resource and by whom. For example, we can only allow certain ports and sources to reach our resources.

* Security groups are stateful, meaning if the request is allowed then the response flows out, irrespective of outbound rules. For example, if we allow traffic on port 80, the response for the request on port 80 flows out even if we have an outbound rule to block the traffic.
* Only allow rules can be configured, no deny rules.
* There are Inbound rules and Outbound rules.
  * Inbound rules manage traffic coming in
  * Outbound rules manage traffic going out to n/w or the internet
* Each rule contains type, protocol, port range, and destination

#### Network Access Control List (NACL)

* They are stateless, meaning inbound rules and outbound rules work independently.
* They contain deny rules as well.
* This is the best place to block IP addresses
* Similar to the security group it has both inbound and outbound rules
* Allow rules to be prioritized with a rule number, rules are applied in the ascending order
* Order of rules matters, if you want to deny access to something, it should be placed before any allowed rule.
* Similar to security groups we have type, protocol, port range, and source.
* Subnets can be associated with NACL
* NACL can contain multiple subnets, whereas each subnet can only be associated with one NACL.

VPC Endpoints connect AWS services without leaving the AWS backbone n/w

VPC Peering allows us to connect 2 VPCs the resources in the VPCs act as if they belong to the same n/w.

VPC peering done at scale is called "AWS Private Link"

Direct Connect is a dedicated n/w from on-premise to AWS

### Route 53

DNS service offered by AWS.

There are different routing techniques offered by Route 53

* Simple Routing: A single A record can have multiple IPs associated, they are randomly accessed
* Weighted Routing: It splits the traffic based on weights to different resources, if one of the weighted resources fails, the traffic is automatically routed to another resource.
* Failover Routing: A backup routing, in case the primary fails
* Geolocation Routing: Based on the location from which the DNS queries originated we can redirect traffic accordingly
* Geoproximity Routing: Combination of multiple routing techniques like geolocation, proximity, availability, etc...
* Latency Routing: Traffic is routed based on the lowest latency to response.
* Multivalue Answer Routing: It is exactly similar to simple routing, but the only difference is if one of the resources is not reachable, traffic is not directed to it unless it becomes healthy.

### Elastic Load Balancer (ELB)

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

As the name suggests, it is used to keep multiple resources behind a single listener

There are multiple types of load balancers

* Application Load Balancer (ALB)
  * It is used for routing traffic at layer 7 (only HTTP/HTTPS traffic).
  * They have listeners with rules and target groups with resources in it.
  * Each listener can have multiple target groups and each target group can have multiple resources in it.
  * Enables path routing
* Network Load Balancer (NLB)
  * It is used at layer 4 (TCP)
  * Used for performance, low latency, and to handle millions of requests.
* Classic Load Balancer
  * Very old and legacy
  * Sticky Sessions

Deregistration delay is nothing but it allows inflight requests to be completed before they before unhealthy.

### Cloudwatch

Cloudwatch is an AWS service for monitoring and observability.

System metrics are used for tracking metrics related to the system like CPU, Memory, Storage, etc...By default, AWS offers certain metrics like CPU utilization, and n/w throughput, for custom metrics we need to add a cloud watch agent to resource those metrics to cloud watch

Application Monitoring is done through cloud watch logs. Logs have 3 important details

Log Event: The message to be logged

Log Stream: It is a continuous stream of Log events

Log Group: A collection of log streams

AWS Managed Grafana: Graphical metrics

AWS Managed Prometheus: AWS managed open-source docker container logs
