PROJECT:- VPC Public-Private Subnet Architecture (Production-Ready)

Designed and implemented a scalable, secure, and highly available AWS infrastructure using VPC, Auto Scaling, and Load Balancer to host applications in private subnets.

Infrastructure Overview:-

* VPC: Provides an isolated virtual network environment in AWS
* Public Subnet:
Application Load Balancer
Bastion Host (Jump Server)
* Private Subnet:
EC2 instances managed by Auto Scaling Group
* NAT Gateway:
Enables outbound internet access for private instances (e.g., updates, package installation)
* Auto Scaling Group:
Maintains desired capacity (min: 2, max: 4)
Automatically replaces unhealthy instances
* Target Group:
Routes traffic only to healthy instances
* Security Groups:
Control inbound and outbound traffic (port-based rules)

Setup Procedure:-

* Create VPC
* Create public and private subnets
* Attach Internet Gateway to VPC
* Create NAT Gateway in public subnet
* Launch Bastion Host (public access)
* Create Launch Template
* Configure Auto Scaling Group
* Create Application Load Balancer
* Attach Target Group


Security Design:-

* Bastion Host:
Used to securely SSH into private EC2 instances
Prevents direct exposure of private servers
* Private Subnets:
No public IP addresses
Protect sensitive backend systems
* NAT Gateway:
Allows private instances to access internet securely (outbound only)
* Security Group Rules:
Port 22 → SSH via Bastion Host
Port 8000 → Application access

Key Features:-

* High Availability with Auto Scaling:-

Maintains application uptime by dynamically adjusting EC2 instances (min: 2, max: 4) based on traffic and health checks.

* Secure Access via Bastion Host:-

Ensures controlled and secure access to private instances without exposing them to the public internet.

* Load Balancing using Application Load Balancer:-

Distributes incoming traffic across multiple instances, ensuring reliability and efficient resource utilization.

* Private Infrastructure with NAT Gateway:-

Enables outbound internet access for private instances while keeping them isolated from direct public access.

* Traffic Flow:-

User → Load Balancer → Target Group → EC2 (Private Subnet) → Internet (via NAT Gateway if required)

Failure Handling:-

* If an EC2 instance fails:

The Application Load Balancer marks it as unhealthy
Traffic is stopped to that instance
Auto Scaling Group launches a replacement instance

* Traffic Rerouting:-
Load Balancer sends traffic only to healthy instances
Failed instances are removed automatically
Traffic is redistributed across remaining instances

* Scaling Mechanism:-

Scaling is triggered using metrics from Amazon CloudWatch:

CPU > 70% → Scale Out

CPU < 30% → Scale In

New instances are automatically registered with the Load Balancer.


