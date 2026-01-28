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
   * #### Public vs. Private AWS Services
     * Some AWS services are **public** (like S3, CloudFront, Route 53, DynamoDB) - meaning they sit **outside** any VPCs and exist in the AWS Cloud itself.
     * Some AWS services are only available **privately** like E2, RDS, etc.
     * Public AWS resources and services still require permissions and authentication tokens to access - just because they exists outside VPCs does not mean anyone on the internet can access them.  
    
## Subnets
   * #### Overview
     * Subnets exist within VPCs and within AZs (recall VPC are Regional and can traverse AZs).
     * Subnets are nested within AZs - they **cannot** traverse AZs.
     * After creating a VPC, you can add one more more subnets within it.
   * #### Types of Subnets
     * If a subnet's traffic is routed to an IGW, it's a **public subnet**.
     * If a subnet's traffic is **not** routed to an IGW (or is routed to a NAT Gateway), it's a **private subnet**. 
     * If a subnet's traffic is routed to a VPG, it's a **VPN-only subnet**. 


 
