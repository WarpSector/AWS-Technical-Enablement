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


