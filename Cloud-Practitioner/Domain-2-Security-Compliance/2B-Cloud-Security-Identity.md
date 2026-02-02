# Domain 2: Security and Compliance
# (2B: Cloud Security and Identity)

# High-Level
### 🛡️ AWS Security & Governance Master Table

| Service | Category | Core Mnemonic |
| :--- | :--- | :--- |
| **CloudTrail** | Auditing | **"Who did what?"** (API Log) |
| **CloudWatch** | Monitoring | **"How is it performing?"** (Metrics) |
| **Config** | Compliance | **"Is the config correct?"** (History) |
| **Artifact** | Compliance | **"Show me the certificates"** (SOC/PCI) |
| **RAM** | Resource | **"Share resources, not users"** (VPC Subnets) |
| **Security Hub** | Dashboard | **"The Central Command Center"** (Aggregator) |

---

### 🔑 Encryption & Secrets Cheat Sheet
* **KMS:** Managed Service, Shared Hardware (FIPS 140-2 L2).
* **CloudHSM:** Dedicated Hardware, Customer Managed (FIPS 140-2 L3).
* **Secrets Manager:** Rotates keys automatically (e.g., RDS passwords).
* **Parameter Store:** Stores plain text or encrypted strings (cheaper).

### 🚨 Threat Detection Timeline
1. **Inspector:** Scans for unpatched software (EC2/Lambda/ECR).
2. **GuardDuty:** Uses ML to find "weird" behavior (API/DNS/VPC Logs).
3. **Detective:** Investigates a GuardDuty finding to find the "Why."
4. **Macie:** Finds Social Security numbers (PII) in S3.

### 🛑 DDoS & Firewalls
* **WAF:** Blocks specific bad requests (Layer 7).
* **Shield Standard:** Free DDoS protection for everyone.
* **Shield Advanced:** $3,000/mo, DDoS insurance, 24/7 Support.
* **Firewall Manager:** Manages WAF/Shield rules across **Multi-Accounts**.

# Deep Dive

## Identity Federation
  * ### Overview
    * Ultimately, identity federation is **single sign-on (SSO)** for the Cloud.
    * It allows you to use your existing credentials from one system (like your company’s Active Directory, or even your Google/Facebook account) to access AWS resources without having to create a separate IAM user for every person.
  * ### IAM - Security Assertion Markup Language (SAML) 2.0 Identity Federation
    * #### SSO Flow using SAML 2.0
      1. User logins into their on-prem systems --> Windows Active Directory (this is an "Identity Store" since it stores user IDs) --> User can access on-prem applications and resources after logging in through Windows Active Directory (AD).
      2. What if the user now wants to access a resource on the AWS Cloud? --> Before that can happen, the user must go through an **identity provider (IdP)** like Windows Active Directory Federation Server (ADFS) --> ADFS allows SSO into the AWS VPC.
      3. Before the user can access resources in the AWS VPC, the AFDS uses IdP that will let the user access the resource --> The IdP goes to the **Amazon Security Token Service (STS)** --> STS provides a temporary security token to the IdP --> User can then access resources in the AWS VPC.
  * ### IAM - Web Identity Federation
    * #### SSO Flow using Web Identity Federation
      * The flow is similar to SMAL 2.0, except the user authenticate using either their Amazon, Apple, Google, or Meta account to access resources.
      * So the user uses a **social identity provider (IdPs)** to access AWS Cloud resources from their mobile device.
      * The IdPs talks to the STS, which returns the temporary token to the user's device letting them access the resource in the AWS VPC.
      * Amazon recommends using **AWS Cognito** for web identity federation.
  * ### IAM - Identity Center
    * #### Overview
      * This is AWS's successor to AWS Single-Sign On.
      * This service enables centralized management of permissions and SSO.
      * You can store identities within Identity Center and they can be identities within Identity Center itself, Azure AD, or any providers using the SAMLE 2.0 protocol.
      * Identity Center can integrate with AWS Organizations to connect to different accounts created throuhg AWS Organizations.
  * ### AWS Cognito
    * #### Overview
      * Cognito can be used for Web Identity Federation by either storing the account in **Cognito User Pool** or by retieving IdSp from the user's Amazon, Apple, Google, or Meta account.
      * The **Cognito User Pool** is the service responsible for authenticating the user.
      * From the Cognito User Pool, a JSON Web Token (JWT) is issued, which then goes to the **Cognito Identity Pool**, which then talks to STS to issue the temporary security token for the session --> User can access resources in the VPC.

