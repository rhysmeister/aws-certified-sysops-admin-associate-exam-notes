# aws-certified-sysops-admin-associate-exam-notes

## Domain 1: Monitoring, Logging, and Remediation
### 1.1 Implement metrics, alarms, and filters by using AWS monitoring and logging services

To assist with the knowledge items in this section we will create 3 Amazon Linux 2 Instances in the default VPC with a new ssh key. Methods for doing this will be given with the Web Console and the AWS Command-Line (CF dropped)

* domain_1/1_ec2_web_console.md
* domain_1/2_ec2_awc_cli.md

* Identify, collect, analyze, and export logs (for example, Amazon CloudWatch Logs, CloudWatch
Logs Insights, AWS CloudTrail logs)
  * Logs Groups should be visible in https://eu-central-1.console.aws.amazon.com/cloudwatch/home?region=eu-central-1#logsV2:log-groups
* Collect metrics and logs using the CloudWatch agent
* Create CloudWatch alarms

* Create metric filters
  * [Web Console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CreateMetricFilterProcedure.html)
* Create CloudWatch dashboards
  * Web Console
    * Go to the CloudWatch Console
    * Dashboards > Create Dashboard
    * Enter Dashboard Name, i.e. RhysTest
    * Add Widget, we'll keep it simple, pick "Bar".
  * AWS CLI
    * `aws cloudwatch put-dashboard --dashboard-name RhysTest --dashboard-body TODO`
