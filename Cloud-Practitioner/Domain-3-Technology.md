# Domain 3: Technology

## AWS Global Infrastructure (The Center)

**Regions**
   * An AWS Region is a geographical area.
   * AWS Regions are spread across the globe.
   * Each AWS Region is designed to be physically isolated from other AWS Regions, but they are connected via the AWS Global Network.
   * AWS Regions each have two or more Availability Zones (AZs).

**Availability Zones (AZs)**
   * AZs are physically isolated from each other within the AWS Region.
   * AZs are designed to be independent failure zones, thus allowing for failover.
       * Failover Design consists of launching EC2 instances in mutliple AZs, so if one AZ goes down, the EC2 instance in the other AZ can pick up the traffic.
       * Failovers can be masked by using Elastic IP addresses.
   * Each AZ has one or more Data Centers within them.
   * Data Centers within an AZ are networked together with redundant, low latency, and high throughput connections.
   * Data Centers within an AZ also have redundant power supplies.

**Internet Traffic Flow into the Cloud (High-Level Overview)**

1. Internet traffic flows into an AWS Region.
2. A Virtual Private Cloud (VPC) inhabits the AWS Region (think of VPCs as the same as Regions - VPCs are Regions, essentially).
3. Internet traffic enters the VPC through an Internet Gateway (IGW).
4. Once inside the VPC, traffic flows into one of the AZs.
5. Once inside the AZ, traffic flows into one of the public subnets (traffic cannot access private subnets).
6. Once inside the subnet, traffic flows into the EC2 instance inside that subnet.

