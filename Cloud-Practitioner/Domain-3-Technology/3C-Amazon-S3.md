# Domain 3: Technology
# (3C: Amazon Simple Storage Service (S3))

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
   
  