## AWS Directory Service
  * ### Overview
    * A suite of products with various options for hosting a directory service in the Cloud or connecting to another directory service.
    * AWS Managed Microsoft Active Directory is one of such directory services you can connect to via AWS Directory Service - this is a fully managed service by AWS on AWS infrastructure.  

## AD Connector
  * ### Overview
    * You can connect a different directory (such as MS AD) via VPN to an AD Connector.
    * The AD Connector then connects to various AWS services.
    * Rather than having any director service running in the Cloud, we can just connect them to the Cloud through the AD Connector.
    * AD Connector eliminates the need for directory synchronization as well as the cost and complexity of hosting a federation structure.
    * Connects your on-prem to AD to AWS.

## Secrets
  * ### Systems Manager Parameter Store
    * Provides hierarchal storage for data management and secrets management.
    * Highly scalable, available, and reliable/durable.
    * You can store passwords, database strings, and license codes as **parameter values**.
    * You can store parameter values as plain text (unencrypted) or as cipher text (encrypted).
    * You then reference parameter values by using the unique name that you specified when you created the parameter.
    * You don't have to store the info in your code, you can store it in a secure, centralized place (you might have lots of apps and can change/store parameter values in one place).
      * #### Example:
        * An EC2 instance needs to query RDS --> In order to authenticate and retrieve information from the DB, it makes an API call to the **Parameter Store** --> The Parameter Store holds a DB connection string --> EC2 instance retrieves the string --> EC2 authenticates and is able to query RDS.   
  * ### AWS Secrets Manager
    * Similar to Parameter Store, but has some additional features, and costs more in some cases.
    * Allows native and automatic rotation of access keys, passwords, etc.
    * Has central auditing for rotation and fine-grained permissions. 

## Encryption
  * ### Overview
    * Data is encrypted **in transit** (as it moves over the network) and **at rest** (while its stored).
    * **Encryption In Transit:** User is connecting to an application over the internet using the HTTPS protocol, which utilizes the **SSL/TLS** certificate to encrypt data **IN TRANSIT**.
    * **Encryption At Rest:** User uploads unencrypted object into S3 and S3 automatically encrypts the object using a **DATA ENCRYPTION KEY** to encrypt the object as it's being written into the S3 bucket.
  * ### Asymmetric Encryption
    * Also known as "public key cryptography".
    * Messages encrypted with a **public key** can only be decrypted by a **private key**.
    * Messages encrypted with a **private key** can only be decrypted by a **public key**.
    * This is why it's known as **asymmetric encryption**.
    * Examples include SSL/TLS certificates and SSH.
  * ### AWS Certificate Manager (ACM)
    * ACM creates, stores, and renews SSL/TLS certificates.
    * Supports single and multiple domains.
    * Integrates with ELB, CloudFront, CloudFormation, Elastic Beanstalk, and Nitro Enclaves.
    * For example: when you're generating a website with CloudFront, you can select a certificate from ACM. 
  * ### Symmetric Encryption
    * Encryption takes place by encrypting the plain text data with an encryption key.
    * Decryption takes place by decrypting the plain text **with the same key**.
    * This is why it's known as **symmetric encryption**.
  * ### AWS Key Management Service (KMS)
    * Creates and manages **BOTH** synmmetric and asymmetric *software* keys.
    * **Customer Managed Keys (CMKs)** are protected by **Hardware Security Modules (HSMs)**.
    * CMKs are created by developers and stored in KMS.
    * CMKs are used for encryption.
    * You can also use **AWS Managed Keys** that are provided by AWS to encrypt things like EBS volumes, SQS, FSx, etc. vs. using CMKs.
  * ### AWS Hardware Security Modules (HSMs)
    * Hardware Security Modules are physical, tamper-resistant computing device that generates, manages, and stores cryptographic keys.
    * In the AWS world, you'll see this as AWS CloudHSM. While most users are happy with AWS KMS (which is a managed software service), some organizations require the "physical isolation" of an HSM to meet strict regulatory or security standards.  

