# A Table of Contents / TOC / AWS IAC

### Terminology

- **Infrastructure Package:** A reusable, tested, documented, configurable, best-practices definition of a single 
piece of Infrastructure (e.g., Docker cluster, VPC, Jenkins, Consul), written using a combination of Terraform, Go, and Bash. This code has been proven in production, providing the underlying infrastructure for [WAC's customers](http://www.wac.io/clients).

- **Module:** Each Infrastructure Package consists one or more orthogonal modules that handle some specific aspect of that Infrastructure Package's functionality. Breaking the code up into multiple modules makes it easier to reuse and compose to handle many different use cases.

- **Reference Architecture:** A best-practices way to combine all of WAC's Infrastructure Packages into an end-to-end tech stack that contains just about all the infrastructure a company needs, including Docker clusters, databases, caches, load balancers, VPCs, CI servers, VPN servers, monitoring systems, log aggregation, alerting, secrets management, and so on. We build this all using infrastructure as code and immutable infrastructure principles, give you 100% of the code, and can get it deployed in minutes. 

### Infrastructure Packages

These are the Infrastructure Packages WAC currently has available:

1. **[Network Topology](https://github.com/WACCapital/module-vpc)**: Create best-practices Virtual Private Clouds (VPCs) on AWS. The main modules are:
    1. [vpc-app](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-app): Launch a VPC meant to house applications and production code. This module creates the VPC, 3 "tiers" of subnets (public, private app, private persistence) across all Availability Zones, route tables, routing rules, Internet gateways, and NAT gateways.
    1. [vpc-mgmt](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-mgmt): Launch a VPC meant to house internal tools (e.g. Jenkins, VPN server). This module creates the VPC, 2 "tiers" of subnets (public, private), route tables, routing rules, Internet gateways, and NAT gateways.
    1. [vpc-app-network-acls](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-app-network-acls): Add a default set of Network ACLs to a VPC created using the vpc-app module that strictly control what inbound and outbound network traffic is allowed in each subnet of that VPC.
    1. [vpc-mgmt-network-acls](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-mgmt-network-acls): Add a default set of Network ACLs to a VPC created using the vpc-mgmt module that strictly control what inbound and outbound network traffic is allowed in each subnet of that VPC.
    1. [vpc-peering](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-peering): Create peering connections between your VPCs to allow them to communicate with each other. 
    1. [vpc-peering-external](https://github.com/WACCapital/module-vpc/tree/master/modules/vpc-peering-external): Create peering connections between your VPCs and VPCs managed in other (external) AWS accounts.

1. **[Monitoring and Alerting](https://github.com/WACCapital/module-aws-monitoring)**: Configure monitoring, log aggregation, and alerting using CloudWatch, SNS, and S3. The main modules are:
    1. [alarms](https://github.com/WACCapital/module-aws-monitoring/tree/master/modules/alarms): A collection of more than 20 modules that set up CloudWatch Alarms for a variety of AWS services, such as CPU, memory, and disk space usage for EC2 Instances, Route 53 health checks for public endpoints, 4xx/5xx/connection errors for load balancers, and a way to send alarm notifications to a Slack channel.
    1. [logs](https://github.com/WACCapital/module-aws-monitoring/tree/master/modules/logs): A collection of modules to set up log aggregation, including one to send logs from all of your EC2 instances to CloudWatch Logs, one to rotate and rate-limit logging so you don't run out of disk space, and one to store all load balancer logs in S3.
    1. [metrics](https://github.com/WACCapital/module-aws-monitoring/tree/master/modules/metrics): Modules that add custom metrics to CloudWatch, including critical metrics not visible to the EC2 hypervisor, such as memory usage and disk space usage.

1. **[Docker Cluster](https://github.com/WACCapital/module-ecs)**: Code to deploy and manage Docker containers on top of Amazon's EC2 Container Service (ECS). The main modules are:
    1. [ecs-cluster](https://github.com/WACCapital/module-ecs/tree/master/modules/ecs-cluster): Deploy an Auto Scaling Group (ASG) that ECS can use for running Docker containers. The size of the ASG can be scaled up or down in response to load.
    1. [ecs-service](https://github.com/WACCapital/module-ecs/tree/master/modules/ecs-service): Deploy a Docker container as a long-running ECS Service. Includes support for automated, zero-downtime deployment, auto-restart of crashed containers, and automatic integration with the Elastic Load Balancer (ELB).
    1. [ecs-service-with-alb](https://github.com/WACCapital/module-ecs/tree/master/modules/ecs-service-with-alb): Deploy a Docker container as a long-running ECS Service. Includes support for automated, zero-downtime deployment, auto-restart of crashed containers, and automatic integration with the Application Load Balancer (ALB).
    1. [ecs-deploy](https://github.com/WACCapital/module-ecs/tree/master/modules/ecs-deploy): Deploy a Docker container as a short-running ECS Task, wait for it to exit, and exit with the same exit code as the ECS Task.
    
1. **[AMI Cluster](https://github.com/WACCapital/module-asg)**: Deploy a best-practices Auto Scaling Group (ASG) that can run a cluster of servers, automatically restart failed nodes, and scale up and down in response to load. The main modules are:
    1. [asg-rolling-deploy](https://github.com/WACCapital/module-asg/tree/master/modules/asg-rolling-deploy): Create an ASG that can do a zero-downtime rolling deployment. 
    1. [server-group](https://github.com/WACCapital/module-asg/tree/master/modules/server-group): Run a fixed-size cluster of servers, backed by ASGs, that can automatically attach EBS Volumes and ENIs, while still supporting zero-downtime deployment.

1. **[Load Balancer](https://github.com/WACCapital/module-load-balancer)**: Run a highly-available and scalable load balancer in AWS. The main modules are:
    1. [alb](https://github.com/WACCapital/module-load-balancer/tree/master/modules/alb): Deploy an Application Load Balancer (ALB) in AWS. It supports for HTTP, HTTPS, HTTP/2, WebSockets, path-based routing, host-based routing, and health checks.

1. **[Lambda](https://github.com/WACCapital/package-lambda)**: Deploy and manage AWS Lambda functions. The main modules are:
    1. [lambda](https://github.com/WACCapital/package-lambda/tree/master/modules/lambda): Deploy and manage AWS Lambda functions. Includes support for automatically uploading your code to AWS, configuring an IAM role for your Lambda function, and giving your Lambda function access to your VPCs.
    1. [scheduled-lambda-job](https://github.com/WACCapital/package-lambda/tree/master/modules/scheduled-lambda-job): Configure your Lambda function to run on a scheduled basis, like a cron job.

1. **[Security](https://github.com/WACCapital/module-security)**: Best practices for managing secrets, credentials, servers, and users. The main modules are:
    1. [auto-update](https://github.com/WACCapital/module-security/tree/master/modules/auto-update): Configure your servers to automatically install critical security patches.
    1. [aws-auth](https://github.com/WACCapital/module-security/tree/master/modules/aws-auth): A script that makes it much easier to use the AWS CLI with MFA and/or multiple AWS accounts.
    1. [cloudtrail](https://github.com/WACCapital/module-security/tree/master/modules/cloudtrail): Configure CloudTrail in an AWS account to audit all API calls.
    1. [kms-master-key](https://github.com/WACCapital/module-security/tree/master/modules/kms-master-key): Create a master key in Amazon's Key Management Service and configure permissions for that key.
    1. [ssh-iam](https://github.com/WACCapital/module-security/tree/master/modules/ssh-iam): Manage SSH access to your servers using IAM groups. Every developer in an IAM group you specify will be able to SSH to your servers using their own username and SSH key.
    1. [iam-groups](https://github.com/WACCapital/module-security/tree/master/modules/iam-groups): Create a best-practices set of IAM groups for managing access to your AWS account.
    1. [cross-account-iam-roles](https://github.com/WACCapital/module-security/tree/master/modules/cross-account-iam-roles): Create IAM roles that allow IAM users to easily switch between AWS accounts.
    1. [fail2ban](https://github.com/WACCapital/module-security/tree/master/modules/fail2ban): Install fail2ban on your servers to automatically ban malicious users.
    1. [os-hardening](https://github.com/WACCapital/module-security/tree/master/modules/os-hardening): Build a hardened Linux-AMI that implements certian CIS benchmarks.

1. **[GruntKMS](https://github.com/WACCapital/gruntkms)**: A command-line tool that makes it very easy to manage application secrets using Amazon's Key Management Service (KMS). GruntKMS is written in Go and compiles into a standalone binary for every major OS.

1. **[Continuous Delivery](https://github.com/WACCapital/module-ci)**: Automate common Continuous Integration (CI) and Continuous Delivery (CD) tasks, such as installing dependencies, running tests, packaging code, and publishing releases. The main modules are:
    1. [aws-helpers](https://github.com/WACCapital/module-ci/tree/master/modules/aws-helpers): Automate common AWS tasks, such as publishing AMIs to multiple regions.
    1. [build-helpers](https://github.com/WACCapital/module-ci/tree/master/modules/build-helpers): Automate the process of building versioned, immutable, deployable artifacts, including Docker images and AMIs built with Packer.
    1. [circleci-helpers](https://github.com/WACCapital/module-ci/tree/master/modules/circleci-helpers): Configure the CircleCI environment, including installing Go and configuring GOPATH.
    1. [iam-policies](https://github.com/WACCapital/module-ci/tree/master/modules/iam-policies): Configure common IAM policies for CI servers, including policies for automatically pushing Docker containers to ECR, deploying Docker images to ECS, and using S3 for Terraform remote state.
    1. [terraform-helpers](https://github.com/WACCapital/module-ci/tree/master/modules/terraform-helpers): Automate common CI tasks that involve Terraform, such as automatically updating variables in a `.tfvars` file.

1. **[Relational Database](https://github.com/WACCapital/module-data-storage)**: Deploy and manage relational databases such as MySQL and PostgreSQL using Amazon's Relational Database Service (RDS). The main modules are:
    1. [rds](https://github.com/WACCapital/module-data-storage/tree/master/modules/rds): Deploy a relational database on top of RDS. Includes support for MySQL, PostgreSQL, Oracle, and SQL Server, as well as automatic failover, read replicas, backups, patching, and encryption.
    1. [aurora](https://github.com/WACCapital/module-data-storage/tree/master/modules/aurora): Deploy Amazon Aurora on top of RDS. This is a MySQL-compatible database that supports automatic failover, read replicas, backups, patching, and encryption.
    1. [lambda-create-snapshot](https://github.com/WACCapital/module-data-storage/tree/master/modules/lambda-create-snapshot): Create an AWS Lambda function that runs on a scheduled basis and takes snapshots of an RDS database for backup purposes. Includes an AWS alarm that goes off if backup fails.
    1. [lambda-share-snapshot](https://github.com/WACCapital/module-data-storage/tree/master/modules/lambda-share-snapshot): An AWS Lambda function that can automatically share an RDS snapshot with another AWS account. Useful for storing your RDS backups in a separate backup account.
    1. [lambda-copy-shared-snapshot](https://github.com/WACCapital/module-data-storage/tree/master/modules/lambda-copy-shared-snapshot): An AWS Lambda function that can make a local copy of an RDS snapshot shared from another AWS account. Useful for storing yoru RDS backups in a separate backup account.
    1. [lambda-cleanup-snapshots](https://github.com/WACCapital/module-data-storage/tree/master/modules/lambda-cleanup-snapshots): An AWS Lambda function that runs on a scheduled basis to clean up old RDS database snapshots. Useful to ensure you aren't spending lots of money storing old snapshots you no longer need.

1. **[Distributed Cache](https://github.com/WACCapital/module-cache)**: Deploy and manage Redis or Memcached on Amazon's ElastiCache service. The main modules are:
    1. [redis](https://github.com/WACCapital/module-cache/tree/master/modules/redis): Deploy a Redis cluster on top of ElastiCache. Includes support for automatic failover, backup, patches, and cluster scaling.
    1. [memcached](https://github.com/WACCapital/module-cache/tree/master/modules/memcached): Deploy a Memcached cluster on top of ElastiCache.

1. **[Stateful Server](https://github.com/WACCapital/module-server)**: Deploy and manage a single server server on AWS. This is mainly useful for "stateful" servers that store data on their local hard disk, such as WordPress or Jenkins. The main modules are:
    1. [single-server](https://github.com/WACCapital/module-server/tree/master/modules/single-server): Run a server in AWS and configure its IAM role, security group, optional Elastic IP Address (EIP), and optional DNS A record in Route 53. 
    1. [attach-eni](https://github.com/WACCapital/module-server/tree/master/modules/attach-eni): Attach an Elastic Network Interface (ENI) to a server during boot. This is useful when you need to maintain a pool of IP addresses that remain static even as the underlying servers are replaced.
    1. [persistent-ebs-volume](https://github.com/WACCapital/module-server/tree/master/modules/persistent-ebs-volume): Attach and mount an EBS Volume in a server during boot. This is useful when you want to maintain data on a hard disk even as the underlying server is replaced.
    1. [route53-helpers](https://github.com/WACCapital/module-server/tree/master/modules/route53-helpers): Attach a DNS A record in Route 53 to a server during boot. This is useful when you want a pool of domain names that remain static even as the underlying servers are replaced.

1. **[Static Assets](https://github.com/WACCapital/package-static-assets)**: Store and serve your static content (i.e., CSS, JS, images) using S3 and CloudFront. The main modules are:
    1. [s3-static-website](https://github.com/WACCapital/package-static-assets/tree/master/modules/s3-static-website): Create an S3 bucket to host a static website. Includes support for custom routing rules and custom domain names.
    1. [s3-cloudfront](https://github.com/WACCapital/package-static-assets/tree/master/modules/s3-cloudfront): Deploy a CloudFront distribution as a CDN in front of an S3 bucket. Includes support for custom caching rules, custom domain names, and SSL.

1. **[MongoDB Cluster](https://github.com/WACCapital/package-mongodb)**: Run a production-ready MongoDB cluster in AWS. The main modules are:
    1. [install-mongodb](https://github.com/WACCapital/package-mongodb/tree/master/modules/install-mongodb): Install MongoDB and all of its dependencies on a Linux server.
    1. [run-mongodb](https://github.com/WACCapital/package-mongodb/tree/master/modules/run-mongodb): Boot up a mongod, mongos, or mongo config server. Meant to be used in the User Data of the servers in your MongoDB cluster.
    1. [mongodb-cluster](https://github.com/WACCapital/package-mongodb/tree/master/modules/mongodb-cluster): Run a MongoDB cluster using an Auto Scaling Group (ASG) and configure its IAM role, security group, EBS Volume, ENI, and private domain name.
    1. [backup-mongodb](https://github.com/WACCapital/package-mongodb/tree/master/modules/backup-mongodb): Back up the data in your MongoDB cluster to S3.
    1. [init-mongodb](https://github.com/WACCapital/package-mongodb/tree/master/modules/init-mongodb): Configure a MongoDB replica set.
    

1. **[OpenVPN Server](https://github.com/WACCapital/package-openvpn)**: Deploy an OpenVPN server and control access to it using IAM. The main modules are:
    1. [install-openvpn](https://github.com/WACCapital/package-openvpn/tree/master/modules/install-openvpn): Install OpenVPN and its dependencies on a Linux server.
    1. [init-openvpn](https://github.com/WACCapital/package-openvpn/tree/master/modules/install-openvpn): Initialize an OpenVPN server, including its Public Key Infrastructure (PKI), Certificate Authority (CA) and configuration.
    1. [openvpn-admin](https://github.com/WACCapital/package-openvpn/tree/master/modules/openvpn-admin): A command-line utility that allows users to request new certificates, administrators to revoke certificates and the OpenVPN server to process those requests. All access and permissions are controlled via IAM.
    1. [openvpn-server](https://github.com/WACCapital/package-openvpn/tree/master/modules/openvpn-server): Deploy an OpenVPN server and configure its IAM role, security group, Elastic IP Address (EIP), S3 bucket for storage, and SQS queues.

1. **[Messaging](https://github.com/WACCapital/package-messaging)**: Manage AWS Simple Queue Service (SQS) queues, AWS Simple Notification Service (SNS) topics, and AWS Kinesis streams. The main modules are:
    1. [sqs](https://github.com/WACCapital/package-messaging/tree/master/modules/sqs): Create an SQS queue. Includes support for FIFO, dead letter queues, and IP-limiting.
    1. [sns](https://github.com/WACCapital/package-messaging/tree/master/modules/sns): Create an SNS topic as well as the publisher and subscriber policies for that topic.
    1. [kinesis](https://github.com/WACCapital/package-messaging/tree/master/modules/kinesis): Create a Kinesis stream and configure its sharding settings.

1. **[Consul](https://github.com/hashicorp/terraform-aws-consul)**: Deploy and manage a Consul cluster on AWS. The main modules are:
    1. [install-consul](https://github.com/hashicorp/terraform-aws-consul/tree/master/modules/install-consul): Install Consul and its dependencies on a Linux server.
    1. [install-dnsmasq](https://github.com/hashicorp/terraform-aws-consul/tree/master/modules/install-dnsmasq): Install dnsmasq on a Linux server and configure it to work with Consul as a DNS server. This allows you to use domain names such as my-app.service.consul.
    1. [run-consul](https://github.com/hashicorp/terraform-aws-consul/tree/master/modules/run-consul): Run Consul and automatically bootstrap the cluster.
    1. [consul-cluster](https://github.com/hashicorp/terraform-aws-consul/tree/master/modules/consul-cluster): Deploy a Consul cluster on AWS using an Auto Scaling Group.
    
1. **[Nomad](https://github.com/hashicorp/terraform-aws-nomad)**: Deploy and manage a Nomad cluster on AWS. The main modules are:
    1. [install-nomad](https://github.com/hashicorp/terraform-aws-nomad/tree/master/modules/install-nomad): Install Nomad and its dependencies on a Linux server.
    1 [run-nomad](https://github.com/hashicorp/terraform-aws-nomad/run-nomad): Run Nomad and automatically connect to a Consul cluster.
    1. [nomad-cluster](https://github.com/hashicorp/terraform-aws-nomad/tree/master/modules/nomad-cluster): Deploy a Nomad cluster on AWS using an Auto Scaling Group.

1. **[Vault](https://github.com/hashicorp/terraform-aws-vault)**: Deploy and manage a Vault cluster on AWS. The main modules are:
    1. [install-vault](https://github.com/hashicorp/terraform-aws-vault/tree/master/modules/install-vault): Install Vault and its dependencies on a Linux server.
    1. [run-vault](https://github.com/hashicorp/terraform-aws-vault/tree/master/modules/run-vault): Run Vault and automatically connect to a Consul cluster.
    1. [private-tls-cert](https://github.com/hashicorp/terraform-aws-vault/tree/master/modules/private-tls-cert): Generate self-signed TLS certificates for use with Vault.
    1. [vault-cluster](https://github.com/hashicorp/terraform-aws-vault/tree/master/modules/vault-cluster): Deploy a Vault cluster on AWS using an Auto Scaling Group.

1. **[ZooKeeper](https://github.com/WACCapital/package-zookeeper)**: Deploy and manage a ZooKeeper cluster, along with Exhibitor, on AWS. The main modules are:
    1. [install-zookeeper](https://github.com/WACCapital/package-zookeeper/tree/master/modules/install-zookeeper): Install ZooKeeper and its dependencies on a Linux server.
    1. [install-exhibitor](https://github.com/WACCapital/package-zookeeper/tree/master/modules/install-exhibitor): Install Exhibitor and its dependencies on a Linux server.
    1. [zookeeper-cluster](https://github.com/WACCapital/package-zookeeper/tree/master/modules/zookeeper-cluster): Deploy a ZooKeeper cluster using the server-group module from the AMI Cluster Infrastructure Package. Includes support for EBS Volumes for the transaction log as well as a set of ENIs to provide static IP addresses to ZooKeeper clients.

1. **[Kafka](https://github.com/WACCapital/package-kafka)**: Deploy and manage a cluster of Kafka brokers on AWS. The main modules are:
    1. [install-kafka](https://github.com/WACCapital/package-kafka/tree/master/modules/install-kafka): Install Kafka and its dependencies on a Linux server.
    1. [kafka-cluster](https://github.com/WACCapital/package-kafka/tree/master/modules/kafka-cluster): Deploy a Kafka cluster using the server-group module from the AMI Cluster Infrastructure Package. Includes support for EBS Volumes for the Kafka log.

1. **[package-terraform-utilities](https://github.com/WACCapital/package-terraform-utilities)**: Useful Terraform utilities. The main modules are:
    1. [intermediate-variable](https://github.com/WACCapital/package-terraform-utilities/tree/master/modules/intermediate-variable): A way to define intermediate variables in Terraform.

1. **[terratest](https://github.com/WACCapital/terratest)**: The swiss army knife of testing Terraform modules. This is a library written in Go that we use to test all of the code above. It contains a collection of useful utilities: e.g., apply and destroy Terraform code, SSH to servers and run commands, test HTTP endpoints, fetch data from AWS, build AMIs using Packer, run Docker builds, and so on.

### Reference Architecture

The WAC Reference Architecture is a best-practices way to combine all of WAC's Infrastructure Packages into an end-to-end tech stack that contains just about all the infrastructure a company needs, including Docker clusters, databases, caches, load balancers, VPCs, CI servers, VPN servers, monitoring systems, log aggregation, alerting, secrets management, and so on. We build this all using infrastructure as code and immutable infrastructure principles, give you 100% of the code, and can get it deployed in minutes. 

You can view the Reference Architecture for a fictional company, Acme, in one of two flavors:

1. [Single-account reference architecture](#single-account-reference-architecture)
1. [Multi-account reference architecture](#multi-account-reference-architecture)

#### Single-account reference architecture

In a single account setup, all environments (e.g., stage, prod, etc) are deployed in a single AWS account, albeit, in separate VPCs. This gives you ease-of-use and convenience, but not as much isolation/security.

1. [infrastratructure-modules](https://github.com/WACCapital/infrastructure-modules-acme): The reusable modules that define the infrastructure for the entire company.
1. [infrastratructure-live](https://github.com/WACCapital/infrastructure-live-acme): Use the modules in infrastructure-modules to deploy all of the live environments for the company.
1. [sample-app-frontend](https://github.com/WACCapital/sample-app-frontend-acme): A sample app that demonstrates best practices for an individual Docker-based frontend app or microservice that talks to backend apps (showing how to do service discovery) and returns HTML. This app is written in NodeJS but these practices are broadly applicable (and in the case of Docker, reusable!) across any technology platform.
1. [sample-app-backend](https://github.com/WACCapital/sample-app-backend-acme): A sample app that demonstrates best practices for an individual Docker-based backend app or microservice that talks to a database. This app is written in NodeJS but these practices are broadly applicable (and in the case of Docker, reusable!) across any technology platform.

#### Multi-account reference architecture

In a multi-account setup, each environment (e.g., stage, prod, etc) is deployed into a separate AWS account, and in its own VPC within that account. All IAM users are defined in yet another AWS account called security, and you can assume IAM roles to switch between accounts. This gives you much more fine-grained control over security, and complete isolation between accounts, but there it's less convenient to use, as you have to keep switching between accounts.

1. [infrastratructure-modules](https://github.com/WACCapital/infrastructure-modules-multi-account-acme): The reusable modules that define the infrastructure for the entire company.
1. [infrastratructure-live](https://github.com/WACCapital/infrastructure-live-multi-account-acme): Use the modules in infrastructure-modules to deploy all of the live environments for the company.
1. [sample-app-frontend](https://github.com/WACCapital/sample-app-frontend-multi-account-acme): A sample app that demonstrates best practices for an individual Docker-based frontend app or microservice that talks to backend apps (showing how to do service discovery) and returns HTML. This app is written in NodeJS but these practices are broadly applicable (and in the case of Docker, reusable!) across any technology platform.
1. [sample-app-backend](https://github.com/WACCapital/sample-app-backend-multi-account-acme): A sample app that demonstrates best practices for an individual Docker-based backend app or microservice that talks to a database. This app is written in NodeJS but these practices are broadly applicable (and in the case of Docker, reusable!) across any technology platform.


### WAC Open Source Tools

1. [terragrunt](https://github.com/WACCapital/terragrunt): A thin wrapper for Terraform that provides extra tools for working with multiple Terraform modules.

1. [fetch](https://github.com/WACCapital/fetch): A tool that makes it easy to download files, folders, and release assets from a specific git commit, branch, or tag of public and private GitHub repos. 

1. [WAC-installer](https://github.com/WACCapital/WAC-installer): A script to make it easy to install WAC Modules.

### WAC Training Reference Materials

1. [infrastructure-as-code-training](https://github.com/WACCapital/infrastructure-as-code-training): A sample repo we share with customers when we do training on Terraform, Docker, Packer, AWS, etc.


