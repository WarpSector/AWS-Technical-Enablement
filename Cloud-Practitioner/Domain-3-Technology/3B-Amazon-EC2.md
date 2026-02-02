# Domain 3: Technology
# (3B: Amazon Elastic Compute Cloud (EC2) Service)

## Amazon Elastic Compute Cloud (EC2)

### Executive Summary
Amazon EC2 provides resizable, virtualized compute capacity in the cloud, allowing users to launch instances with full control over the operating system and software stack. By utilizing Auto Scaling Groups and Elastic Load Balancing, EC2 creates a self-healing, "loosely coupled" architecture that can scale horizontally to meet demand while maintaining fault tolerance.

### EC2 Overview and Basics
  * #### Compute is the engine of the Cloud.
    * This is where all the compute processing (CPU) of your Cloud applications and web services happen.
    * Amazon Elastic Compute Cloud (EC2) is an AWS service that lets you easily use compute resources in the Cloud.
    * You run virtual instances/machines in the Cloud when you use Amazon EC2.
    * Amazon EC2 instances can run Linux, Windows, or MacOS.
    * **You only pay for what you use.**
  * #### Elasticity
    * The instances are **elastic** allowing you to provision additional servers when needed and terminate unused or underutilized servers when you no longer need them.
      * **NOTE:** Scaling is NOT automatic by default. You will need to set the rules in the AWS Management Console and configure the Auto-Scaling Groups (ASGs) to scale your EC2 instances when necessary.
    * The ASGs will scale **horizontally** since this is not disruptive.
    * Vertical scaling is available to perform manually, but it's disruptive (requires you to stop/start the instance) and defeats the purpose of the Cloud's elasticity (horizontal scaling and distributing the workload).
  * #### Instance Types
    * You can select different instance types with varying combinations of CPU, memory, storage, networking, and OS:
      * **Family/Generation Examples:**
        * Main/General Purpose (M)
        * Compute Intensive (C)
        * Graphics Intensive (G)
        * Data (D)
        * RAM (R)
        * Cheap (T)
        * Fast (F)
        * High Memory (U)
        * High Compute + Memory (Z)
        * High Disk Throughput (H), etc.
      * **Volume/Size (Just remember the next size is double the previous size):**
        * Nano
        * Micro
        * Small
        * Medium
        * Large
        * X-Large
        * 2X-Large
  * #### Shared Responsibility
    * AWS manages the physical underlying infrastructure (host) and the virtualization layer (hypervisor), while the users manage their VMs and resources in the Cloud.
    * EC2 is IaaS: AWS manages the underlying hardware and you manage everything else from the OS (platform) and up.
    * EC2 instances "sit" inside the public or private subnet within the AZ, which "sits" within the (regional) VPC.

### Virtualization
  * #### EC2 works via virtualization technology.
    * The physical server uses virtualization software, which creates a **hypervisor** over the physical server (enabled by using virtualization software such as Zen, Microsoft V, KVM, etc.).
    * The hypervisor creates a layer of "abstracton" between the physical server and the virtual server by allocating resources from the physical hardware into the virtual machine (VM) including the memory, storage, networking, and CPU power.
    * Virtual Machines are also referred to as virtual servers or virtual instances.
  * #### Benefits of Virtualization 
    * Virtualization allows you to run multiple VMs on the same physical server.
    * Virtualization allows portability allowing you to move the VM to different physical servers (it resolves the limitation of having the OS tied to a specific piece of hardware).
    * Virtualization allows scaling - allowing you to spread your VMs across multiple physical servers.

