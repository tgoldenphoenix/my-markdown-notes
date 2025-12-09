# Elastic Compute Cloud (`EC2`) & Load Balancer

## The Basics

This is IaaS (Infrastructure as a Service).

The EC2 service provides **virtual machines**. You’ll use a Linux machine with a distribution called Amazon Linux to install Apache, PHP, and WordPress. You aren’t limited to Amazon Linux; you could also choose Ubuntu, Debian, Red Hat, or Windows.

Virtual machines can fail, so you need at least two of them. The load balancer will distribute the traffic between them. In case a virtual machine fails, the load balancer will stop sending traffic to the failed VM, and the remaining VM will need to handle all requests until the failed VM is replaced.

**AMI ID (Amazon Machine Image ID)**: Remember that you used the Amazon Linux OS. If you click the AMI ID, you’ll see the version number of the OS, among other things.

- Virtual Machines use cases:
  * Hosting a web application such as WordPress
  * Transforming or analyzing data, such as encoding video files

EC2 instance = a virtual machine

**HVM (Hardware Virtual Machine)** is the virtualization type. HVM is the modern, high-performance method for running EC2 instances.

The **AMI (Amazon Machine Image)** is the basis for your virtual machine starts. AMIs are offered by AWS, third-party providers, and by the community. AWS offers the Amazon Linux AMI, which is based on Red Hat Enterprise Linux and optimized for use with EC2. You’ll also find popular Linux distributions and AMIs with Microsoft Windows Server as well as more AMIs with preinstalled third-party software in the AWS Marketplace.

A **virtual appliance** is an image of a virtual machine containing an OS and preconfigured software. Virtual appliances are used when the hypervisor starts a new VM. Because a virtual appliance contains a fixed state, every time you start a VM based on a virtual appliance, you’ll get exactly the same result.  
You can reproduce virtual appliances as often as needed, so you can use them to eliminate the cost of installing and configuring complex stacks of software. Virtual appliances are used by virtualization tools from VMware, Microsoft, and Oracle, and for Infrastructure as a Service (IaaS) offerings in the cloud.

The AMI is a special type of virtual appliance for use with the EC2 service. An AMI technically consists of a read-only filesystem including the OS, additional software, and configuration. You can also use AMIs for deploying software on AWS, but the AMI does not include the kernel of the OS. The kernel is loaded from an Amazon Kernel Image (AKI).

You can view VM logs without needing an SSH connection.

## Choose the VM Size

Virtual Machine size = computing power

AWS classifies computing power into instance types. An instance type primarily describes the number of virtual CPUs and the amount of memory.

Instance families are optimized for different kinds of use cases, as described next:

- T family—Cheap, moderate baseline performance with the ability to burst to higher performance for short periods of time
- M family—General purpose, with a balanced ration of CPU and memory
- C family—Computing optimized, high CPU performance
- R family—Memory optimized, with more memory than CPU power compared to the M family
- X family—Extensive capacity with a focus on memory, up to 1952 GB memory and 128 virtual cores
- D family—Storage optimized, offering huge HDD capacity
- I family—Storage optimized, offering huge SSD capacity
- P, G, and CG family—Accelerated computing based on GPUs (graphics processing units)
- F family—Accelerated computing based on FPGAs (field-programmable gate arrays)

You’ll overestimate the resource requirements for your applications. We recommend that you try to start your application with a smaller instance type than you think you need at first—you can change the instance family and type later if needed.

`[instance type][generation].[instance size]`. The instance family groups instance types with similar characteristics.  The instance size defines the capacity of CPU, memory, storage, and networking. For example, the instance type t2.micro tells you the following:

- The instance family is called t. It groups small, cheap virtual machines with low-baseline CPU performance but the ability to burst significantly over baseline CPU performance for a short time.
- You’re using generation 2 of this instance family.
- The size is micro, indicating that the EC2 instance is very small.

---

You might have already heard about Apple switching from Intel processors to ARM processors. The reason for this is that custom-built ARM processors achieve higher performance with lower energy consumption. This is, of course, exciting not only for laptops but also for servers in the data center.

