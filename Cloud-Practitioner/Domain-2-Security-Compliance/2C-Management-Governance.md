# Domain 2: Security and Compliance
# (2C: AWS Cloud Management and Compliance)

# High-Level
### 🏛️ 1. Centralized Management & Governance

| Service | Primary Function | Key Architectural Hook |
| :--- | :--- | :--- |
| **AWS Organizations** | Centralizes management of multiple accounts. | **SCPs** and **Consolidated Billing**. |
| **AWS Control Tower** | Sets up a secure, multi-account **Landing Zone**. | Automatic governance/security guardrails. |
| **AWS Service Catalog** | Manages catalogs of approved IT services. | Deploy only pre-approved "stacks." |
| **AWS Systems Manager** | Unified UI for operational management. | **Patch Manager**, **Session Manager**, **Run Command**. |

### 🛡️ 2. Monitoring, Compliance & Health

| Service | Purpose | Exam Keyword |
| :--- | :--- | :--- |
| **AWS Config** | Audits/records resource configurations. | **"Compliance Audit,"** **"Desired State."** |
| **AWS Trusted Advisor** | Guidance for Best Practice optimization. | **"Cost/Security/Performance" checks.** |
| **Personal Health DB** | Specific impact to YOUR account/resources. | **"Personalized view,"** **"Impacts YOU."** |
| **Service Health DB** | Global status of all AWS services. | **"Public status,"** **"General Availability."** |

# Deep Dive

## AWS Organizations
  * #### Overview
    * AWS Organizations is created with an **AWS Management** account - this account becomes the root of the hierarchy of accounts you either create or invite into the AWS Organization account you've created.
    * AWS Organizations allows you to consolidate multiple AWS accounts into **organizational units (OU)** that you create and centrally manage (re: "Production" accounts go into a "Production OU", "Development" accounts go into a "Development OU", etc.).
    * **Service Control Policies (SCPs):** are applied to OUs and can control tagging as well as available API actions.
  * #### Feature Sets
    * **Consolidated Billing:** allows you to consolidate the bills from all of the accounts into one bill and allows you to take advantage of discounts from aggregated services.
        * **Paying Account:** an independent account that exists only to pay the consolidated bill and does not have access to the root account or the OUs.
        * **Linked Accounts:** independent accounts that get linked the to Paying Account so the Paying Account is able to consolidate the bill. 
    * **All Features:** allows you to access all of the features on top of the Consolidated Billing feature available through AWS Organizations including SCPs.  

## AWS Control Tower
  * #### Overview
    * AWS Control Tower simplifies the process of creating multi-account environments (called **Landing Zones**).
    * AWS Control Tower sets up security, governance, and compliance guardrails for you.
    * Integrates with other AWS services including:
      * AWS Organizations
      * Organizational Units (OUs)
      * Service Control Policies (SCPs)
      * AWS Config
      * AWS CloudTrail
      * Amazon S3
      * Amazon SNS
      * Amazon CloudFormation
      * AWS Service Catalog
      * AWS SSO

## AWS Systems Manager
  * #### Overview
    * Manages many AWS services such as EC2, S3, RDS, etc.
    * Components of the AWS System Manager:
      * **Automation:** uses documents to run automations.
      * **Run Command:** allows you to run commands on EC2 instances
      * **Inventory:** gathers inventory information
      * **Patch Manager:** manage patching schedules and installation
      * **Session Manager:** connect securely without using SSH or RDP
      * **Parameter Store:** store secrets and configuration data securely      

## AWS Service Catalog
  * #### Overview
    * Allows organizations to create and manage catalogs of IT services that are approved for use on AWS.
    * Allows you to manage commonly deployed IT services such as VMs/EC2 images, servers, software, DBs, and mutli-tiered app architectures (microservices).
    * Enables users to quickly deploy only the approved IT services that they need.

## AWS Config
  * #### Overview
    * AWS Config allows you to audit the configurations of your resources and can be used for compliance reasons.
    * Allows you to validate if your resources are configured a certain way (compares current configuration to desired configuration).
    * Resources can report in their configuration status to AWS Config, which then records information in S3, notifies via SNS, and triggers CloudWatch events/alerts.
    * AWS Config can also leverage Lambda functions to conduct configuration remediation to automatically configure your resources to the desired configuration state.
   
## AWS Trusted Advisor
  * #### Overview
    * AWS Trusted Advisor is an online resource that helps you optimize your AWS resources:
      * Reduce cost (advises you on **cost optimization** - one of the 6 pillars of the Well-Architected Framework).
      * Increase performance (advises you on how to get the best **performance** from your services including guarding against **fault tolerance**).
      * Improve secruity (advises you on how to optimize the **security** of your services).
    * AWS Trusted Advisor provides real-time guidance to help you provision your resources following **best practices** (remember the term "best practices" for the exam!).
   
## AWS Health API
  * #### AWS Personal Health Dashboard
    * Provides alerts and remediation guidance when AWS is experiencing events that may impact **YOU**.
    * Provides a personalized view into the performance and availability of the AWS services underlying your AWS resources.
    * Provides proactive notifications of scheduled events and downtime allowing you to plan accordingly.  
  * #### AWS Service Health Dashboard
    * Provides current status of AWS services.
    * NOT personalized to you.
   

 

