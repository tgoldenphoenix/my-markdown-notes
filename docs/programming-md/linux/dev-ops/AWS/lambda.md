# AWS Lambda

AWS Lambda is a new way of computing: with functions. It is used to automate operational tasks.

Computing capacity is available at different layers of abstraction on AWS: virtual machines (EC2), containers, and functions (lambda). 

Lambda (Function as a Service) gives you maximum convenience. You upload a function, and AWS handles everything else. Your code runs only when triggered by an event (like an API request or a file upload to S3). It is best for short, event-driven tasks, microservices, and cost optimization.

Both AWS Lambda and EC2 provide compute power (run code)

- EC2 (Elastic Compute Cloud):
  * Server-Based (IaaS or Infrastructure as a Service). Renting a virtual machine.
  * Pay per Hour/Second the server is running (even when idle).
  * Your Responsibility. You must patch, update, and manage the OS.
  * You have Full control over the operating system.

- AWS Lambda
  * Serverless (FaaS or Function as a Service). Renting a function execution.
  * Pay per Execution (down to the millisecond).
  * AWS Responsibility. AWS manages the OS, patching, scaling, and infrastructure.
  * No OS access; you only upload your code.

## Terminologies

The term **serverless** refers to any service that you do not need to managing the server yourself & only focus on the coding of the software (like the operating system). It does not refer to the absence of physical servers (because everything runs on servers).  
Serverless services are billed per request and by resource consumption

The term serverless is best understood as a spectrum of abstraction:

- Least Serverless (Most Management): EC2 Instances
- Mid-Serverless: Containers on AWS Fargate (You manage the application layer, not the server layer).
- Most Serverless (Zero Management): AWS Lambda, Google Cloud Run (serverless container), AWS App Runner, cloud function (FaaS).

AWS is not the only provider offering a serverless platform. Google (Cloud Functions) and Microsoft (Azure Functions) are other competitors in this area