## Logging and Auditing
  * ### AWS CloudWatch
    * CloudWatch monitors the performance of the applications and resources in your AWS account.
    * CloudWatch is also a respository for **metrics** from your EC2 instances, network, S3, utilization, etc.
    * **CloudWatch Logs:** CloudWatch Logs gathers the metrics and builds the repository.
  * ### AWS CloudTrail
    * CloudTrail captures user activity and API calls in your AWS account such as creating or terminating resources, logins, etc.
    * CloudTrail captures who used an API, what time, what action, and on what resource.
    * Logs are retained for 90 days **unless** a **CLOUDTRAIL TRAIL** logs events into an S3 bucket for indefinite retention.
    * CloudTrails can be in one or multiple Regions.
  * ### VPC Flow Logs
    * VPC Flow Logs capture information about the IP traffic going to and from network interfaces within a VPC.
    * Data is stored using CloudWatch Logs at the VPC, subnet, and network interface levels.
  * ### Access Logs
    * **ELB Access Logs:** captures detailed information about requests sent to the load balancer, the IP, request type, etc. to analyze traffic patterns.
    * **S3 Access Logs:** choose a target bucket and provides detailed records for requests made to the bucket (disabled by default).  

## Threat Detection and Response
  * ### Amazon Inspector (The "Inspector" BEFORE the threat)
    * Amazon Inspector is an automated vulnerability management service that continually scans your AWS workloads for software vulnerabilities and unintended network exposure.
    * Amazon Inspector automatically scans all elgible resources once enable and provides a (0-10) risk score.
    * Amazon Inspector scans whenever changes are made such as installing new software, changing settings, or when a Zero Day vulnerability is discovered globally.
  * ### Amazon GuardDuty (The "Responder" DURING the threat)
    * Amazon GuardDuty is an intelligent threat detection service.
    * Amazon GuardDuty detects account compromise, instance compromise, bucket compromise, and malicious reconnaissance.
    * Amazon GuardDuty continuously monitors for events across:
      * AWS CloudTrail Management Events
      * AWS CloudTrain S3 Data Events
      * VPC Flow Logs
      * DNS Logs  
  * ### Amazon Detective (The "Detective" investigating AFTER the threat)
    * Amazon Detective analyzes, investigates, and identifies the root cause of the potential security issue and/or breach.
    * Amazon Detective automatically collects data from AWS resources including VPC Flow Logs, CloudTrail, and GuardDuty.
    * Amazon Detective uses ML, statistical analysis, and graph theory to determine root cause.
    * Amazon Detective provides a unified, interactive view of resources, users, and the interactions between them.
  * ### Amazon Macie (The "Robot" that enables security and preventative security)
    * Amazon Macie is a fully managed service that uses ML and pattern matching to discover, monitor, and protect sensitive data in your S3 buckets.
    * Amazon Macie can identify a variety of data types including personally identifiable information (PII), protected health information (PHI), API keys, and secret keys.
 
