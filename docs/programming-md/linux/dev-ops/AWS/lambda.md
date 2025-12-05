# Lambda

AWS Lambda is a new way of computing: with functions. It is used to automate operational tasks.

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