AWS offers machines based on custom-built ARM processors called Graviton as well. As a customer, you will notice similar performance at lower costs. However, you need to make sure that the software you want to run is compiled for the ARM64 architecture. We migrated workloads from EC2 instances with Intel processors to virtual machines with ARM processors a few times already, typically within one to four hours.

## Connecting to VMs

As an administrator of a Linux machine, you used a username and password or username and a public/private key pair to authenticate yourself in the past.  
By default, AWS uses a username and a key pair for authentication into an EC2 instance. We try to avoid this approach, because it works only for a single user, and it is not possible to change the key pair externally after launching an EC2 instance.

You will learn about a new approach to connect to EC2 instances that does not require inbound SSH connectivity.

- You will learn how to connect to an EC2 instance by using the **AWS Systems Manager Session Manager**. The advantages of this approach follow:
  * You do not need to configure key pairs upfront but use temporary key pairs instead.
  * You don’t need to allow inbound SSH or RDP connectivity, which limits the attack surface.
  * Open a terminal to your instance directly on the browser.

## Shutting down a virtual machine

- To avoid incurring charges, you should always turn off virtual machines when you’re not using them. You can use the following four actions to control a virtual machine’s state:
  1. Start a stopped VM
  2. Stop a running virtual machine. A stopped virtual machine doesn’t incur charges, except for attached resources like network-attached storage. A stopped virtual machine can be started again but likely on a different host. If you’re using network-attached storage, your data persists.
  3. Reboot: Turn off, then on again. You won’t lose any persistent data when rebooting a virtual machine because it stays on the same host.
  4. Terminate: delete the VM. You can’t start a virtual machine that you’ve terminated. The virtual machine is deleted, usually together with its dependencies, like network-attached storage and public and private IP addresses. A terminated virtual machine doesn’t incur charges.

## Allocating a public IP address

The public IPv4 address assigned to your EC2 instance is subject to change. For example, when you stop and start your instance, AWS assigns a new public IPv4 address. Therefore, you will learn how to attach a fixed public IP address to the virtual machine in the following section.

AWS offers a service called `Elastic IPs` for allocating fixed public IP addresses.

## Load Balancer

Load Balancer is part of the EC2 service.

The load balancer performs health checks to ensure requests are routed only to healthy targets. 

**Virtual machines instances** can be started and stopped on demand to fulfill your computing needs within minutes. Being able to install software on virtual machines enables you to execute your computing tasks without needing to buy or rent hardware.

## Terminologies

`vCPU` = virtual CPU

- `GiB` (Gibibyte) is a binary unit equals 2^30 bytes (It is based on binary). Example memory 1 GiB
- `GB` (gigabyte) is a metric unit equals 10^9 bytes (Calculated based on decimal).

subnet = VPC

- The difference between EC2 and ECS is the level of abstraction and control you have over your compute resources.
  * EC2 (Elastic Compute Cloud) provides Infrastructure as a Service (IaaS). It gives you a raw virtual machine (VM) that you manage completely.
  * ECS (Elastic Container Service) is a Container Orchestration Service. It manages, runs, and scales your Docker containers automatically, abstracting the VM layer away from you.
- ECS often runs on top of EC2, acting as a management layer for containerized applications.

---

The choice between Amazon ECS (Elastic Container Service) and Kubernetes (often used via Amazon EKS—Elastic Kubernetes Service) is the primary decision point for container orchestration on AWS.  
Both are powerful tools for deploying, managing, and scaling Docker containers, but they differ fundamentally in vendor lock-in, complexity, and integration.

- ECS:
  * is AWS-Native Proprietary Service
  * Vendor Lock-in, Only runs on AWS.
- Kubernetes:
  * is Open Source; the De-facto Industry Standard
  * Same config runs on AWS, Azure, GCP, or on-premises.

The **AWS Systems Manager (SSM)** is a collection of tools and capabilities that provide a unified operational hub for securely viewing, managing, and automating routine operational tasks across your AWS resources and on-premises infrastructure.