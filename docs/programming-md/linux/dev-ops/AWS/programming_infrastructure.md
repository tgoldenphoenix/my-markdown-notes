# Programming your Infrastructure: The command line, SDKs, and CloudFormation

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

A template is a description of your infrastructure, written in JSON or YAML, that can be interpreted by CloudFormation. The idea of describing something rather than listing the necessary actions is called a **declarative approach**.  
Declarative means you tell CloudFormation how your infrastructure should look. You aren’t telling CloudFormation what actions are needed to create that infrastructure, and you don’t specify the sequence in which the actions need to be executed.

Don’t mix up the terms Infrastructure as Code and `Infrastructure as a Service` (IaaS)! IaaS means renting virtual machines, storage, and network with a pay-per-use pricing model.

The AWS CloudFormation `yaml` file is turned into AWS API calls.

- Benefits of using CloudFormation:
  * It **handles dependencies**. Ever tried to register a web server with a load balancer that wasn’t yet available? When you first start trying to automate infrastructure creation, you’ll miss a lot of dependencies. Trust us: never try to set up complex infrastructure using scripts. You’ll end up in dependency hell! You should use a declarative approach like CloudFormation.
  * It’s **reproducible**. Is your test environment an exact copy of your production environment? Using CloudFormation, you can create two identical infrastructures. It is also possible to apply changes to both the test and production environment.
  * It’s **updatable**. CloudFormation supports updates to your infrastructure. It will figure out the parts of the template that have changed and apply those changes as smoothly as possible to your infrastructure.

- `Parameters`—Parameters are used to customize a template with values, for example, domain name, customer ID, and database password.
- A `resource` is the smallest block you can describe. Example: a network topology, a load balancer, a DNS entry, virtual machines, database, CND, an Elastic IP address, security group, IAM role, etc
- `Outputs`—An output is comparable to a parameter, but the other way around. An output returns details about a resource created by the template, for example, the public name of an EC2 instance.

If you create an infrastructure from a template, CloudFormation calls it a **stack**. You can think of template versus stack much like class versus object. The template exists only once, whereas many stacks can be created from the same template. 

### Parameter

A parameter has at least a `name` and a `type`. We encourage you to add a `description` as well, as shown in the next listing.

```yaml
Parameters:
  Demo: # You can choose the name of the parameter.
    Type: Number # This parameter represents a number.
    Description: 'This parameter is for demonstration'
```

In addition to using the `Type` and `Description` properties, you can enhance a parameter with some properties (key: value pair in yaml).

A parameter section of a CloudFormation template could look like this:

```yaml
Parameters:
   KeyName:
     Description: 'Key Pair name'
     Type: 'AWS::EC2::KeyPair::KeyName'
   NumberOfVirtualMachines:
     Description: 'How many virtual machine do you like?'
     Type: Number
     Default: 1
     MinValue: 1
     MaxValue: 5 # Prevents massive costs with an upper bound
   WordPressVersion:
     Description: 'Which version of WordPress do you want?'
     Type: String
     AllowedValues: ['4.1.1', '4.0.1']
```

### Resource

A resource has at least a name, a type, and some properties, as shown in the next listing.

```yaml
Resources:
  VM: # Name or logical ID of the resource that you can choose
    Type: 'AWS::EC2::Instance' # The resource of type AWS::EC2::Instances defines a virtual machine.
    Properties:
      # [...]
```

When defining resources, you need to know about the type and that type’s `properties`. In this book, you’ll get to know a lot of resource types and their respective properties. An example of a single EC2 instance appears in the following code snippet.

```yaml
Resources:
  VM:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: 'ami-6057e21a'
      InstanceType: 't2.micro'
      SecurityGroupIds:
      - 'sg-123456'
      SubnetId: 'subnet-123456'
```

### Output

A CloudFormation template’s output includes at least a name (like parameters and resources) and a value, but we encourage you to add a description as well, as illustrated in the next listing. You can use outputs to pass data from within your template to the outside.

```yaml
Outputs:
  NameOfOutput: # Name of the output that you can choose
    Value: '1'
    Description: 'This output is always 1'
```

If you see `!Ref NameOfSomething`, think of it as a placeholder for what is referenced by the name. A `!GetAtt 'NameOfSomething.AttributeOfSomething'`, shown in the next code, is similar to a ref but you select a specific attribute of the referenced resource.

```yaml
Outputs:
  ID:
    Value: !Ref Server # References the EC2 instance
    Description: 'ID of the EC2 instance'
  PublicName:
    Value: !GetAtt 'Server.PublicDnsName' # Gets the attribute PublicDnsName of the EC2 instance
    Description: 'Public name of the EC2 instance'
```

### Alternatives to CloudFormation

If you don’t want to write plain JSON or YAML to create templates for your infrastructure, a few alternatives to CloudFormation exist.

**Terraform** and AWS CloudFormation are both leading tools for Infrastructure as Code (IaC), allowing you to define cloud resources using code. The choice between them largely depends on whether your organization is strictly committed to AWS or requires a multi-cloud strategy.

- Terraform also supports AWS, Azure, GCP, VMware, etc
- CloudFormation is native only to AWS

## Temporary notes

created IMA user: `mycli`

create another IMA user: `myuser`

create a stack from cloudformation template named `myvm`

A **bucket** is used to store static assets (images, CSS files, ...).