# Programming your infrastructure: The command line, SDKs, and CloudFormation

AWS provides an application programming interface (API) that we can use to control every part of AWS with HTTPS requests.

In the end, you can write software that spins up VMs on AWS as well as in-memory caches, data warehouses, and much more.

Calling the HTTP API is a low level task and requires a lot of repetitive work, like authentication, data (de)serialization, and so on. That’s why AWS offers tools on top of the HTTP API that are easier to use. Those tools follow:

- Command-line interface (CLI)—Use the CLI to call the AWS API from your terminal.
- Software development kit (SDK)—SDKs, available for most programming languages, make it easy to call the AWS API from your programming language of choice.
- AWS CloudFormation—Templates are used to describe the state of the infrastructure. AWS CloudFormation translates these templates into API calls.

On AWS, you control everything via an API. You interact with AWS by making calls to the REST API using the HTTPS protocol, as figure 4.1 illustrates. Everything is available through the API. You can start a virtual machine with a single API call, create 1 TB of storage, or start a Hadoop cluster over the API. By everything, we really mean everything.

Calling the API directly using plain HTTPS requests is inconvenient. The easy way to talk to AWS is by using the CLI or SDKs, as you’ll learn in this chapter. The API, however, is the foundation of all these tools.

## Using the command-line interface

The AWS CLI is a convenient way to interact with AWS from your terminal. It runs on Linux, macOS, and Windows and provides a unified interface for all AWS services. Unless otherwise specified, the output is by default in JSON format.

Verify your AWS CLI installation by running aws --version in your terminal. The version should be at least 2.4.0.

To use the AWS CLI, you need to specify a service and an action. You can add options with --name value as follows: `$ aws <service> <action> [--name value ...]`

One important feature of the CLI is the help keyword. You can get help at the following three levels of detail:

- `aws help`—Shows all available services
- `aws <service> help`—Shows all actions available for a certain service
- `aws <service> <action> help`—Shows all options available for the particular service action

## Programming with the SDK

An AWS SDK is a convenient way to make calls to the AWS API from your favorite programming language. The SDK takes care of things like authentication, retry on error, HTTPS communication, and XML or JSON (de)serialization. You’re free to choose the SDK for your favorite language, but in this book, most examples are written in JavaScript and run in the Node.js runtime environment.

## AWS CloudFormation

Infrastructure as Code. Automate the setup process.

You can control every single service on AWS by sending requests to a REST API. Based on this, a variety of solutions can help you automate your overall infrastructure. Infrastructure automation is a big advantage of the cloud compared to hosting on-premises. This part will guide you into infrastructure orchestration and the automated deployment of applications.

A template is a description of your infrastructure, written in JSON or YAML, that can be interpreted by CloudFormation. The idea of describing something rather than listing the necessary actions is called a declarative approach. Declarative means you tell CloudFormation how your infrastructure should look. You aren’t telling CloudFormation what actions are needed to create that infrastructure, and you don’t specify the sequence in which the actions need to be executed.


