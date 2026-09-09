# AWS Notes

## **IAM**

IAM stands for Identity and Access Management. It controls who can access AWS and what they are allowed to do.

### **Users and Groups**

- The root account is created by default and should not be used for normal daily tasks.
- IAM users represent individual users.
- Users can be added to groups.
- Groups cannot contain other groups.
- A user can belong to multiple groups.

### **Permissions and Policies**

Permissions are controlled using JSON policies.

Policies can be attached to:

- Users
- Groups
- Roles

AWS recommends using least privilege, meaning only giving the permissions that are actually needed.

Important policy parts include:

- Effect
- Action
- Resource
- Condition
- Principal

### **MFA**

MFA adds another layer of security on top of a password.

AWS supports:

- Authenticator apps
- Security keys
- Hardware MFA devices

### **Accessing AWS**

AWS can be accessed through:

- Management Console
- CLI
- SDK

Access keys are used for programmatic access and should never be shared.

### **IAM Security Tools**

**Credentials Report**

Shows IAM users and the status of their credentials.

**Access Advisor**

Shows which AWS services a user can access and when they were last accessed.

---

## **EC2**

EC2 stands for Elastic Compute Cloud and allows you to run virtual machines in AWS.

It can be used for:

- Hosting applications
- Hosting websites
- Running servers
- Installing software

### **EC2 User Data**

User Data is a startup script that runs when an EC2 instance launches.

It can:

- Install software
- Install updates
- Download files
- Start services
- Configure the instance

For example, User Data could install and start NGINX automatically.

### **Instance Types**

Different instance families are designed for different workloads.

- T is burstable general purpose.
- M is balanced general purpose.
- C is compute optimised.
- R and X are memory optimised.
- I is storage optimised.
- G and P are used for GPU workloads.
- Inf is used for machine learning inference.
- Trn is used for machine learning training.

### **Purchasing Options**

**On-Demand**

Pay for what you use with no long-term commitment.

**Reserved Instances**

Used for longer workloads and can reduce costs.

**Savings Plans**

Commit to a certain amount of usage for lower pricing.

**Spot Instances**

Very cheap spare AWS capacity, but AWS can interrupt the instance.

**Dedicated Hosts / Instances**

Used when dedicated hardware is required.

**Capacity Reservations**

Reserve EC2 capacity in a specific Availability Zone.

---

## **Security Groups**

Security Groups act like firewalls for resources such as EC2.

They control:

- Inbound traffic
- Outbound traffic
- Ports
- Protocols
- Source or destination

Security Groups only contain allow rules.

Good practice:

- Only open required ports.
- Restrict SSH to trusted IP addresses.
- Keep rules as tight as possible.

Security Groups can reference other Security Groups, which makes communication between groups of instances easier to manage.

### **Common Ports**

- 21 FTP
- 22 SSH
- 22 SFTP
- 53 DNS
- 80 HTTP
- 443 HTTPS
- 3389 RDP

---

## **Elastic IP**

An Elastic IP is a fixed public IPv4 address.

A normal EC2 public IP can change after stopping and starting an instance, while an Elastic IP stays the same.

Unused Elastic IPs can result in charges.

---

## **Storage**

### **EBS**

EBS is block storage for EC2, similar to a virtual hard drive.

It is useful for:

- Databases
- Logs
- Application files
- Persistent data

EBS volumes are tied to an Availability Zone.

To move the data to another AZ, you can create a snapshot and create a new volume from it.

### **AMI**

An AMI is a template used to launch EC2 instances.

It can contain:

- Operating system
- Installed software
- Applications
- Files
- Configuration

This allows new instances to launch with the same setup.

### **EFS**

EFS is shared file storage that multiple EC2 instances can access at the same time.

**EBS**

Usually attached to one EC2 instance.

**EFS**

Shared across multiple EC2 instances.

---

## **Scalability**

### **Vertical Scaling**

Vertical scaling means increasing the power of one machine.

This could mean adding:

- CPU
- RAM
- Storage

### **Horizontal Scaling**

Horizontal scaling means adding more instances instead of making one instance bigger.

This helps spread the workload across multiple servers.

---

## **High Availability**

High Availability means designing a system so it keeps running if part of it fails.

AWS often achieves this by using multiple Availability Zones.

### **Passive**

One resource handles the workload while another waits as a backup.

### **Active**

Multiple resources handle the workload at the same time.

If one fails, the remaining resources continue running.

---

## **Load Balancing**

A Load Balancer distributes incoming traffic across multiple backend resources.

It helps:

- Prevent one instance becoming overloaded.
- Improve availability.
- Send traffic only to healthy instances.
- Spread traffic across Availability Zones.

Load Balancers can also:

- Provide one DNS endpoint.
- Perform health checks.
- Handle HTTPS.
- Support sticky sessions.

### **Health Checks**

Load Balancers check whether targets are healthy.

If a target becomes unhealthy, new traffic can be sent to other healthy targets.

---

## **Application Load Balancer**

ALB works at Layer 7 and is mainly used for HTTP and HTTPS traffic.

It supports:

- HTTP
- HTTPS
- HTTP/2
- WebSockets

