# Domain 3: Technology
# (3D: Amazon Web Services Networking Services)

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
    * **Application Load Balancer (ALB):**
      * Routes traffic based on the content of the request (L7).
      * Used for web applications, microservices architectures (Docker containers), and Lambda targets.
      * This is **request based**, meaning it looks at the HTTP protocol and knows where to route the traffic based on:
        * **Path-based** routing (URL path after "/")
        * **Host-based** routing (the domain name)
        * **Query string-based** routing (the pararmeters in the URL strong after "?"
        * **Source IP-based** routing (the client-side IP address)
    * **Network Load Balancer (NLB):**
      * Routes traffic based on the IP protocol data (L4).
      * Offers ultra-high performance, low latency routing.
      * Used for VPC endpoints, TCP and UDP based apps.
    * **Gateway Load Balancer (GLB)**
      * Listens for all packets on all ports (L3).
      * Routes traffic to the target group specified in the "listener rules".
      * Exchanges traffic with appliances using the **GENEVE** protocol (Port 6081).
      * Used in "virtual appliances" such as firewalls and deep packet inspection systems.    

