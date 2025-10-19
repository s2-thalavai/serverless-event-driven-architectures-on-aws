# serverless-event-driven-architectures-on-aws

Create an EventBridge Pipe using SQS, Lambda, and SNS

## Objectives

This lab demonstrates how to build a serverless event-driven architecture using Amazon EventBridge Pipes. 
The setup connects multiple AWS services to create a loosely coupled, reactive system.

## High-Level Architecture Overview

<img width="1387" height="848" alt="image" src="https://github.com/user-attachments/assets/ad3e3d85-90a8-4eb6-ab6a-5dbbf9dc9c7f" />


## Core AWS Components Used

| Component             | Purpose                                                         |
| --------------------- | --------------------------------------------------------------- |
| **Amazon SQS**        | Decouple message producers from consumers. Source for the Pipe. |
| **EventBridge Pipes** | Connects source to target; adds filtering and enrichment.       |
| **AWS Lambda**        | Enriches messages with extra data from DynamoDB.                |
| **Amazon DynamoDB**   | Stores enrichment data (e.g., lookup information).              |
| **Amazon SNS**        | Publishes final messages to subscribers (email, etc.).          |



## Cloud Lab Tasks
1.
Introduction
Getting Started
2.
Create the Required Resources
Create an SQS Queue and an SNS Topic
Create a DynamoDB Table
Create a Lambda Function
3.
Putting It All Together
Create an EventBridge Pipe
Test the EventBridge Pipe
4.
Conclusion
Clean Up
Wrap Up