### EC2 Benefits
  * #### Elastic
    * Increase/decrease capacity to meet demand fluctuations and web traffic spikes.
  * #### Reliable
    * Highly reliable environment where replacement instances can be provisioned rapidly with **Regional** service-level agreements (SLAs) of 99.99% **if architected for High Availability** (i.e., EC2 instances are spread across multiple AZs and have an elastic load balancer along with an ASG to distribute and handle the load while the first failed EC2 instance is repaired and brought back online (this is also known as "self-healing" architecture: High Availability (HA) + Fault Tolerance (FT)).
  * #### Inexpensive
    * Amazon passes on the benefit of economies of scale to the users.
  * #### Integrated
    * EC2 instances are integrated with a wide variety of AWS services (including S3, RDS, and VPC) so you can build complete services within the Cloud.
  * #### Secure
    * EC2 works inconjunction with the VPC to provide a secure location with an IP address range you can configure to allow access to your EC2 (in combination with network security measures like Network Access Control Lists (Network ACLs) protecting your subnet boundaries, Web Application Firewalls (WAFs) protecting your application load balancers (ALBs), and Security Groups protecting the EC2 instances themselves).

### States and Scaling
   * #### Stateful vs. Stateless
     * **Stateless:** Does not record any information, no state recorded for the session, does not "remember" you.
     * **Stateful:** Records information, creates a session record for the session, "remembers" you.
   * #### Scaling Up (Vertical)
     * Adding CPU, RAM, and more resources to the server or machine itself.
     * Also called **vertical** scaling.
   * #### Scaling Out (Horizontal)
     * Adding more instances of the EC2 server and/or application.
     * You are spreading the workload across multiple instances (and balance the load with ELBs).
     * Also called **horizontal** scaling.
   * #### Auto-Scaling
     * Amazon EC2 (as well as other compute services such as ECS and EKS) is capable of **auto-scaling** where instances can automatically be launched or terminated in response to demand.
     * Auto-Scaling is what maintains **high availability** and **scales** capacity.
     * Auto-Scaling is **horizontal** (scales out).
     * Auto-Scaling is **elastic** can scale out for added capacity and can terminate instances when demand drops.
     * #### Types of Auto-Scaling:
       * **Manual:** make changes to the ASG manually.
       * **Dynamic:** automatically scales based on demand.
       * **Predictive:** uses machine learning to predict when to scale.
       * **Scheduled:** scales based on a scheduled scaling policy you set.  
     * #### Integrates with many AWS services including:
       * **CloudWatch** for monitoring and scaling (EC2 instances are constantly sending information to CloudWatch such as metrics, CPU utilization, etc.).
       * **Elastic Load Balancing (ELB)** for distributing the connections and workload between different instance.
       * **EC2 Spot Instances** for cost optimization.
       * **Amazon Virtual Private Cloud (VPC)** for deploying instances across AZs.
   * #### Auto-Scaling Groups (ASGs)
     * Auto-Scaling Groups automatically add or terminate EC2 instances in response to demand.
     * You can define the number of instances you want to run at a steady-state in an ASG.
     * EC2 instances are sending metrics to CloudWatch (known as "health checks") --> CloudWatch monitors for CPU utilization --> CloudWatch sends a signal to the ASG --> ASG either scales out or terminates instances based on the metrics reported by CloudWatch --> ASG also signals the ELB so the ELB knows if there are additional instances to route traffic to (ELBs also send "health checks" to CloudWatch and ASG).
   * #### Scaling Policies
     * **Target Tracking:** attempts to keep the instance group at or close to the targeted metric/utilization.
     * **Simple Scaling:** adjusts the instance group size based on a specific metric.
     * **Step Scaling:** adjusts the instance group size based on a specific metric, but the adjustments vary based on the size of the alarm breach.
     * **Scheduled Scaling:** adjusts the instance group size at a specific time.   

### Amazon Machine Image (AMI)
  * #### Overview
    * When you launch an EC2 instance, you must select an Amazon Machine Image (AMI) that specifies the type of OS you want to run, the resources you want to allocate to the VM, and the data and applications you want in the VM.
    * Think of AMIs as the "blueprint" for the EC2 instance you are launching. If the EC2 instance is the "house", then the AMI is the "furniture".
    * AMIs are "region locked" meaning they only exist in the Region they are created. You must copy them to other Regions if you want to launch a specific AMI in a different Region.
  * #### There are 3 types of AMIs
    * **Community AMIs:**
      * AMIs that are created by other AWS users, which you can access from the community and download for free.
    * **AWS AMIs:**
      * AMIs built by Amazon and available for purchase since they come packaged with additional licensed software.
    * **Custom AMIs:**
      * AMIs you've built yourself.
      * Best practice for Custom AMIs is to build them and install all the software, data, applications, and configurations you want before you create the snapshot.
      * This way, if you need to load it into a different EC2 instance, you have the entire VM snapshotted and can easily upload it in one fell swoop (versus having to manually install the software and apps and manually configuring the instance each time you want to boot up a new one).

### EC2 Pricing
   * **There are different pricing models available:**
       * #### On-Demand Instances:
         * Good for users that need flexibility without any upfront payment or long-term committment (true "pay-as-you-go" pricing model).
         * Good for users with unpredictable and spiky web traffic or unpredictable workloads that need to be processed with zero interruption.
         * Good for users developing and testing applications on EC2 for the first time.            
       * #### Reserved Instances (RIs):
         * Good for users that have steady-state or predictable workloads.
         * Good for users that need reserved capacity for their apps and workloads.
         * Standard Reserve Instances (RIs) provide up to 75% off the On-Demand price. 
         * Users can make upfront payments to reduce their computing costs even further.
         * RIs can be scheduled and launched within a time window you reserve, which allows you to match your reserved capacity to a predictable recurring schedule.
         * **Convertible Reserved Instances:** These are RIs that allow you to change your AZ, instance size, family, or OS.
      * #### Spot Instances:
         * Good for users who have flexible start and end times, because Spot Instances can be interrupted if other users demand additional capacity.
         * Good for users who have an urgent need for a large amount of additional compute capacity.
         * If Amazon interrupts or terminates your instance, you don't pay. If you terminate, you pay for the hour.
      * #### Dedicated Hosts:
         * These are dedicated **physical** servers reserved only for you.
         * The user has control over which instances to deploy on the host, though the user can only deploy one instance size and type on the host.
         * This allows complete isolation. No one else will use the underlying server you are hosting your instances on so you can create all the VMs you want.
         * Good for users with regulatory compliance and/or licensing requirements as it allows visibility into the hardware layer (to have telemetry on core sockets and host affinitiy).
         * This is the most expensive option.
      * #### Dedicated Instances:
         * This is Dedicated Hosts with (dedicated) instances, but without the insight into the hardware layer that you get when choosing Dedicated Hosts (insights into things like core sockets and host affinity).
         * Billing is per instance.
      * #### Savings Plans:
         * Flexible pricing model that saves up to 72% on your AWS compute usage.
         * Offers lower pricing on EC2 instance usage regardless of family, OS, size, or type.
         * Savings also apply to AWS Lambda and AWS Fargate.          

### EC2 Metadata and User Data
  * #### User Data
    * User data is a form of **bootstrapping** - you provide a script that the instance automatically executes when it first boots up without you having to manually perform whatever function the script is supposed to execute after the instance boots up (think of it as a "To Do" list for the server to carry out the first few seconds it "wakes up").
  * #### Metadata   
    * Metadata is data about the EC2 instance that can be used to configure and manage the running instance (available at http://169.254.169.254/latest/meta-data
    * **NOTE:** Metadata and User Data are NOT encrypted.

### AWS Family of Compute Services
  * #### While EC2 is the "grandfather" of Amazon's compute cloud services, other compute services are available:
   * #### AWS Batch:
      * Enables developers, scientists, and engineers to easily and efficiently run hundreds of thousands of batch computing jobs on AWS.
      * AWS Batch dynamically provisions the optimal quantity and type of compute resources for the job being processed.
      * With AWS Batch, you package the code to run the jobs, specify any dependencies, and then submit the batch job via the AWS Management Console, AWS Command Line Interface (CLI), or an AWS Software Development Kit (SDK).
   * #### Amazon Lightsail:
      * Best for users who do not have technical expertise with AWS as this service makes it very easy for them to provision compute services.
      * Amazon Lightsail provides compute, storage, and networking capacity and capability to deploy and manage websites, webservices, and databases in the Cloud.
      * Amazon Lightsail includes a VM, SSD-based storage, data transfer, DNS management, and a static IP.
      * Lets you deploy load balancers and attach block storage.
      * Best suited for projects that require a few dozen instances or less: good for blogs, websites, web apps, e-commerce, etc.
      * Not as powerful as EC2 instances.
      * Has a simple management interface.  
   * #### Amazon Elastic Container Service (ECS):
      * Amazon ECS is used to run Docker Containers in the Cloud.
        * **Recall:** Containers are application code, dependencies, and system libraries all packaged up and designed to run in various runtime environments.
        * **Recall:** Containers are **isolated** - even though the container shares the OS kernel, it will not interact with the OS until it is commanded to.
        * **Recall:** Containers are **portable** - because the runtime environment is standardized, the Container will run exactly the same way as it does on your laptop on any other machine or on AWS Fargate or EC2 instance.
        * **Recall:** Containers are **lightweight** - because they contain the OS kernel and not the entire OS (like a VM does), they are capable of starting up in **seconds** rather than minutes.
        * **Recall:** **Docker** is used to build Containers - the process of taking the application, code, scripts, dependencies, and system libraries and packaging them up (sometimes called **"Dockerizing"**).
      * ECS Containers are known as **tasks** that run in the Cloud.
      * The task definitions are written in JSON (the Amazon "native way").
      * The way it works: Container (The Box) --> Task Definition (JSON "Instruction Manual" to run the Container/Task) --> Task (Container is run by Amazon ECS, the "robot" that reads the JSON instructions and runs the task).
   * #### Container Launch Types:
      * **EC2:** You manage the instances running the tasks yourself. You spin up the instance, provision what you need yourself, and then bring the Containers in yourself to run them.
      * **AWS Fargate:** An AWS managed service that manages the underlying compute, cluster, and scaling to run the Container - also known as **SERVERLESS CONTAINERS** - where you don't have to manage the EC2 instance or provision the resources yourself to run the Container:
        1) You deploy ECS to run the Container and choose Fargate to do it.
        2) Fargate provisions the resources needed to deploy the Container and pulls the Container image from the Elastic Container Registry (ECR) (a private Container image registry where you can store snapshots of your Containers).
        3) ECS then runs the Container and monitors the task - if the Container fails, ECS will use Fargate to set up the resources, pull the Container image, and then will run the Container again.
   * #### Amazon Elastic Kubernetes Service (EKS):
      * Amazon EKS is used to run Containers where the "instruction manual" is written in Kubernetes (YAML).
      * Kubernetes is the "universal instruction manual" that lets the Container run on AWS, Azure, or Google Cloud (multi-Cloud deployable).
      * EKS is simply Amazon's service to orchestrate the Container with a Kubernetes (K8s) manual.
      * It follows the same flow as ECS, except for one small change: Container (The Box) --> Task Definition (Kubernetes/K8s (YAML) "Instruction Manual" to run the Container/Task) --> Task (Container is **run by Amazon EKS** for K8s instead of Amazon ECS).
   * #### Amazon Lambda:
      * **SERVERLESS COMPUTING** technology that allows you to run code without having to provision the resources yourself.
      * AWS Lambda executes code only when needed and scales automatically.
      * You pay only for the compute time you consume (you pay nothing when your Lambda code is not running).
      * **Benefits of Lambda:**
        * No servers to manage.
        * Continuous scaling.
        * Billing down to the millisecond.
        * Integrates with almost all AWS services.
      * **Primary Use Cases:**
        * Real-time file processing.
        * Data processing.
        * Can be triggered when certain events occur (e.g., a user uploads a large 4k image to an S3 bucket --> the S3 bucket triggers the Lambda code to resize the image into a thumbnail for the display --> Lambda triggers, creates the thumbnail, saves it in another S3 bucket and then shuts down --> you are billed for only for the 200ms it took for Lambda to execute).
        * **NOTE:** Lambda code can only run events for a maximum of **15 minutes.**
  * #### Amazon Elastic Beanstalk:
       * Fastest and simplest way to get web applications up and running on AWS.
       * Developers upload their application code --> AWS Elastic Beanstalk provisions all necessary resources to run the application and scale it as necessary (re: EC2, ASG, ECS/EKS, ELB).
       * Elastic Beanstalk is **PaaS**, because you are operating at the platform level. Amazon is running the infrastructure and the platform for you, so you can focus on testing and deploying applications and code. 

