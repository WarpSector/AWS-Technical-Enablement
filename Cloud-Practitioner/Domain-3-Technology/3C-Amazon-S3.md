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
    * Files on local hard drives (block-based storage containers) are accessed in a file hierarchy.
    * These are essentially networked block based storage drives shared over a network.
    * Hardware known as **network attached storage servers (NAS)** are "mounted" to the network and "shares" files over the network.
    * Computers can interact with network shared files the same way local files can be accessed.
  * #### Object-Based Storage (Object):
    * Users upload files into an **object based storage container** over the internet using HTTP/HTTPS protocols (e.g., GET (Read), PUT (Update), POST (Create), SELECT (Select), DELETE (Delete)).
    * This storage type is called a "bucket" because there is no file hierarchy - think of all the files (objects) as grains of sand into a bucket (though there is a way to mimic a hierarchy via S3).
    * Any type of file can be uploaded into a bucket.
    * Objects can be managed by developers and services can be integreated via **REpresentational State Transfer (REST)** API so the files can be managed via code.
      * **Recall:** Rest APIs are a set of rules for how computers should "talk" to each other over the internet using HTTP protocol. 
    * This type of storage is scalable and very cost-effective.
   
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

### Amazon Elastic File System (EFS)
   * Amazon EFS is a shared file system that can be shared across multiple EC2 instances.
   * An EFS can be Regional and shared to different AZs.
   * **Regional EFS** are connected to a **mount target** (efs/mnt) inside an AZ and then the EC2 instances inside that AZ connect to that mount target to access the Regional EFS.
     * **NOTE:** The connection protocol to mount EC2 to mount targets is **network file system (NFS)**, which is only available in **Linux**.
     * You can use NFS protocol with other OS, but Amazon only supports Linux.
   * You can also deploy an EFS in a single AZ through **one zone** mount targets where the EFS --> Local AZ MTN --> Local AZ EC2 Instances are shared within that AZ.
     * You can connect to mount points in different AZs if you need to.
   * **Data Consistency:**
     * Regional EFS are durable, meaning the files are copied across the AZs they are mounted to.
     * **File Locking:** NFS protocol allows read/write locking to ensure data consistency.    
   * **Storage Classes:**
     1. **EFS Standard:** uses SSD for low-latency performance.
     2. **EFS Infrequent Access (IA):** useful if you don't need to access the files regularly and is a more cost-effective option than the EFS Standard.
     3. **EFS Archive:** used for archival purposes and an even cheaper option than EFS IA.
   * EFS can be replicated across Regions for disaster recovery (DR) purposes.
     * **Note:** You can create a mount point in the Region the EFS is replicated (re: 2nd Region), however, that 2nd mount point will only allow **read-only** access to the EFS (the only time it won't be read-only is if your access fails over to the 2nd Region).
   * EFS can also be connect to an on-prem data center (from outside the Cloud) via Amazon Direct Connect or a VPN connect using the Linux NFS protocol.
   * **Automatic Back Up:** EFS uses **AWS Backup** for automatic file system backups.
   * **Performance Options:**
     * **Provisioned Throughput:** you set the performance and throughput you want regardless of how large the EFS is.
     * **Bursting Throughput:** throughput scales with the amount of storage and can burst to higher performance when needed.
    
  