**NOTES ABOUT PRIVATE SUBNETS:**
  * Private subnets are **inaccessible** to internet traffic (the internet doesn't even see the private subnet).
  * If something is needed from the private subnet (for example, a record or some data that is stored), the request is routed through an application in the public subnet first (think of a receptionist routing a message to the back office and then returning a response to the front desk).
  * The only way private subnets are accessible to Administrators are via the AWS Systems Manager or through a Bastion Host "Jump Box" and then SSH jumping into the private subnet (this is the traditional way, the modern way is using AWS Systems Manager).
  * Private subnets can send traffic **OUT** into the internet through a NAT Gateway positioned in a public subnet. 

```mermaid
graph TD
    Internet((Internet)) --- IGW[Internet Gateway]

    subgraph Region [AWS Region]
        IGW --- VPC
        
        subgraph VPC [AWS VPC Regional]
            
            subgraph AZ1 [Availability Zone 2]
                subgraph PubSub1 [Public Subnet B Zonal]
                    Web1[Web Server B]
                end
                subgraph PrivSub1 [Private Subnet B Zonal]
                    DB1[Database B]
                end
            end

            subgraph AZ2 [Availability Zone 1]
                subgraph PubSub2 [Public Subnet A Zonal]
                    Web2[Web Server A]
                end
                subgraph PrivSub2 [Private Subnet A Zonal]
                    DB2[Database A - Standby]
                end
            end

        end
    end

    %% Connections
    Web1 -.-> DB1
    Web2 -.-> DB2
    Web1 <==> Web2
```

## AWS Global Infrastructure (The Ecosystem)

**AWS Outposts**
   * A fully managed service that extends AWS infrastructure, services, and APIs to virtually any **on-premises** data center or co-location space.
   * Provides a **consistent experience** because you use the same AWS Console and tools to manage on-prem resources as you do for cloud resources.
   * **AWS Direct Connect:** A service where a physical fiber optic connection between the AWS Outpost infrastrucutre and the on-prem infrastructure creates a low latency, high throughput, and secure connection (standard connection is using the public internet with a Virtual Private Network (VPN) connection). 
   * Primary Use Cases:
     * Low Latency: When applications need to talk to local equipment in milliseconds (e.g., factory floor robotics).
     * Local Data Processing: When data is too heavy to move to the cloud quickly.
     * Data Residency: Complying with regulations (like GDPR) that require data to stay within a specific physical building or country.
   * Responsibility: AWS ships, installs, and manages the hardware; the **customer** provides the "floor space," power, and cooling.

**AWS Local Zones**
   * An extension of an AWS Region that places compute and storage services close to large population centers (cities).
   * Provides single-digit millisecond latency for high-performance apps (e.g., real-time gaming, VR/AR, media production).
   * Managed entirely by AWS; you gain the benefit of "Edge" computing without managing your own hardware.
   * You must enable the Local Zone in your AWS Account settings before use.

**AWS Wavelength Zones**
   * AWS infrastructure deployments that embed AWS compute and storage services within **5G networks** of telecommunications providers.
   * Provides **ultra-low latency** (single-digit milliseconds) specifically for mobile devices and users on cellular networks.
   * Primary Use Cases:
      * Connected Vehicles: Real-time monitoring and autonomous driving safety.
      * AR/VR on Mobile: Delivering immersive experiences to phones without lag.
      * Healthcare: Real-time medical image analysis during remote procedures.
      * Smart Factories: Processing IoT data from sensors on 5G-connected robots.

**AWS Edge Locations**
   * A global network of Points of Presence (PoPs) used to deliver content (videos, images, multimedia) to end users with the lowest possible latency.
   * The Content Delivery Network (CDN) Connection: Their primary purpose is to host Amazon CloudFront, AWS’s Content Delivery Network (CDN).
   * How it Works: Instead of a user in London fetching an image from a server in North Virginia, they fetch a cached copy from the London Edge Location.
   * Edge Location: Small, numerous, and closest to the user.
   * Regional Edge Cache: Larger caches that sit between your Origin and the Edge. If the Edge doesn't have the file, it checks here first before bothering your main server.
   
```mermaid
graph TD
    %% Define Styles
    classDef region fill:#ff9900,stroke:#232f3e,stroke-width:2px,color:#fff;
    classDef extension fill:#232f3e,stroke:#ff9900,stroke-width:1px,color:#fff;

    subgraph Parent_Region [AWS Region]
        direction TB
        AZ1[Availability Zone A]
        AZ2[Availability Zone B]
    end

    %% Extension Services
    LZ[Local Zone]:::extension
    WZ[Wavelength Zone]:::extension
    OP[AWS Outposts]:::extension
    EL[Edge Location]:::extension

    %% Connections
    Parent_Region --- LZ
    Parent_Region --- WZ
    Parent_Region --- OP
    Parent_Region --- EL

    %% Descriptions (Using click or notes isn't great in Mermaid, so we use text nodes)
    LZ_Text[Low Latency in Cities] -.-> LZ
    WZ_Text[5G Mobile Edge] -.-> WZ
    OP_Text[On-Premises Data Center] -.-> OP
    EL_Text[Content Delivery / CloudFront] -.-> EL
```


## Amazon Elastic Compute Cloud (EC2)

**EC2 Overview and Basics**
   * **Compute is the engine of the Cloud.** This is where all the compute processing (CPU) of your Cloud applications and web services happen.
   * Amazon Elastic Compute Cloud (EC2) is an AWS service that lets you easily use compute resources in the Cloud.
   * You run virtual instances/machines in the Cloud when you use Amazon EC2.
   * Amazon EC2 instances can run Linux, Windows, or MacOS.
   * The instances are **elastic** allowing you to provision additional servers when needed and terminate unused or underutilized servers when you no longer need them.
      * **NOTE:** Scaling is NOT automatic by default. You will need to set the rules in the AWS Management Console and configure the Auto-Scaling Groups (ASGs) to scale your EC2 instances when necessary.
      * The ASGs will scale **horizontally** since this is not disruptive.
      * Vertical scaling is available to perform manually, but it's disruptive (requires you to stop/start the instance) and defeats the purpose of the Cloud's elasticity (horizontal scaling and distributing the workload). 
   * You can select different instance types with varying combinations of CPU, memory, storage, networking, and OS:
      * Family/Generation: Examples: Main/General Purpose (M), Compute Intensive (C), Graphics Intensive (G), Data (D), RAM (R), Cheap (T), Fast (F), High Memory (U), High Compute + Memory (Z), High Disk Throughput (H), etc.  
      * Volume/Size: Nano, Micro, Small, Medium, Large, X-Large, 2X-Large - just remember the next size is double the previous size.
   * You pay only for what you use.
   * AWS manages the physical underlying infrastructure (host) and the virtualization layer (hypervisor), while the users manage their VMs and resources in the Cloud.
   * EC2 is IaaS: AWS manages the underlying hardware and you manage everything else from the OS (platform) and up.
   * EC2 instances "sit" inside the public or private subnet within the AZ, which "sits" within the (regional) VPC.

**Virtualization**
   * EC2 works via virtualization technology.
   * The physical server uses virtualization software, which creates a **hypervisor** over the physical server (enabled by using virtualization software such as Zen, Microsoft V, KVM, etc.).
   * The hypervisor creates a layer of "abstracton" between the physical server and the virtual server by allocating resources from the physical hardware into the virtual machine (VM) including the memory, storage, networking, and CPU power.
   * Virtual Machines are also referred to as virtual servers or virtual instances.
   * Virtualization allows you to run multiple VMs on the same physical server.
   * Virtualization allows portability allowing you to move the VM to different physical servers (it resolves the limitation of having the OS tied to a specific piece of hardware).
   * Virtualization allows scaling - allowing you to spread your VMs across multiple physical servers.

**EC2 Benefits**
   * Elastic: increase/decrease capacity to meet demand fluctuations and web traffic spikes.
   * Reliable: highly reliable environment where replacement instances can be provisioned rapidly with **Regional** service-level agreements (SLAs) of 99.99% **if architected for High Availability** (i.e., EC2 instances are spread across multiple AZs and have an elastic load balancer along with an ASG to distribute and handle the load while the first failed EC2 instance is repaired and brought back online (this is also known as "self-healing" architecture: High Availability (HA) + Fault Tolerance (FT)).
   * Inexpensive: Amazon passes on the benefit of economies of scale to the users.
   * Integrated: EC2 instances are integrated with a wide variety of AWS services (including S3, RDS, and VPC) so you can build complete services within the Cloud.
   * Secure: EC2 works inconjunction with the VPC to provide a secure location with an IP address range you can configure to allow access to your EC2 (in combination with network security measures like Network Access Control Lists (Network ACLs) protecting your subnet boundaries, Web Application Firewalls (WAFs) protecting your application load balancers (ALBs), and Security Groups protecting the EC2 instances themselves).

**Amazon Machine Image (AMI)**
   * When you launch an EC2 instance, you must select an Amazon Machine Image (AMI) that specifies the type of OS you want to run, the resources you want to allocate to the VM, and the data and applications you want in the VM.
   * Think of AMIs as the "blueprint" for the EC2 instance you are launching. If the EC2 instance is the "house", then the AMI is the "furniture".
   * There are 3 types of AMIs:
       * Community AMIs: AMIs that are created by other AWS users, which you can access from the community and download for free.
       * AWS AMIs: AMIs built by Amazon and available for purchase since they come packaged with additional licensed software.
       * Custom AMIs: AMIs you've built yourself. Best practice for Custom AMIs is to build them and install all the software, data, applications, and configurations you want before you create the snapshot. This way, if you need to load it into a different EC2 instance, you have the entire VM snapshotted and can easily upload it in one fell swoop (versus having to manually install the software and apps and manually configuring the instance each time you want to boot up a new one).
   * **NOTE:** AMIs are "region locked" meaning they only exist in the Region they are created. You must copy them to other Regions if you want to launch a specific AMI in a different Region.

**EC2 Pricing**
   * There are different pricing models available:
       * On-Demand Instances:
         * Good for users that need flexibility without any upfront payment or long-term committment (true "pay-as-you-go" pricing model).
         * Good for users with unpredictable and spiky web traffic or unpredictable workloads that need to be processed with zero interruption.
         * Good for users developing and testing applications on EC2 for the first time.            
       * Reserved Instances (RIs):
         * Good for users that have steady-state or predictable workloads.
         * Good for users that need reserved capacity for their apps and workloads.
         * Standard Reserve Instances (RIs) provide up to 75% off the On-Demand price. 
         * Users can make upfront payments to reduce their computing costs even further.
         * RIs can be scheduled and launched within a time window you reserve, which allows you to match your reserved capacity to a predictable recurring schedule.
      * Spot Instances:
         * Good for users who have flexible start and end times, because Spot Instances can be interrupted if other users demand additional capacity.
         * Good for users who have an urgent need for a large amount of additional compute capacity.
         * If Amazon interrupts or terminates your instance, you don't pay. If you terminate, you pay for the hour.
      * Dedicated Hosts:
         * These are dedicated **physical** servers reserved only for you.
         * The user has control over which instances to deploy on the host, though the user can only deploy one instance size and type on the host.
         * This allows complete isolation. No one else will use the underlying server you are hosting your instances on so you can create all the VMs you want.
         * Good for users with regulatory compliance and/or licensing requirements.
         * This is the most expensive option.
      * Dedicated Instances:
         * This can be Dedicated Hosts with Dedicated Instances already spun up for you.
         * This can also be Dedicated Instances just for you, but not on Dedicated Hosts (you just have the instances, but the host can be shared with other users). 
         * Billing is per instance.
      * Savings Plans:
         * Flexible pricing model that saves up to 72% on your AWS compute usage.
         * Offers lower pricing on EC2 instance usage regardless of family, OS, size, or type.
         * Savings also apply to AWS Lambda and AWS Fargate.          

**EC2 Metadata and User Data**
   * User data is a form of **bootstrapping** - you provide a script that the instance automatically executes when it first boots up without you having to manually perform whatever function the script is supposed to execute after the instance boots up (think of it as a "To Do" list for the server to carry out the first few seconds it "wakes up").
   * Metadata is data about the EC2 instance that can be used to configure and manage the running instance (available at http://169.254.169.254/latest/meta-data
   * **NOTE:** Metadata and User Data are NOT encrypted.

**AWS Family of Compute Services**
   * While EC2 is the "grandfather" of Amazon's compute cloud services, other compute services are available:
      * AWS Batch:
        * Enables developers, scientists, and engineers to easily and efficiently run hundreds of thousands of batch computing jobs on AWS.
        * AWS Batch dynamically provisions the optimal quantity and type of compute resources for the job being processed.
        * With AWS Batch, you package the code to run the jobs, specify any dependencies, and then submit the batch job via the AWS Management Console, AWS Command Line Interface (CLI), or an AWS Software Development Kit (SDK).
      * Amazon Lightsail:
        * Best for users who do not have technical expertise with AWS as this service makes it very easy for them to provision compute services.
        * Amazon Lightsail provides compute, storage, and networking capacity and capability to deploy and manage websites, webservices, and databases in the Cloud.
        * Amazon Lightsail includes a VM, SSD-based storage, data transfer, DNS management, and a static IP.
        * Lets you deploy load balancers and attach block storage.
        * Best suited for projects that require a few dozen instances or less: good for blogs, websites, web apps, e-commerce, etc.
        * Not as powerful as EC2 instances.
        * Has a simple management interface.