## Summary Tables

### 🏛️ AWS Compute Service Type Comparison

| Service | Compute Type | Management Level | Scaling Style | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Amazon EC2** | Virtual Machines (IaaS) | **High** (You manage OS) | Manual or ASG | Legacy apps, full OS control. |
| **AWS Lambda** | Serverless Functions | **Zero** (No servers) | Automatic (per event) | Event-driven code, 15-min tasks. |
| **AWS Fargate** | Serverless Containers | **Zero** (No servers) | Automatic (per task) | Microservices without managing EC2. |
| **Amazon ECS / EKS**| Container Orchestration| **Medium** (Orchestration)| Managed Clusters | Managing thousands of Docker boxes. |
| **Elastic Beanstalk**| Platform as a Service (PaaS)| **Low** (Upload & Run) | Automated | Rapid web app deployment. |
| **Amazon Lightsail** | Virtual Private Server (VPS)| **Very Low** (Simple UI) | Fixed / Predictable | Small websites, blogs, dev labs. |
| **AWS Batch** | Batch Computing | **Low** (Job-based) | Dynamic | Thousands of scientific/data jobs. |
---
### 🛡️ EC2 Pricing Model Comparison

| Pricing Model | The "Vibe" | Best Use Case | Cost Benefit |
| :--- | :--- | :--- | :--- |
| **On-Demand** | Flexible & Spiky | New apps, Dev/Test, Unpredictable traffic. | No commitment; pay-per-second. |
| **Savings Plans** | Long-term Commitment | Consistent spend across EC2, Fargate, Lambda. | **Up to 72% off** (Flexible by family/region). |
| **Reserved (RI)** | Predictable & Steady | 24/7 databases or stable legacy apps. | Up to 75% off (Region/AZ specific). |
| **Spot** | High-Risk / Low-Cost | Batch jobs, stateless apps, "Interruptible" work. | **Up to 90% off** (Cheapest possible option). |
| **Dedicated Host** | Full Control | Compliance, strict licensing (e.g., bring your own). | Most expensive; physical server isolation. |

