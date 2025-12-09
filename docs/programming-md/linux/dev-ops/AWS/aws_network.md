# AWS Networking & Security

Securing your system: IAM, security groups (filrewalls), and VPC (private network).

`NACLs` stands for Network Access Control Lists.

In AWS (and similar cloud platforms), a NACL is an optional layer of security that acts as a stateless firewall for controlling traffic entering and leaving one or more subnets.

Creating virtual networks allows you to build closed and secure network environments on AWS and to connect these networks with your home or corporate network.

- What are your responsibilities?
  * Configuring access management that restricts access to AWS resources like S3 and EC2 to a minimum, using AWS IAM
  * Configuring a firewall for your virtual network that controls incoming and outgoing traffic with security groups and NACLs

## Security Groups

Control incoming and outgoing traffic to your virtual machine, your database, or your load balancer with a **firewall**. 
For example, use a security group allowing incoming HTTP traffic from the internet to `port 80` of the load balancer. Or restrict network access to your database on port 3306 to the virtual machines running your web servers.

- It is a virtual firewall for your resources.
- firewall rules = security groups

## AWS Identity and Access Management (`IAM`)

The **Identity and Access Management (IAM) service** provides authentication and authorization for the AWS API.

When you send a request to the AWS API, IAM verifies your identity and checks whether you are allowed to perform the action. IAM controls who (authentication) can do what (authorization) in your AWS account. For example, is the user allowed to launch a new virtual machine?

- The various components of IAM follow:
  * An `IAM user` is used to authenticate people or workloads running outside of AWS (cái này giống cái root account).
  * An `IAM group` is a collection of IAM users with the same permissions.
  * An `IAM role` is used to authenticate **AWS resources**, for example, an EC2 instance.
  * An `IAM identity policy` is used to **define** the permissions for a user, group, or role.

- **IAM Roles** authenticate AWS entities such as EC2 instances. Roles are attached to entities (EC2 instance, ECS container, Lambda function).
- **IAM users** authenticate the people who manage AWS resources, for example, system administrators, DevOps engineers, or software developers.