ALB can route traffic based on:

- URL path
- Hostname
- Query string

### **Target Groups**

A Target Group contains the backend resources that receive traffic from the Load Balancer.

Targets can include:

- EC2 instances
- ECS tasks
- Lambda functions
- Private IP addresses

### **Forwarded Headers**

ALB can pass information about the original client to the application.

**X-Forwarded-For**

Contains the original client IP.

**X-Forwarded-Port**

Contains the client port.

**X-Forwarded-Proto**

Shows whether HTTP or HTTPS was used.

---

## **Network Load Balancer**

NLB works at Layer 4.

It is designed for:

- TCP
- UDP
- High traffic
- Low latency
- High performance

---

## **Sticky Sessions**

Sticky sessions keep a user's requests going to the same backend instance for a set period.

A cookie is normally used to remember which instance the user was originally sent to.

---

## **SSL and TLS**

SSL/TLS secures network traffic.

HTTPS uses TLS.

An SSL/TLS certificate:

- Proves the server's identity.
- Enables HTTPS.
- Encrypts traffic.

### **SSL Termination**

The Load Balancer can handle HTTPS encryption instead of the EC2 instances.

### **SNI**

SNI allows one Load Balancer to host multiple HTTPS websites by selecting the correct certificate for the requested hostname.

---

## **Connection Draining**

Connection draining lets existing requests finish when an instance is being removed.

The Load Balancer stops sending new traffic to that instance while existing connections complete.

---

## **Auto Scaling Groups**

An ASG automatically increases or decreases the number of EC2 instances depending on demand.

It uses:

- Minimum capacity
- Desired capacity
- Maximum capacity

If traffic increases, more instances can be launched.

If traffic decreases, instances can be removed.

### **Launch Template**

A Launch Template can define:

- AMI
- Instance type
- User Data
- EBS volumes
- Security Groups
- SSH key pair
- IAM role
- VPC
- Subnets
- Load Balancer settings

### **Scaling Policies**

**Target Tracking**

Keeps a metric around a target value.

**Step Scaling**

Adds or removes instances when thresholds are reached.

**Scheduled Scaling**

Changes capacity at certain times.

---

## **Containers on AWS**

Main AWS container services include:

- ECS
- EKS
- Fargate
- ECR

### **ECS**

ECS is AWS's container management service.

A cluster is a logical group of resources where container workloads run.

With the EC2 launch type, you manage the EC2 infrastructure while ECS manages the containers.

### **Fargate**

Fargate lets you run containers without managing EC2 instances.

You define the container, CPU and memory while AWS manages the infrastructure.

### **ECS IAM Roles**

**Instance Profile**

Gives the EC2 host permissions.

**Task Role**

Gives individual ECS tasks permissions.

This avoids giving every container unnecessary access.

### **ECS Auto Scaling**

ECS can increase or decrease the number of running tasks based on:

- CPU
- Memory
- ALB request count
- CloudWatch metrics

### **ECR**

ECR stores container images.

Features include:

- Vulnerability scanning
- Image tags
- Lifecycle management

### **EKS**

EKS is AWS's managed Kubernetes service.

Important parts include:

- Worker nodes
- Pods
- VPC networking
- Availability Zones
- Load Balancers
- NAT Gateways

Pods are the smallest deployable units in Kubernetes.

Worker nodes run the workloads.

---

## **Serverless**

Serverless means AWS manages the underlying infrastructure while you focus on your application.

It does not mean there are no servers.

### **Lambda**

Lambda runs code without you managing servers.

The normal process is:

1. An event happens.
2. Lambda is triggered.
3. Lambda runs the code.
4. The task is completed.

Lambda supports languages such as:

- Python
- JavaScript
- Java
- Ruby
- C#
- PowerShell

### **Other Serverless Services**

**DynamoDB**

Managed NoSQL database.

**Cognito**

Handles authentication and user sign-ups.

**API Gateway**

Provides APIs that can connect users to backend services such as Lambda.

**S3**

Stores files and static content.

**SNS**

Used for notifications.

**SQS**

Used for message queues.

**Kinesis Data Firehose**

Used for streaming data.

**Aurora Serverless**

Managed database that can scale with demand.

**Step Functions**

Used to manage workflows across multiple AWS services.

### **Lambda Example**

A user uploads an image to S3.

The upload triggers Lambda.

Lambda creates a thumbnail and saves it back to S3.

Metadata can also be stored in DynamoDB.

---

## **VPC**

A VPC is a private network inside AWS.

It gives you control over:

- IP ranges
- Subnets
- Routing
- Internet access
- Security

---

## **CIDR**

CIDR defines IP address ranges.

IPv4 contains 32 bits split across 4 octets.

The prefix tells you how many bits belong to the network.

- /24 means the first 3 octets are fixed.
- /16 means the first 2 octets are fixed.
- /8 means the first octet is fixed.

More network bits means fewer host addresses.

Fewer network bits means more host addresses.

### **Subnetting Formula**

Host bits are calculated using:

32 minus the prefix.

For /26:

32 minus 26 gives 6 host bits.

Total addresses are calculated using 2 to the power of the host bits.

6 host bits gives 64 addresses.

