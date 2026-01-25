# Domain 1: Cloud Concepts
# (1A: Cloud Benefits, Models, and Deployment)

## What is "The Cloud"?

Cloud computing is the on-demand delivery of IT resources over the internet with pay-as-you-go pricing. Instead of buying, owning, and maintaining physical data centers and servers, you can access technology services, such as computing power, storage, and databases, on an as-needed basis from a cloud provider like Amazon Web Services (AWS).

## Six (6) Benefits of the AWS Cloud (and Cloud Services in General)

1. **Trade Capital Expenses for Variable Expenses (CAPEX vs. OPEX):**
   * Building on-premises hardware is a capital expense where you pay up front and then pay for resources if you are not using them (CAPEX).
   * You have to guess how much hardware and capacity you need upfront (CAPEX).
   * Leveraging resources through the Cloud with pay-as-you go pricing, you trade that capital expense with variable expense (OPEX).
  
2. **Benefit from Massive Economies of Scale:**
   * AWS has hundreds of thousands of customers, which allows them to achieve much higher economies of scale, which lowers pay-as-you go pricing.

3. **Stop Guessing about Capacity:**
   * You no longer need to guess at how much hardware and capacity you need.
   * The Cloud allows you to scale up or scale down resources as needed (elasticity).
  
4. **Increase Speed and Agility:**
   * New IT resources are only a click away.
   * The time to leverage IT resources are now significantly reduced (speed) and with resources so readily available, you gain the ability to fail fast, experiment, and innovate (agility).
  
5. **Stop Spending Money Running and Maintaining Data Centers:**
   * No longer having to spend money on infrastructure, hardware, and data centers allows you to focus money on innovation and differentiating your business.
  
6. **Go Global in Minutes:**
   * Easily deploy your application in multiple regions around the world in minutes and with the lowest latency possible.
  
## Cloud Computing Models

**Infrastructure as a Service (IaaS)**
   * This gives you the most control where you control the virtual server (EC2), storage (S3), databases (RDS), and the OS.
   * AWS manages the hardware and infrastructure of the Cloud, you manage everything else (the OS, the software/apps, the patches) in the Cloud.
   * AWS Examples: Amazon EC2 Instances, Amazon VPC
   * User Profile: For the Cloud Engineer who wants total control.

**Platform as a Service (PaaS)**
   * You manage your apps, data, and code.
   * AWS manages the hardware and infrastructure of the Cloud **as well as** the OS and the patching of the OS.
   * AWS Examples: AWS Elastic Beanstalk, AWS Lambda
   * User Profile: For the Software Developer who only wants to run code and build apps.

**Software as a Service (SaaS)**
   * You only use the web-based apps.
   * AWS manages the hardware and infrastructure of the Cloud, the OS and patches, **as well as** the apps.
   * AWS Examples: AWS Marketplace apps
   * User Profile: For the Business User who only wants to use the apps.

## Types of Cloud Deployment

1. **Public Cloud**
   * Full deployment in the Cloud.
   * All apps and services run in the Cloud having been either deployed in the Cloud directly or migrated from on-prem infrastructure.

2. **Hybrid**
   * Connecting on-prem infrastructure resources (off the Cloud) to resources in the Cloud.
     
3. **On-Premises (On-Prem)**
   * Resources completely out of the Cloud and on-premises hardware.
