# Domain 3: Technology
# (3D: Amazon Serverless Architectures)

## Overview
  * Amazon strongly promotes use of their serverless services such as Lambda, Fargate, DynamoDB.
  * With serverless there are NO instances to manage - you do not need to provision hardware.
  * There is no management of OS or software.
  * Capacity provisioning and patching is handled automatically.
  * Automatic scaling and high availability is provided.

## Serverless Services
  * AWS Lambda
  * AWS Fargate ("serverless containers" - *see EC2 Compute markdown file 2B*)
  * Amazon Eventbridge
  * AWS Step Functions: orchestrates the components of your application.
  * Amazon Simple Queue Service (SQS)
  * Amazon Simple Notification Service (SNS)
  * Amazon API Gateway
  * Amazon S3
  * Amazon DynamoDB

## AWS Lambda
  * Executes code only when needed and scales automatically.
  * You put your code on Lambda, something triggers it, it runs, and you only pay for the amount of time it runs (millisecond billing).
  * No overhead, provisioning resources, etc. - just upload your code and let it do its thing!
  * You pay $0 when your code is not running.
  * Lambda integrates with almost all other AWS services.

## Amazon SQS
  * Reliable, highly-scalable message queueing service storing messages in transit between computers.
  * Good for distributed and decoupled applications and architectures.
  * SQS is pull-based not push-based, so applications will poll SQS for messages and pull the message in queue - useful for requests coming in from the front-end and not overwhelming & flooding the transactional layer.
  * There's also **Amazon MQ**, a message service that's similar to SQS, but based on Apache MQ and Rabbit MQ - used when customers require industry standard APIs and protocols (used when we can't use Amazon's proprietary message protocols).

## Amazon SNS
  * Publisher - Subscriber Model: you publish a notification and the subscribers all receiver it at the same time.
  * SNS is used for building and integrating loosely coupled architectures.
  * Instantaneous push-based delivery (no polling like SQS).
  * Simple API, easy integration with applications.
  * Inexpensive, pay-as-you-go pricing with no upfront cost.

## AWS Step Functions
  * 