## Firewalls and Distributed Denial of Service (DDoS) Attacks
  * ### AWS Web Application Firewall (WAF)
    * WAF can be placed in front of:
      * CloudFront
      * ALBs
      * Amazon API Gateway
      * AWS AppSync   
    * WAFs are layer 7 OSI capable of "reading" IP addresses, HTTP headers and body, and custom URIs.
    * WAFs block cross-scripting XSS and SQL injection attacks. 
  * ### AWS Shield
    * AWS Shield is standard protection that protects you from DDoS attacks where attackers attempt to bring down your service by overwhelming your service with requests.
    * AWS Shield safeguards web applications running on AWS with **always-on** detection and automatic **in-line** mitigations.
    * Two tiers:
      * Standard: standard, no cost
      * Advanced: $3k/month and 1-year commitment, provides access to a **Shield Response Team (SRT)** giving you 24/7 access to DDoS experts who will help you fight the attack and cost protection, where you will not be charged for any auto-scaling that occurs as a result of the DDoS attack.
  * ### Network Firewalls vs. DNS Firewalls
    * #### **AWS Network Firewall**
      * Managed service for **VPC network protection**.
      * Protects resources in your VPC via a **firewall endpoint**, which is a flexible rules engine providing fine-grained control over network traffic.
      * Always placed in a "firewall" subnet to guard access to your "protected" subnet.
      * Traffic is sent via the firewall endpoint in the firewall subnet, which then routes traffice to the internet gateway (IGW) (in the protected subnet route table, the firewall endpoint is listed as a target VPC endpoint).
      * Usually will have a network load balancer (NLB) in front of it.
      * Includes *stateful* and *stateless* firewalls, intrusion prevention systems (IPS), and web filtering.
      * Managed via the **AWS Firewall Manager** service or the **AWS Organizations** service (if managing multiple firewall deployments across multiple AWS accounts). 
    * #### **Amazon Route 53 Resolver DNS Firewall**
      * Filters and regulates inbound and outbound DNS traffic for VPCs.
      * Requests route through Route 53 Resolver for DNS, which helps prevent DNS exfiltration of data (prevents sending data to the wrong DNS or endpoint if hijacked by attackers).
      * Monitors and controls the domains applications an query.
      * Managed through AWS Firewall Manager or AWS Organizations (if mutliple accounts).

## AWS Security Hub
  * ### Overview
    * Provides a comprehensive view of security alerts and posture across AWS accounts.
    * Aggregates, organizes, and prioritizes security alerts and findings from multiple AWS services.
    * Continuous monitoring of your environment using automated security checks.
    * Validates against the following security standards:
      * AWS Foundational Security Best Practices
      * CIS AWS Foundations Benchmar
      * PCI DSS  

## AWS Security Bulletins
  * ### Overview
    * Security and privacy events affecting AWS services published to a dashboard (and provides a really simple syndication (RSS) feed).

## AWS Trust & Safety Team
  * ### Overview
    * Contact this team if your AWS resources are being used for:
      * Spam
      * Port Scanning
      * DDoS Attacks
      * Intrusion Attempts
      * Hosting of Objectional or Copyrighted Content
      * Distributing Malware  

## Penetration Testing
  * ### Overview
    * Penetration testing is the practice of testing one's own application's security for vulnerabilities by simulating an attack.
    * AWS allows penetration testing without prior approval for 8 AWS services (just remember you cannot simulate DDoS attacks or any attacks involving DNS). 

## AWS Resource Access Manager (RAM)
  * ### Overview
    * RAM allows you to share specific AWS resources across different AWS accounts within your Organization.
    * Instead of creating a duplicate subnet or a license in five different accounts, you create it once in a "Central" account and share it.
    * This saves money and simplifies networking.
    * Users who have resources shared WITH them cannot modify those resources.
   
## AWS Artifact
  * ### Overview
    * Provides on-demand access to AWS's security and compliance reports as well as select online agreements
    * Reports available in AWS Artifact include:
      * Service Organizaton Control (SOC) Reports
      * Payment Card Industry (PCI) Reports  

