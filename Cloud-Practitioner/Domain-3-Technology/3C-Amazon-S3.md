# Domain 3: Technology
# (3C: Amazon Simple Storage Service (S3))

## Amazon Simple Storage Service (S3)

### Storage Basics
* **Storage is broken down into three (3) types:**
  * #### Block-Based Storage (Block):
    * These are physical hard drives.
    * They can be Hard Disk Drives (HDDs) with the spinning platter and needle (like a record player) or Solid State Drives (SSDs) which use flash memories and have no moving parts.
    * SSDs are thousands of time faster than HDDs.
    * HDDs (also called "magnetic drives") are cheaper and still used in some places.
  * #### File-Based Storage (File):
    * File systems are mounted on hard drives (block-based storage containers) and the files are stored in data directories - think of all the files arranged like a tree with multiple branches.
    * Network file systems are essentially networked block-based storage drives shared over a network.
    * Hardware known as **network attached storage servers (NAS)** are "mounted" to the network and "shares" files over the network accessed via data directories (same as how you'd access files on your local block-based storage).
  * #### Object-Based Storage (Object):
    * Users upload files into an **object based storage container** over the internet using HTTP/HTTPS protocols (e.g., GET (Read), PUT (Update), POST (Create), SELECT (Select), DELETE (Delete)).
    * This storage type is called a "bucket" because there is no file hierarchy - think of all the files (objects) as grains of sand into a bucket (though there is a way to mimic a hierarchy via S3).
    * Any type of file can be uploaded into a bucket.
    * Objects can be managed by developers and services can be integreated via **REpresentational State Transfer (REST)** API so the files can be managed via code.
      * **Recall:** Rest APIs are a set of rules for how computers should "talk" to each other over the internet using HTTP protocol. 
    * This type of storage is scalable and very cost-effective.
* #### Differences between File-Based Storage and Object-Based Storage
    * File-based storage is accessed via local and networked HDDs/SSDs vs. Object-based storage which is accessed over the internet.
    * File-based storage uses data directories and a file hierarchy (re: the "tree with many branches" analogy) vs. Object-based storage does not use any directories nor has a hierarchy (re: "sand in a bucket" analogy).
    * File-based storage is always accessible since local hard drive and network hard drives are mounted and always available vs. Object-based storage once the RestAPI call is made, the connection is closed. 
   
### Amazon Elastic Block Store (EBS) and Instance Stores
* #### Amazon Elastic Block Store (EBS)
   * This is the block-based storage system on AWS.
   * EBS exists as volumes within Availability Zones (AZs) and can be mounted to EC2 instances.
   * The EBS volume is automatically replicated within the AZ.
   * The EBS volume **will not delete** any data/files if the EC2 instance connected to it is terminated.
* #### Instance Stores
   * Instance Store volumes are physically attached to the EC2 instance host server (re: the block storage hardware in the physcial host server the EC2 instance is running on).
   * Instance Store volumes are included with the EC2 instance and are good for high throughput and high performance.
   * Instance Store volumes are **EPHEMERAL** - if the EC2 instance is terminated or the power is lost to the host server, the Instance Store will be wiped.
* #### EBS Snapshots
   * EBS Snapshots are **back ups** of EBS volumes.
   * Snapshots are captured **point-in-time** states of the EBS volume.
   * Snapshots are not stored in the AZ, they are stored in Amazon S3 outside the AZ.
   * Snapshots are **incremental** - once the original baseline snapshot is taken, subsequent snapshots capture the changes made to the EBS volume.
   * You can take the EBS snapshot and move that EBS volume into different AZs so multiple EC2 instances in different AZs see the same EBS volume.
   * You can also create an AMI from a snapshot and distribute them to different EC2 instances that way.
   * **Data Lifecycle Manager (DLM):** A service you can use to automate the creation, retention, and deletion of your EBS snapshots.

### Amazon Elastic File System (EFS)
   * #### Basics
     * Amazon EFS is a shared file system that can be shared across multiple EC2 instances.
     * An EFS can be Regional and shared to different AZs.
   * #### Regional EFS
     * **Regional EFS** are connected to a **mount target** (efs/mnt) inside an AZ and then the EC2 instances inside that AZ connect to that mount target to access the Regional EFS.
      * **NOTE:** The connection protocol to mount EC2 to mount targets is **network file system (NFS)**, which is only available in **Linux**.
      * You can use NFS protocol with other OS, but Amazon only supports Linux.
    * You can also deploy an EFS in a single AZ through **one zone** mount targets where the EFS --> Local AZ MTN --> Local AZ EC2 Instances are shared within that AZ.
      * You can connect to mount points in different AZs if you need to.
   * #### Data Consistency
     * Regional EFS are durable, meaning the files are copied across the AZs they are mounted to.
     * **File Locking:** NFS protocol allows read/write locking to ensure data consistency.    
   * #### Storage Classes
     * **EFS Standard:** uses SSD for low-latency performance.
     * **EFS Infrequent Access (IA):** useful if you don't need to access the files regularly and is a more cost-effective option than the EFS Standard.
     * **EFS Archive:** used for archival purposes and an even cheaper option than EFS IA.
   * #### Replications, Mount Points, and Back Ups
     * EFS can be replicated across Regions for disaster recovery (DR) purposes.
     * **Note:** You can create a mount point in the Region the EFS is replicated (re: 2nd Region), however, that 2nd mount point will only allow **read-only** access to the EFS (the only time it won't be read-only is if your access fails over to the 2nd Region).
     * EFS can also be connect to an on-prem data center (from outside the Cloud) via Amazon Direct Connect or a VPN connect using the Linux NFS protocol.
     * **Automatic Back Up:** EFS uses **AWS Backup** for automatic file system backups.
     * **Performance Options:**
      * **Provisioned Throughput:** you set the performance and throughput you want regardless of how large the EFS is.
      * **Bursting Throughput:** throughput scales with the amount of storage and can burst to higher performance when needed.
    
### Amazon Simple Storage Service (S3)
   * #### Basics
     * S3 uses **buckets** - a container where you store your objects.
     * **Objects** are files - S3 stores any type of file.
     * Files can be as large as 5 TB.
     * S3 is designed for 99.999999999% ("11 9's") durability.
     * S3 can store millions of objects (storage is virtually unlimited), can scale very easily, and is cheap.
   * #### Typical Use Cases
     * Backup and Storage
     * Application Storage
     * Media Hosting
     * Software Delivery
     * Static Website
   * #### S3 on the Internet   
     * S3 uses HTTP/HTTPS protocols over the internet to GET, PUT, POST, DELETE objects and uses RestAPI (so developers can use an Amazon Software Development Kit (SDK) to write code integrating S3 into their applications).
     * **Keys** are the file names of the objects (so when you see the S3 URL, at the end you'll see the file name of the object - this is called the **key**).
      * Example URL: https://bucket.s3.aws-region.amazonaws.com/key <-- "key" is the file name of the object 
     * **Bucket Names** the name of the bucket has to be unique across all of AWS.
   * #### S3 Objects consist of:
     * Key (name of the object)
     * Version ID
     * Value (the actual data)
     * Metadata
     * Subresources
     * Access Control Information
   * #### Accessing S3   
     * S3 is typically accessed over the internet, so to access it from (or connect it to) your VPC, you'll be doing it over the public internet.
      * **Path**: EC2 in Public Subnet (or EC2 in Private Subnet via NAT Gateway) --> Internet Gateway (IGW) --> Public Internet --> S3 (recall that S3 exists **outside** of AZs and sits in the public space of AWS). 
     * You can access S3 privately (meaning not through the public internet) by connecting to S3 via an **S3 Gateway Endpoint**, which connects your VPC directly to S3 via the AWS Internal Backbone (so your traffic never touches the public internet backbone).
      * **NOTE:** To access S3 privately from an on-prem data center, you would connect to S3 via an **AWS Storage Gateway** using **AWS Direct Connect**.
      * On Prem DC --> AWS Storage Gateway --> AWS Direct Connect (Public or Private Virtual Interface (VIF)) --> Amazon S3
   * #### S3 Versioning
     * **Versioning** is keeping multiple variants of the same object in a bucket (re: if you upload a newer version of the same object, both the newer version and the old version will be available in the bucket).
     * Versioning can preserve, retrieve, and restore every version of an object stored in a bucket.
     * Enabling versioning in your bucket can recover your object in case of accidental deletion or overwrite.      
   * #### S3 Replication
     * **Cross-Regional Replication (CRR):** Buckets are replicated across different Regions.
     * **Same-Region Replication (SRR):** Buckets are replicated within the same Region.
     * **Versioning must be enabled in the bucket in order to use CRR and/or SRR!** 
   * #### S3 Lifecycle Management
     * **Transition Actions:** when objects transition to different storage classes (e.g., S3 Standard transitions to S3 Standard IA after 30 days of no use/retrieval).
     * **Expiration Actions:** define when an object expires so S3 will automatically delete it. 

### Storage: Durability vs. Availability
   * #### Durability
     * Protection against:
      * Data Loss
      * Data Corruption
      * "11 9's" of durability: If you store 10 million objects, then you expect to lose 1 object every 10,000 years!
      * The "11 9's" of durability is accomplished because AWS stores multiple copies of objects across different AZs.
   * #### Availability
     * Measurement of:
      * Amount of time data is available to you.
      * Expressed as a percent of time per year (e.g., 99.99%).

### Amazon S3 Storage Classes
   * **There are a total of seven (7) classes of storage available with S3.**
   * #### Data to be Accessed Quickly:
     * **S3 Standard:**
       * Automatically stored in S3 standard by default.
       * Latency is in milliseconds. 
     * **S3 Intelligent Tiering:**
        * Data is moved into different storage classes based on how frequently you are accessing that data to try and optimize for cost and performance.
        * Latency is in milliseconds. 
     * **S3 Standard Infrequent Access (IA):**
        * Data that is infrequently accessed for 30 days.
        * A fee is charged for retrieval.
        * Latency is in milliseconds.
     * **S3 One Zone Infrequent Access (IA):**
        * Data that is infrequently access for 30 days, but is stored **only in one AZ*8 (lowers cost since it's being stored in one AZ versus the usual 3 or more AZs).
        * A fee is charged for retrieval.
        * Latency is in milliseconds.
   * #### Data to be Archived:
     * **S3 Glacier Instant Retrieval:**
        * Data archived for a minimum of 90 days.
        * Can be retrieved in milliseconds (useful for data that needs to be archived, but retrieved quickly when needed).
        * A fee is charged for retrieval. 
     * **S3 Glacier Flexible Retrieval:**
        * Data is archived for a minimum of 90 days.
        * Can be retrieved in 3-5 hours standard, 5-12 hours for bulk data retrieval.
        * Expedited Retrieval: 1-5 minutes.
        * A fee is charged for retrieval.
     * **S3 Glacier Deep Archive:**
        * Data is archived for a minimum of 180 days.
        * Can be retrieved in 12 hours standard, 48 hours for bulk retreival.
        * Expedited Rertrieval: Unavailable.
        * This solution is for data that is unlikely to be accessed, but needs to be stored for regulatory/compliance reasons.
        * A fee is charged for retrieval.
     * **Additional Features of AWS Glacier:**
        * **S3 Object Lock:** Objects can be "write-once read-many" (WORM) configured, which prevents the object from being overwritten or deleted for a fixed amount of time or permenantly.
        * **S3 Glacier Vault Lock:** Applies the WORM model as a policy to the entire archive (via a JSON document) and has a 24-hour "locking" period to reverse it (if not reversed, the WORM policy applies indefinitely for the specified amount of time set in the policy (JSON) that not even the root user can change).  

### Amazon FSx
   * **Amazon FSx offers fully managed third-party file systems for either Windows File Server (for Windows-based apps) or Lustre (for high performance computing (HPC) workloads).**
   * #### FSx for Windows:
     * Provides a fully managed Windows file-systems, included high-availability and multi-AZ setups.
     * On-Prem Data Centers can access vis VPN or AWS Direct Connect.
   * #### FSx Lustre:
     * High performance file system optimized for fast processing of workloads suchs as machine learning, HPC, video processing, financial modeling, and electronic design automation (EDA).
     * Lustre works natively with Amazon S3 - presents S3 objects as files. (Remember: FSx and S3 are **Lustre**!).
     * On Prem Data Centers can access vis VPN or AWS Direct Connect (common use case: cloud bursting).
    
### AWS Storage Gateway
   * **This service is used to connect on-prem infrastructure and applications to Cloud storage (aka: hybrid cloud storage service).**
   * #### Use Cases:
     *  Moving backups to the Cloud
     *  Using on-prem files shares backed by Cloud storage
     *  Low latency access to AWS services from on-prem applications
     *  Disaster recovery
   * #### Types of Gateways:
     * **File Gateway:** gateway from on-prem to Cloud file-based storage systems.
     * **Volume Gateway:** gateway from on-prem to Cloud block-based storage systems.
     * **Tape Gateway:** gateway from on-prem to Cloud file-based or block-based protocols via popular backup software as if it were a "tape" (but really, it's pulling objects from S3 in the Cloud).
    
### AWS Elastic Disaster Recovery Service (DRS)
   * ### AWS DRS
     * **Application Recovery:** designed to protect and recover critical applications quickly at low cost.
     * **Reduced Downtime:** ensures minimal downtime in the event of app failure by recoving apps in AWS.
      * Applications can be recovered from physical servers in your data center.
      * Applications can be recovered from VMs.
      * Applications can be recovered from the Cloud (AWS, Google, Azure, Oracle).
   * ### Recovery Process
     * A **staging area** is built where your data is continuously replicated in a "staging subnet" in the VPC (replication must be occurring BEFORE the disaster).
     * A **recovery subnet** is also established where data and applications replicated in the "staging area" are stored in a private subnet where your backups are held and can be tapped back to the primary site during a diaster to minimize downtime.
     * **Conversion:** AWS DRS will automatically convert your on-premises (or Cloud) data into AWS-compatible format so your data can be recovered during a disaster.          

### AWS Snow Services
   * #### AWS Snowball
     * Snowball is a service that allows you to move hundreds of terabytes or petabytes of data from your on-prem data center to Amazon S3 (and vice versa) (import/export into/out of S3).
     * Snowball uses a secure storage device for physical data transportation.
     * **AWS Snowball Client** is software installed on a local computer that identifies, compresses, encrypts, and transfers data into the Cloud.
     * Snowball is used for bulk data transfer, for edge storage, and for edge compute. 
   * #### AWS Snowmobile
     * A literal shipping container full of storage (up to 100PB) and a truck to transport it.
   * #### AWS Snowcone
     * The smallest device in the service best suited for outside data centers.  



