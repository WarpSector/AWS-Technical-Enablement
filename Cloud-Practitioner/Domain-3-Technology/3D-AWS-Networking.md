# Domain 3: Technology
# (3D: Amazon Web Services Networking)

## Amazon Route 53
  * ### Amazon Route 53 is the AWS Domain Name Service (DNS).
    * Recall that websites are "read" by computers as IP addresses.
    * When you type in a website address into the browser, your computer sends a query to the DNS Server.
    * The DNS server has a "zone file" with a list of IP addresses.
    * It finds the IP address you queried from its "zone file" and returns it to your computer (if the first DNS server can't find the IP, it forwards the request to the next DNS server until the query is answered).
    * Once your computer receives the destination IP address, it performs an HTTP GET protocol over the internet to connect to the destination website.
  * ### Fully Qualified Domain Names (FQDNs)
    * When we're using DNS, we're actually using something called a FQDN.
    * **Example:** www.example.com <-- This is a FQDN.
      * There's usually a "." after the ".com" - so it's ".com.", but we don't use a dot after we type in the web address. This dot "." after the ".com" is the **root domain**.
      * The ".com" is the **top-level domain**.
      * The ".example" is the **subdomain**.
      * The "www" is the **hostname**.
  * ### Main Functions of Route 53
    * **Domain Registration:** Route 53 allows you to register domain names.
    * **Domain Name Service (DNS):** Route 53 translates domain names to IP addresses using a global network of authoritative DNS servers.
    * **Health Checking:** Route 53 sends automated requests to your application to verify that it's reachable, available, and functional.
    * **You can use any combination of these functions when you use this service.**
  * ### Benefits of Route 53
    * Domain Registration (getting your own domain name).
    * Domain Name Service (translates the domain names to IP addresses using a global DNS network).
    * Traffic Flow (sending users to the best endpoint).
    * Health Checking (of your domain name).
    * DNS Failover (automatically changes the domain endpoint if the system fails).
    * Integrates with other Amazon services such as Elastic Load Balancing (ELB), S3, and CloudFront as endpoints.
  * ### Routing Policies used by Route 53
    * **Simple:** routes to the IP adress associated with the name.
    * **Failover:** routes to the secondary address if the primary is down.
    * **Geolocation:** routes based on geographic location of the request.
    * **Geoproximity:** routes to closest geographic region.
    * **Latency:** uses the lowest latency routes to the resource.
    * **Multivalue Answer:**  returns several IP addresses.

## Elastic Load Balancing (ELB)
  * ### Overview
    * Elastic Load Balancing distributes incoming web traffic and workloads across resources to provide **high availability** and **fault tolerance**.
    * ELBs can be attached to EC2 instances, ECS/EKS Containers, IP address, and Lambda functions.
  * ### Types of Load Balancers
    * #### Application Load Balancer (ALB):
      * Routes traffic based on the content of the request (L7).
      * Used for web applications, microservices architectures (Docker containers), and Lambda targets.
      * This is **request based**, meaning it looks at the HTTP protocol and knows where to route the traffic based on:
        * **Path-based** routing (URL path after "/")
        * **Host-based** routing (the domain name)
        * **Query string-based** routing (the pararmeters in the URL strong after "?"
        * **Source IP-based** routing (the client-side IP address)
    * #### Network Load Balancer (NLB):
      * Routes traffic based on the IP protocol data (L4).
      * Offers ultra-high performance, low latency routing.
      * Used for VPC endpoints, TCP and UDP based apps.
    * #### Gateway Load Balancer (GLB):
      * Listens for all packets on all ports (L3).
      * Routes traffic to the target group specified in the "listener rules".
      * Exchanges traffic with appliances using the **GENEVE** protocol (Port 6081).
      * Used in "virtual appliances" such as firewalls and deep packet inspection systems.    

## Virtual Private Cloud (VPC)
   * #### Overview
     * A VPC is a virtual network dedicated to your AWS account.
     * VPCs operate at the **Regional** level and you can create up to five (5) VPCs in a Region.
     * VPCs span across all AZs in a Region.
     * VPCs are logically isolated from other VPCs in the AWS Cloud.
     * VPCs connect to the internet via an **internet gateway (IGW)** or a **virtual private gateway (VPG)** if connecting to a VPN.
     * A VPC provides complete control over the virtual networking environment including selection of IP addresses, creation of subnets, and configuration of routing tables and gateways.
     * When you create an AWS account, a VPC is created for you by default in all AWS Regions.
     * You have full control over who has access to the AWS resources within your VPC.
     * **Subnets** (both public and private) exist inside your VPC, nested inside the AZs. 
   * #### Classless Inter-Domain Routing (CIDR) Blocks and Subnet Masks
     * A **CIDR Block** is the overall IP address range for a VPC.
     * An example CIDR Block for a VPC is: 10.0.0.0/16 (this is the primary block of IP adresses inside your VPC).
     * Each VPC has a router (behind the scenes) that routes traffic inside the VPC to the subnets and AWS resources.
     * **Route tables** are used to route traffic inside the VPC to its destination.
     * Subnets are then assigned a **subnet mask** - a subset of addresses from the VPC's CIDR Block (example: if the VPC's CIDR Block is 10.0.0.0/16, a subnet inside the VPC will have an address of 10.0.1.0/24. Another subnet will have 10.0.2.0/24. A third 10.0.3.0/24, and so on).
     * Subnets will have longer subnet masks than the VPC's CIDR block.
   * #### VPC Peering
     * VPCs can be connected to other VPCs via **VPC Peering**.
     * VPC Peering is connecting two VPCs together via private IP adresses.
     * VPC Peering is **non-transitive** meaning VPCs cannot route through other VPCs to make a connection to another VPC - all VPCs must be directly peered to be able to talk to each other.
     * VPC Peering can be done with VPCs in different Regions and different accounts - you just need to connect them using their private IP addresses. 
   * #### AWS Site-to-Site VPN vs. AWS Direct Connect
     * **AWS Site-to-Site VPN:** You can connect your VPC to your on-prem data center **over the internet** using a VPN. While your connection is encrypted, it's subject to internet bandwidth.
     * **AWS Direct Connect:** You can connect your VPC to your on-prem directly using AWS's backbone (not the public internet) to achieve lower latency and higher throughput as well as secure connections. 
   * #### AWS Transit Gateway
     * The AWS Transit Gateway makes complex VPC Peering architectures simple by "plugging" all of your VPCs into a transit  hub allowing all VPCs the ability to connect and talk to each other.
     * The AWS Transit Gateway also allows you to connect an on-prem data center to your VPC Peering hub so all your VPCs and your on-prem data center can all connect and talk to each other. 
   * #### Public vs. Private AWS Services
     * Some AWS services are **public** (like S3, CloudFront, Route 53, DynamoDB) - meaning they sit **outside** any VPCs and exist in the AWS Cloud itself.
     * Some AWS services are only available **privately** like E2, RDS, etc.
     * Public AWS resources and services still require permissions and authentication tokens to access - just because they exists outside VPCs does not mean anyone on the internet can access them.  
    
## Subnets
   * #### Overview
     * Subnets exist within VPCs and within AZs (recall VPC are Regional and can traverse AZs).
     * Subnets are nested within AZs - they **cannot** traverse AZs.
     * After creating a VPC, you can add one more more subnets within it.
   * #### Types of Subnets (Public vs. Private)
     * If a subnet's traffic is routed to an IGW, it's a **public subnet**.
     * If a subnet's traffic is **not** routed to an IGW (or is routed to a NAT Gateway), it's a **private subnet**. 
     * If a subnet's traffic is routed to a VPG, it's a **VPN-only subnet**. 

## Firewalls
   * #### Network Access Control Lists (NACLs or Network ACLs)
     *  Network ACLs protect the subnet boundaries inside the VPC.
     *  Network ACLs are layer 3 (network) security.
     *  Network ACLs are **stateless** - they don't "remember" the traffic coming in or our, so both inbound and outbound traffic is inspected by the NACL.
     *  Since Network ACLs are stateless, they allow you to set both allow/deny rules.
   * #### Security Groups
     * Security Groups protect the EC2 instances inside the subnets.
     * Security Groups are layer 4 (transport) security.
     * Security Groups are **stateful** - they will inspect the inbound traffic, but not inspect the same traffic leaving since it "remembers" the traffic.
     * Since Security Groups are stateful, they allow you to set only allow rules.
   * #### Web Application Firewalls (WAFs)
     * WAFs protect ALBs, CloudFront, and API Gateways.
     * WAFs are level 7 (application) security.
     * WAFs block against cross-site scripting (XSS), SQL injection attacks, and HTTP floods.
     * WAFs are **stateful**.
    
## NAT Gateways
   * #### Overview
     * NAT Gateways are what you use to connect internet traffic to an EC2 instance inside of a private subnet.
     * NAT Gateways sit inside the public subnet and are connected between the IGW and the private EC2 instance.
     * NAT Gatewats are managed by AWS.
     * NAT Gateways automatically provide high availability (multi-AZ deployment)
       * **REMEMBER:** High Availability (HA) = 99.99% uptime, if there's a failure the user will experience a very short "blip" while the service switches over to the next AZ.
       * **REMEMBER:** Fault Tolerance (FT) = 100% uptime, if there's a failure the user will not even know or experience a drop in service because you have an exact, fully operable identical infrastructure running in another AZ (aka: "fault masking").

## AWS Direct Connect (DX)
  * #### Overview
    * Amazon Direct Connect is an alternative to connecting on-prem data centers to the AWS Cloud.
    * DX creates a private, fiber optic connection using the AWS backbone to connect on-prem DCs to the Cloud.
    * It's an alternative to using Amazon Peer-to-Peer VPN, while encrypted, still uses the public internet and is prone to bandwidth issues (as is the case using the public internet backbone).
    * DX allows you to connect to all AZs within the Region.
  * #### Benefits of DX
    * Reduces cost when moving large volumes of data and traffic
    * Increases reliability (predictable performance)
    * Increases bandwidth (predictable bandwidth)
    * Decreases latency

## AWS Outposts
  * #### Overview
    * AWS Outposts is a fully managed service that extends AWS infrastructure, services, APIs, and tools to any on-prem data center.
    * AWS Outposts are installed co-located hardware racks in the same on-prem data center to provide a truly hybrid Cloud environment.
    * AWS Outposts extends the VPC into the on-prem data center.
    * AWS Outposts provides compute, storage, database, and other services that can run locally and allow you to manage your on-prem infrastructure using AWS tools. 
  * #### Use Cases
    * Workloads that require low latency with on-prem infrastructure.
    * Local data processing
    * Data residency requirements
    * Migration of applications with local system interdependencies.
 
## AWS Global Accelerator
  * #### Overview
    * Global Accelerator improves the availability and performance of applications for local and global users.
    * Global Accelerator uses the AWS Global Network to optimize the path for users to applications, which improves the performance of TCP and UDP traffic (such a online gaming).
    * **Example:** Users accessing applications via TCP/UDP traffic --> Routed to Regional Edge Location --> Global Accelerator uses the AWS Global Network backbone to optimize traffic to Origin Regions.
    * Global Accelerator accomplishes this by providing static IP addresses (**anycast IPs**) that act as **fixed entry points** to application endpoints in a single and/or multiple Regions (connecting to ALBs, NLBs, or EC2 instances). 
