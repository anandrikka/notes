# 🍕 Cheat Sheet

### EC2 Auto scaling groups

#### Scaling Policies

* Simple scaling policies
  * The drawback of simple scaling policies is that new scaling changes have to wait till the current changes are completely applied
* Step scaling or tracking policy
  * The changes can be applied immediately, keeping the cost down
* Scheduled scaling is good for predictable workloads
* Predictive scaling assuming the EC2 instances are homogeneous

<div align="left">

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>EC2 Autoscaling Group: Default Termination Policy </p></figcaption></figure>

</div>



### S3

* Object locking requires versioning and can be done using
  * Governance mode - Users will still have permissions to delete based on role
  * Compliance mode - Even the root user cannot delete the files
* Legal hold - Can be placed by anyone who has permission to put the legal hold, irrespective of what kind of mode is used, the file cannot be deleted unless the legal hold is removed by the user
* Standard -> Standard - Infrequent Access (IA) -> Intelligent Tiering -> Glacier -> Glacier Deep Archive

### Cloud watch

* The default metrics available are
  * CPU utilization
  * Network utilization
  * Disk performance
  * Disk Read/Writes

### Cloudfront

* Lambda Edge to process the requests & responses to and from origin servers.
* Failover origin groups to respond with different error codes.

### Lambda

* Use encryption helpers with custom KMS keys for environment variables to protect from those who have access to the lambda console.

### VPC

* Transit Gateway: connects VPC and on-premises networks through a central hub for navigation
* Direct Connect: Establish dedicated n/w from on-premises to AWS
* VPN Cloud Hub: Establish secure communication b/w remote sites example: Head office and other branches.
* VPC endpoint: Private IP address to communicate without leaving AWS.

### RDS

* CPU & MEM metrics are not readily available, instead, we should opt for enhanced monitoring