AWS reserves 5 addresses per subnet.

This leaves 59 usable addresses in a /26 subnet.

---

## **Internet Gateway**

An Internet Gateway allows suitable VPC resources to communicate with the internet.

Creating a VPC does not automatically create one.

You need to:

1. Create the Internet Gateway.
2. Attach it to the VPC.
3. Update the route table.

---

## **Bastion Host**

A Bastion Host provides controlled SSH access to private EC2 instances.

The Bastion Host sits in a public subnet.

You connect to the Bastion Host first and then SSH into the private instance.

This prevents the private instance from being directly exposed to the internet.

---

## **NAT Gateway**

A NAT Gateway allows private IPv4 resources to make outbound internet connections.

It is useful for things such as:

- Installing updates
- Downloading packages
- Pulling container images

The private resource does not need a public IP.

Using NAT Gateways in multiple Availability Zones improves availability.

### **NAT Gateway and NAT Instance**

**NAT Gateway**

- Managed by AWS.
- Scales automatically.
- Less administration.

**NAT Instance**

- EC2 instance configured for NAT.
- Managed by you.
- You manage scaling and availability.

---

## **NACL**

A NACL controls traffic entering and leaving a subnet.

NACLs are:

- Subnet level.
- Stateless.
- Able to allow traffic.
- Able to deny traffic.

Because they are stateless, return traffic must also be explicitly allowed.

### **Security Group and NACL**

**Security Group**

- Resource level.
- Stateful.
- Allow rules.

**NACL**

- Subnet level.
- Stateless.
- Allow and deny rules.

---

## **VPC Peering**

VPC Peering privately connects two VPCs.

Important points:

- Uses private IP addresses.
- CIDR ranges cannot overlap.
- Peering is not transitive.
- Route tables must be updated.

---

## **VPC Endpoints**

VPC Endpoints allow resources inside a VPC to access supported AWS services privately.

This avoids using the public internet.

### **Interface Endpoint**

Uses AWS PrivateLink.

### **Gateway Endpoint**

Used for supported services such as:

- S3
- DynamoDB

---

## **IPv6**

IPv6 uses 128-bit addresses compared with IPv4's 32-bit addresses.

This gives IPv6 a much larger address range.

IPv6 was created because IPv4 addresses are limited.

Traditional NAT is normally less necessary with IPv6 because addresses can be globally unique.

### **Egress-Only Internet Gateway**

Used with IPv6.

It allows outbound internet connections while preventing unsolicited inbound connections.

---

## **Route 53**

Route 53 is AWS's managed DNS service.

DNS translates domain names into addresses computers can use.

### **Hosted Zones**

A Hosted Zone stores DNS records for a domain.

**Public Hosted Zone**

Used for DNS names that need to work on the public internet.

**Private Hosted Zone**

Used for DNS names that should only work inside associated VPCs.

---

## **DNS Records**

DNS records store information about a domain.

Common records include:

- A
- AAAA
- CNAME
- MX

### **TTL**

TTL stands for Time To Live.

It controls how long a DNS resolver keeps a DNS record cached.

**High TTL**

Record stays cached for longer.

Useful for stable records.

**Low TTL**

Record expires sooner.

Useful when DNS changes need to be picked up quickly.

### **CNAME**

Points one hostname to another hostname.

Normally used for subdomains.

### **Alias Record**

A Route 53 Alias can point domains to supported AWS resources such as:

- Load Balancer
- CloudFront
- API Gateway

Unlike CNAME, Alias records can also be used with the root domain.

---

## **Route 53 Routing Policies**

### **Simple**

Normal DNS response without advanced routing.

### **Weighted**

Splits traffic between resources using percentages.

Useful for:

- Testing versions
- Gradual migrations
- Splitting environments

### **Failover**

Uses a backup if the main resource becomes unhealthy.

### **Latency-Based**

Routes users to the resource with the lowest latency.

### **Geolocation**

Routes users based on their location.

### **Multi-Value**

Returns multiple healthy IP addresses for the same domain.

It can improve availability but is not a replacement for a Load Balancer.

### **Geoproximity**

Routes based on the geographic location of users and resources.

### **IP-Based**

Routes traffic based on client IP or CIDR ranges.

### **Health Checks**

Route 53 can monitor resources and stop directing users to unhealthy resources.

---

## **CloudFront**

CloudFront is AWS's Content Delivery Network.

It speeds up websites by serving cached content from edge locations closer to users.

CloudFront can deliver:

- Images
- Videos
- HTML
- CSS
- JavaScript
- Files

### **Origins**

An origin is where CloudFront gets its content from.

Origins can include:

- S3
- Application Load Balancer
- EC2
- HTTP servers

### **Caching**

CloudFront checks the nearest edge location first.

If the content is cached, it is served from there.

If it is not cached, CloudFront gets it from the origin and can cache it for future requests.

### **S3 Origin**

S3 can store static files while CloudFront delivers them to users.

Access can be configured so users access the files through CloudFront rather than directly from S3.

### **ALB or EC2 Origin**

CloudFront can also use an ALB or EC2 application as its origin.

CloudFront handles content delivery while the backend handles the application.