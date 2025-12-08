# Programming your infrastructure: The command line, SDKs, and CloudFormation

- AWS provides an application programming interface (API) that we can use to control every part of AWS with HTTPS requests. By everything, we really mean everything.
- However, calling the HTTP API directly is a low level task and requires a lot of repetitive work, like authentication, data (de)serialization, and so on. That’s why AWS offers tools on top of the HTTP API that are easier to use. Those tools follow:
  * `Command-line interface (CLI)`—Use the CLI to call the AWS API from your **local machine terminal**.
  * `Software development kit (SDK)`—SDKs, available for most programming languages, make it easy to call the AWS API from your programming language of choice.
  * `AWS CloudFormation`—Templates are used to describe the state of the infrastructure. AWS CloudFormation translates these templates into API calls.
- The API, however, is the foundation of all these tools.

## Using the command-line interface

The AWS CLI is a convenient way to interact with AWS from your **local machine terminal**. It runs on Linux, macOS, and Windows and provides a unified interface for all AWS services. Unless otherwise specified, the output is by default in JSON format.

Verify your AWS CLI installation by running aws --version in your terminal. The version should be at least 2.4.0.

To use the AWS CLI, you need to specify a service and an action. You can add options with `--name value` as follows: `$ aws <service> <action> [--name value ...]`

The `--query` option uses JMESPath syntax, which is a query language for JSON, to extract data from the result.

Verify your AWS CLI installation by running `aws --version` in your terminal.

Khi tạo IAM user sẽ được cấp cho một pair: `access key ID` & `secret access key`:
- Cả 2 keys đều phải giữ bí mật, không để lộ.
- Copy-paste vào local terminal `aws configure`
- Access Key ID is the Identifier (like a username). Secret Access Key is the Secret (like a password).

- SSH Public Key & Private Key:
  * Public Key is the Identifier (placed on the server). Private Key is the Secret (stays with the user).

One important feature of the CLI is the help keyword. You can get help at the following three levels of detail:

- `aws help`—Shows all available services
- `aws <service> help`—Shows all actions available for a certain service
- `aws <service> <action> help`—Shows all options available for the particular service action

- The limitations of the CLI solution follow:
  * It can handle only one virtual machine at a time.
  * There is a different version for Windows than for Linux and macOS.
  * It’s a command-line application, not a graphical one.

## Programming with the SDK

An AWS SDK is a convenient way to make calls to the AWS API from your favorite programming language. The SDK takes care of things like authentication, retry on error, HTTPS communication, and XML or JSON (de)serialization. You’re free to choose the SDK for your favorite language, but in this book, most examples are written in JavaScript and run in the Node.js runtime environment.

- The hard parts about using the SDK follow:
  * The SDK (or, better, Node.js) follows an **imperative approach**. You provide all instructions, step by step, in the right order, to get the job done.
  * You have to deal with dependencies (e.g., wait for the instance to be running before connecting to it).
  * There is no easy way to update the instances that are running with new settings (e.g., change the instance type).

It’s time to enter a new world by leaving the imperative world and entering the **declarative world**.

## AWS CloudFormation

Infrastructure as Code. Automate the setup process.

You can control every single service on AWS by sending requests to a REST API. Based on this, a variety of solutions can help you automate your overall infrastructure. Infrastructure automation is a big advantage of the cloud compared to hosting on-premises. This part will guide you into infrastructure orchestration and the automated deployment of applications.

A template is a description of your infrastructure, written in JSON or YAML, that can be interpreted by CloudFormation. The idea of describing something rather than listing the necessary actions is called a declarative approach. Declarative means you tell CloudFormation how your infrastructure should look. You aren’t telling CloudFormation what actions are needed to create that infrastructure, and you don’t specify the sequence in which the actions need to be executed.

## Temporary notes

created IMA user: `mycli`