- IAM user & AWS account root user similarity:
  * Both have a password (needed to log in to the AWS Management Console)
  * Both Can have access keys (needed to send requests to the AWS API (e.g., for CLI or SDK)

By default, users and roles **can’t do anything**. You have to create an identity policy stating what actions they’re allowed to perform. IAM users and IAM roles use **identity policies** for authorization.

AMI (Amazon Machine Image). Khác với IAM.

An IAM role grants processes running on the EC2 virtual machine access to other AWS services. This is needed because you will use AWS services called Systems Manager and EC2 Instance Connect to establish an SSH connection with your virtual machine later.

### Defining permissions with an IAM identity policy

By attaching one or multiple **IAM identity policies** to an IAM user or role, you are granting permissions to manage AWS resources. Identity policies are defined in **JSON** and contain one or more **statements**. A statement can either allow or deny specific actions on specific resources. You can use the wildcard character `*` to create more generic statements.

---

**Identity vs. resource policies**

IAM policies come in two types. **Identity policies** are attached to users, groups, or roles. **Resource policies** are attached to resources. Very few resource types support resource policies. One common example is the S3 bucket policy attached to S3 buckets.

If a policy contains the property `Principal`, it is a resource policy. The Principal defines who is allowed to perform the action. Keep in mind that the principal can be set to `public`.

---

If you have multiple statements that apply to the same action, `Deny` overrides `Allow`. When you deny an action, you can’t allow that action with another statement.

Resources in AWS have an **Amazon Resource Name (ARN)**. Example of resource: an EC2 instance is a resource.  
The ARN of an EC2 instance: `arn:aws:ec2:us-east-1:878533158213:instance/i-3dd4f812`

The following two types of identity policies exist:

- `Managed policy`—If you want to create identity policies that can be **reused** in your account, a managed policy is what you’re looking for. There are two types of managed policies:
  1. `AWS managed policy`—An identity policy maintained by AWS. There are identity policies that grant admin rights, read-only rights, and so on.
  2. `Customer managed`—An identity policy maintained **by you**. It could be an identity policy that represents the roles in your organization, for example.
- `Inline policy`—An identity policy that **belongs** to a certain IAM role, user, or group. An inline identity policy **can’t** exist without the IAM role, user, or group that it belongs to.

### Users for authentication and groups to organize users

A user can authenticate using either a username and password or access keys. When you log in to the Management Console, you’re authenticating with your username and password. When you use the CLI from your computer, you use access keys to authenticate as the `mycli` user.

Groups can’t be used to authenticate, but they centralize authorization. So, if you want to stop your admin users from terminating EC2 instances, you need to change the identity policy only for the group instead of changing it for all admin users.

### Authenticating AWS resources with roles

**WARNING**: You should never copy a user’s access keys to an EC2 instance; use IAM roles instead! Don’t store security credentials in your source code. And never ever check them into your source code repository. Try to use IAM roles instead whenever possible, as described in the next section.

Various use cases exist where an EC2 instance needs to access or manage AWS resources. For example, an EC2 instance might need to do the following:

- Back up data to the object store S3
- Terminate itself after a job has been completed
- Change the configuration of the private network environment in the cloud

To be able to access the AWS API, an EC2 instance needs to authenticate itself. You could create an IAM user with access keys and store the access keys on an EC2 instance for authentication. But doing so is a hassle and violates security best practices, especially if you want to rotate the access keys regularly.

Instead of using an IAM user for authentication, you should use an IAM role whenever you need to **authenticate AWS resources** like EC2 instances. When using an IAM role, your access keys are injected into your EC2 instance automatically.

If an IAM role is attached to an EC2 instance, all identity policies attached to those roles are evaluated to determine whether the request is allowed.  
By default, no role is attached to an EC2 instance, and, therefore, the EC2 instance is not allowed to make any calls to the AWS API.

## Controlling network traffic to and from your virtual machine

You want traffic to enter or leave your EC2 instance only if it has to do so. With a firewall, you control ingoing (also called **inbound or ingress**) and outgoing (also called **outbound or egress**) traffic. 

If you run a web server, the only ports you need to open to the outside world are ports `80` for HTTP traffic and `443` for HTTPS traffic. All other ports should be closed down. You should only open ports that must be accessible, just as you grant only the permissions you need with IAM

Before network traffic enters or leaves your EC2 instance, it goes through a firewall provided by AWS. The firewalls inspects the network traffic and uses rules to decide whether the traffic is allowed or denied.

AWS is responsible for the firewall, but you’re responsible for the rules. By default, a security group does not allow any **inbound traffic**. You must add your own rules to allow specific incoming traffic.  
A security group contains a rule **allowing all outbound traffic** by default. If your use case requires a high level of network security, you should remove the rule and add your own rules to control outgoing traffic.

- IAM Resource (User, Role, Policy):
  * Identity and Access Management (Authentication & Authorization).
  * Controls access to AWS services and actions (e.g., launching an EC2 instance, reading an S3 bucket).
- Security Group (SG):
  * Network Traffic Filtering (Firewall).
  * Controls network packets entering or leaving a resource (e.g., EC2, RDS).

### Controlling traffic to virtual machines with `security groups`

A **security group** acts as a firewall for virtual machines and other services.

You will associate a security group with AWS resources, such as EC2 instances, to control traffic. It’s common for EC2 instances to have more than one security group associated with them and for the same security group to be associated with multiple EC2 instances.

A security group consists of a set of rules (not to be confused with IAM rules).

A **Security Group (SG)** is a virtual, stateful firewall that controls the traffic allowed to and from the resources (such as an EC2 instance, RDS database, or load balancer) to which it is attached.  
It is considered the first and most common layer of defense for your individual resources in AWS.

By default, inbound traffic is denied and outbound traffic is allowed.

## Creating a private network in the cloud: Amazon Virtual Private Cloud (VPC)

Control network traffic by defining subnets and routing tables. Doing so allows you to specify private networks that are not reachable from the outside.

When you create a VPC, you get your own private network on AWS. Private means you can use the address ranges `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16` to design a network that isn’t necessarily connected to the public internet. You can create subnets, route tables, network access control lists (NACLs), and gateways to the internet or a VPN endpoint.

Amazon Virtual Private Cloud supports IPv6 as well. You can create IPv4 only, IPv6 only, or IPv4 and IPv6 VPCs. To reduce complexity, we are sticking to IPv4 in this chapter.

Subnets allow you to separate concerns. We recommend to create at least the following two types of subnets:

- Public subnets—For all resources that need to be reachable from the internet, such as a load balancer of a internet-facing web application
- Private subnets—For all resources that should not be reachable from the internet, such as an application server or a database system

What’s the difference between a public and private subnet? A public subnet has a route to the internet; a private subnet doesn’t.

### Default Gateway vs VPN Gateway

The term Gateway (or Default Gateway) refers to the device that acts as the front door for all traffic leaving a local network (LAN) and heading to a foreign network (like the public internet).

## Terminologies

multifactor authentication (MFA)

The `TLS (Transport Layer Security)` protocol is the industry standard for securing communication over computer networks, most notably the internet.  
It is the updated, more secure version of the older `SSL (Secure Sockets Layer)` protocol, and when you see `HTTPS` in your web browser, you are using TLS.  
TLS operates logically between the Application Layer (Layer 7) and the Transport Layer (Layer 4) of the OSI model.

---

k

## Temporary

created IMA user: `mycli`

create another IMA user: `myuser`

create a stack from cloudformation template named `myvm`