* Configure notifications (for example, Amazon Simple Notification Service [Amazon SNS],
Service Quotas, CloudWatch alarms, AWS Health events)
### 1.2 Remediate issues based on monitoring and availability metrics
* Troubleshoot or take corrective actions based on notifications and alarms
* Configure Amazon EventBridge rules to trigger actions
* Use AWS Systems Manager Automation documents to take action based on AWS Config rules
## Domain 2: Reliability and Business Continuity
### 2.1 Implement scalability and elasticity
* Create and maintain AWS Auto Scaling plans
  * [AWS Auto Scaling](https://aws.amazon.com/autoscaling/)
* Implement caching
  * [AWS Elasticache](https://aws.amazon.com/elasticache/)
* Implement Amazon RDS replicas and Amazon Aurora Replicas
  * [Amazon RDS Read Replicas](https://aws.amazon.com/rds/features/read-replicas/)
  * Read Replicas Maximums
    * MySQL, MariaDB and PostgreSQL - 15
    * Oracle & SQL Server - 5.
  * [Aurora Replication](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html)
* Implement loosely coupled architectures
  * Loosely coupled architecture is an architectural style where the individual components of an application are built independently from one another (the opposite paradigm of tightly coupled architectures). Each component, sometimes referred to as a microservice, is built to perform a specific function in a way that can be used by any number of other services. This pattern is generally slower to implement than tightly coupled architecture but has a number of benefits, particularly as applications scale. [source](https://glossary.cncf.io/loosely-coupled-architecture/)
  * [Build a loosely coupled architecture with microservices using DevOps practices and AWS Cloud9](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/build-a-loosely-coupled-architecture-with-microservices-using-devops-practices-and-aws-cloud9.html)
  * [Loosely Coupled Scenarios](https://docs.aws.amazon.com/wellarchitected/latest/high-performance-computing-lens/loosely-coupled-scenarios.html)
* Differentiate between horizontal scaling and vertical scaling
  * [Difference between horizontally scaling and vertically scaling](https://medium.com/@khushalbisht/aws-scaling-horizontally-vs-vertically-3e30e3e71118)
  * [RDS Instance Horizontal and Vertical Scaling](https://aws.amazon.com/blogs/database/scaling-your-amazon-rds-instance-vertically-and-horizontally/)
### 2.2 Implement high availability and resilient environments
* Configure Elastic Load Balancer and Amazon Route 53 health checks
  * [Elastic Load Balancer Checks](https://aws.amazon.com/blogs/aws/elastic-load-balancer-health-checks/)
  * [Add Elastic Load Balancing health checks to an Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-add-elb-healthcheck.html)
  * [How Amazon Route 53 checks the health of your resources](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome-health-checks.html)
  * [Types of Amazon Route 53 health checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/health-checks-types.html)
* Differentiate between the use of a single Availability Zone and Multi-AZ deployments (for
example, Amazon EC2 Auto Scaling groups, Elastic Load Balancing, Amazon FSx, Amazon RDS)
* Implement fault-tolerant workloads (for example, Amazon Elastic File System [Amazon EFS],
Elastic IP addresses)
* Implement Route 53 routing policies (for example, failover, weighted, latency based)
### 2.3 Implement backup and restore strategies
* Automate snapshots and backups based on use cases (for example, RDS snapshots, AWS
Backup, RTO and RPO, Amazon Data Lifecycle Manager, retention policy)
  * [RDS Backup & Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html)
  * [AWS Backup](https://aws.amazon.com/backup/)
  * [RTO & RPO](https://aws.amazon.com/blogs/mt/establishing-rpo-and-rto-targets-for-cloud-applications/)
  * [EBS Snapshots](https://aws.amazon.com/ebs/snapshots/)
  * [Amazon Data Lifecycle Manager](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/snapshot-lifecycle.html)
* Restore databases (for example, point-in-time restore, promote read replica)
  * [Restore a DB Snapshot](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html)
  * [PITR](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIT.html)
  * [Promote a read replica](https://fitdevops.in/how-to-promote-read-replica-to-standalone-db-instance/)
* Implement versioning and lifecycle rules
* Configure Amazon S3 Cross-Region Replication
* Execute disaster recovery procedures
## Domain 3: Deployment, Provisioning, and Automation
### 3.1 Provision and maintain cloud resources
* Create and manage AMIs (for example, EC2 Image Builder)
* Create, manage, and troubleshoot AWS CloudFormation
* Provision resources across multiple AWS Regions and accounts (for example, AWS Resource
Access Manager, CloudFormation StackSets, IAM cross-account roles)
* Select deployment scenarios and services (for example, blue/green, rolling, canary)
* Identify and remediate deployment issues (for example, service quotas, subnet sizing,
* CloudFormation and AWS OpsWorks errors, permissions)
### 3.2 Automate manual or repeatable processes
* Use AWS services (for example, OpsWorks, Systems Manager, CloudFormation) to automate
deployment processes
* Implement automated patch management
* Schedule automated tasks by using AWS services (for example, EventBridge, AWS Config)
## Domain 4: Security and Compliance
### 4.1 Implement and manage security and compliance policies
* Implement IAM features (for example, password policies, MFA, roles, SAML, federated identity,
resource policies, policy conditions)
* Troubleshoot and audit access issues by using AWS services (for example, CloudTrail, IAM
Access Analyzer, IAM policy simulator)
* Validate service control policies and permissions boundaries
* Review AWS Trusted Advisor security checks
* Validate AWS Region and service selections based on compliance requirements
* Implement secure multi-account strategies (for example, AWS Control Tower, AWS
Organizations)
### 4.2 Implement data and infrastructure protection strategies
* Enforce a data classification scheme
* Create, manage, and protect encryption keys
* Implement encryption at rest (for example, AWS Key Management Service [AWS KMS])
* Implement encryption in transit (for example, AWS Certificate Manager, VPN)
* Securely store secrets by using AWS services (for example, AWS Secrets Manager, Systems
Manager Parameter Store)
* Review reports or findings (for example, AWS Security Hub, Amazon GuardDuty, AWS Config,
Amazon Inspector) 
## Domain 5: Networking and Content Delivery
### 5.1 Implement networking features and connectivity
* Configure a VPC (for example, subnets, route tables, network ACLs, security groups, NAT
gateway, internet gateway)
* Configure private connectivity (for example, Systems Manager Session Manager, VPC
endpoints, VPC peering, VPN)
* Configure AWS network protection services (for example, AWS WAF, AWS Shield)
### Configure domains, DNS services, and content delivery
* Configure Route 53 hosted zones and records
* Implement Route 53 routing policies (for example, geolocation, geoproximity)
* Configure DNS (for example, Route 53 Resolver)
* Configure Amazon CloudFront and S3 origin access identity (OAI)
* Configure S3 static website hosting
### Troubleshoot network connectivity issues
* Interpret VPC configurations (for example, subnets, route tables, network ACLs, security
groups)
* Collect and interpret logs (for example, VPC Flow Logs, Elastic Load Balancer access logs, AWS
WAF web ACL logs, CloudFront logs)
* Identify and remediate CloudFront caching issues
* Troubleshoot hybrid and private connectivity issues
## Domain 6: Cost and Performance Optimization
### 6.1 Implement cost optimization strategies
* Implement cost allocation tags
* Identify and remediate underutilized or unused resources by using AWS services and tools (for
example, Trusted Advisor, AWS Compute Optimizer, Cost Explorer)
* Configure AWS Budgets and billing alarms
* Assess resource usage patterns to qualify workloads for EC2 Spot Instances
* Identify opportunities to use managed services (for example, Amazon RDS, AWS Fargate, EFS)
### 6.2 Implement performance optimization strategies
* Recommend compute resources based on performance metrics
* Monitor Amazon EBS metrics and modify configuration to increase performance efficiency
* Implement S3 performance features (for example, S3 Transfer Acceleration, multipart uploads)
* Monitor RDS metrics and modify the configuration to increase performance efficiency (for
example, Performance Insights, RDS Proxy)
* Enable enhanced EC2 capabilities (for example, enhanced network adapter, instance store,
placement groups)