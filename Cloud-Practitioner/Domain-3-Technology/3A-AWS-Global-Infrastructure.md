# Domain 3: Technology
# (3A: AWS Global Infrastructure)

# High-Level
### 🏛️ 1. The Core Infrastructure: Regions & AZs

| Component | Scope | Analogy | Key Characteristics |
| :--- | :--- | :--- | :--- |
| **Region** | Geographic Area | The "Island" | Completely isolated. Used for **Data Residency** and local compliance. |
| **Availability Zone (AZ)** | Within a Region | The "Building" | One or more data centers. Independent failure zones (Failover). |
| **Data Center** | Within an AZ | The "Server Room" | Redundant power/cooling. The actual physical hardware. |
| **VPC** | Regional | The "Property Line" | Spans all AZs in a Region. Your isolated virtual network. |

---

### 🛡️ 2. The Edge & Hybrid Ecosystem

| Service | "Vibe" | Primary Target | Pro-Tip for Feb 7 Exam |
| :--- | :--- | :--- | :--- |
| **Edge Location** | Content Caching | Global users (CloudFront) | **"Smallest & Most Numerous."** Used for S3/CloudFront caching. |
| **Local Zone** | City Computing | Metro areas (e.g., NYC, LA) | **"City Edge."** High-perf compute (Gaming/Media) near users. |
| **Wavelength** | 5G Mobile | Mobile Device users | **"Telecom Edge."** Hardware inside 5G carrier networks. |
| **AWS Outposts** | On-Premises | Corporate Data Centers | **"The Rack."** You provide floor space; AWS provides the hardware. |

# Deep Dive

## AWS Global Infrastructure (The Core)

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




