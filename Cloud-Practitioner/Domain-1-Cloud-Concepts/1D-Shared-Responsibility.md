# Domain 1: Cloud Concepts
# (1D: The Shared Responsibility Model)

# High-Level
### 🏛️ The Shared Responsibility Master Table

| Component | IaaS (e.g., EC2) | PaaS (e.g., RDS, S3, Lambda) | SaaS (e.g., Chime, Marketplace) |
| :--- | :--- | :--- | :--- |
| **Physical / Hardware** | AWS 🔒 | AWS 🔒 | AWS 🔒 |
| **Virtualization Layer** | AWS 🔒 | AWS 🔒 | AWS 🔒 |
| **OS Patching** | **YOU 👤** | AWS 🔒 | AWS 🔒 |
| **Runtime / DB Engine** | **YOU 👤** | AWS 🔒 | AWS 🔒 |
| **App Configuration** | **YOU 👤** | **YOU 👤** | AWS 🔒 (mostly) |
| **IAM / Access Control** | **YOU 👤** | **YOU 👤** | **YOU 👤** |
| **Customer Data** | **YOU 👤** | **YOU 👤** | **YOU 👤** |

# Deep Dive

## AWS Responsibility vs. Customer Responsibility
  * ### AWS is responsible for security **OF** the Cloud.
    * #### Physical Infrastructure and Hardware in:
        * Regions
        * Availability Zones (data centers in the AZs)
        * Edge Locations (regional and edge)
    * #### Virtualization
        * Hypervisor
        * Compute hardware
        * Storage hardware
        * Networking hardware
        * Database hardware    
  * ### Customers are responsible for security **IN** the Cloud.
    * #### **Infrastructure as a Service (IaaS)**
        * Client-side data encryption
        * Server-side encryption (EC2)
        * Networking traffic encryption
        * OS configuration and patching
        * Network configuration
        * Firewall configuration
        * Applications
        * Code
        * Identity and Access Management (IAM)
    * #### **Platform as a Service (PaaS)**
        * Applications
        * Code
        * IAM
    * #### **Software as a Service (SaaS)**
        * Customer Data
        * IAM
    * #### Remember: The customer is ALWAYS responsible for the DATA. AWS is always responsible for the PHYSICAL INFRASTRUCTURE. 
    