---

### 🏗️ Resilience Deep Dive: High Availability (HA) vs. Fault Tolerance (FT)

**Core Philosophy:** In the cloud, we don't try to prevent failure; we architect to survive it. High Availability is about **recovering fast**, while Fault Tolerance is about **never stopping.**

| Feature | High Availability (HA) | Fault Tolerance (FT) |
| :--- | :--- | :--- |
| **User Experience** | A brief "hiccup" or refresh (seconds). | **Zero disruption** (invisible to the user). |
| **Mechanism** | **Self-healing** (ASG) + **Failover** (ALB). | **Active-Active** redundancy (1:1 duplicates). |
| **Design Goal** | Resilience via **Automated Recovery**. | Resilience via **Total Redundancy**. |
| **Infrastructure** | Usually **Multi-AZ** (Single Region). | Often **Multi-Region** (Global). |
| **Cost** | Efficient (Pay for what you need). | **Expensive** (Paying for 2x everything). |
| **Analogy** | A car with a **spare tire** in the trunk. | A car with **dual engines** running at once. |

---

### 🛡️ The "Failure" Decision Matrix

* **Self-Healing (ASG):** The system detects a failed instance and replaces it. (Maintains *Quantity*).
* **Failover (ALB/Route 53):** The system detects an unhealthy path and reroutes traffic. (Maintains *Connectivity*).
* **Fault Tolerance:** The system has no single point of failure; if one half dies, the other half is already processing the load. (Maintains *Continuity*).

**Exam Tip:** If the question mentions **"Zero Downtime"** or **"Mission Critical,"** lean toward Fault Tolerance. If it mentions **"Minimize Downtime"** or **"Cost-effective,"** lean toward High Availability.
