# AWS Notes

## Basic Terminologies

In PowerShell: replace the continuation backslash `\` with a `. At the command prompt: replace `\` with a `^`. 

**Resource pooling**—The cloud assigns resources based on a multitenant model, which means consumers share the same physical resources.

- Public—A cloud managed by an organization and open to use by the general public
- Private—A cloud that virtualizes and distributes the IT infrastructure for a single organization
- Hybrid—A mixture of a public and a private cloud

Amazon Web Services (AWS) is a public cloud. By combining your on-premises data center with AWS, you are building a hybrid cloud.

Cloud computing services also have several classifications, described here:

- Infrastructure as a Service (IaaS)—Offers fundamental resources like computing, storage, and networking capabilities, using virtual machines such as Amazon EC2, Google Compute Engine, and Microsoft Azure Virtual Machines.
- Platform as a Service (PaaS)—Provides platforms to deploy custom applications to the cloud, such as AWS Lambda, AWS App Runner, Google App Engine, and Heroku.
- Software as a Service (SaaS)—Combines infrastructure and software running in the cloud, including office applications like Amazon WorkSpaces, Google WorkSpace, and Microsoft 365.

AWS is a cloud-computing provider with a wide variety of IaaS, PaaS, and SaaS offerings.

The most prominent services provided by AWS are `EC2`, which offers virtual machines, and `S3`, which offers storage capacity.

- **On-premises** the software and hardware are located and managed within your organization's local physical data center.
- **Cloud** như AWS là trái nghĩa với on-premises.

Chọn **data center** của AWS ở gần vị trí mình cần.

---

- An Object Store (S3) is designed for storing large, unstructured files (image, video, logs, backups) and is accessed via HTTP API calls.
- Database Services (RDS/DynamoDB) are designed for storing structured data and handling relationships and transactions.

Object Store (S3) is used with CDN to deliver static content (in contrast to dynamic contents). This increase perfomance.

Why are AWS relational databases (RDS/DynamoDB) called "fully managed by AWS"? The user do not need to do this?

- You Manage: Database schema, SQL query optimization, indexing, application logic, and user permissions.
- AWS Manages: OS patches, hardware provisioning, backups, high availability, and network routing for the database endpoint.

This allows developers and engineers to focus only on their application code and database schema, rather than the infrastructure maintenance.

**Infrastructure on Demand (IoD)** is the core business model of public cloud computing (like AWS, GCP, and Azure). It refers to the ability of users to provision, manage, and pay for IT resources (servers, networking, storage) instantly, autonomously, and exactly when they need them.

**Failover handling** is the automated process of switching from a primary, active system to a redundant, standby system when the primary system fails or becomes unavailable.  
Failover handling is only half of the process. Once the primary system is repaired, the system needs to return to its original state.

**Primary-Standby replication (also known as Active-Passive replication)** is a system architecture designed to ensure High Availability (HA) and Disaster Recovery (DR) for databases and critical application servers.  
In this setup, only one server actively handles client traffic, while the other sits idle, constantly maintaining an exact copy of the data, ready to take over instantly.

The **Classic Load Balancer (CLB)** is the older, legacy load balancer from AWS, and the Application Load Balancer (ALB) is the modern, more intelligent replacement designed for contemporary web and microservice architectures.

## What can you do with AWS?

The web shop consists of dynamic content (such as products and their prices) and static content (such as the company logo). Splitting these up would reduce the load on the web servers and improve performance by delivering the **static content** over a **content delivery network (CDN)**.

The application running the web shop can be installed on virtual machines. Using AWS, John can run the same amount of resources he was using on his on-premises machine but split them into multiple, smaller virtual machines at no extra cost. If one of these virtual machines fails, the load balancer will send customer requests to the other virtual machines. This setup improves the web shop’s **reliability**.

load balancer for reliability, high availability

---

You defines a virtual network in the cloud and connects it to the corporate network through a virtual private network (VPN) connection.

---

AWS bills virtual machines per second with a minimum of 60 seconds. So Nick launches a virtual machine when starting a batch job and terminates it immediately after the job finishes.

As you’ve learned, AWS is a platform of services. Common problems such as load balancing, queuing, sending email, and storing files are solved for you by services. You don’t need to reinvent the wheel. It’s your job to pick the right services to build complex systems. Let AWS manage those services while you focus on your customers.

Because AWS is **API driven**, you can automate everything: write code to create networks, start virtual machine clusters, or deploy a relational database. Automation increases reliability and improves efficiency.

## Billing Cost

A bill from AWS is similar to an electric bill. Services are billed based on use. You pay for the time a virtual machine was running, the used storage from the object store, or the number of running load balancers. Services are invoiced on a monthly basis.

You can use some AWS services for free within the first 12 months of signing up.

Here is a taste of what’s included in the Free Tier:

- 750 hours (roughly a month) of a small virtual machine running Linux or Windows. This means you can run one virtual machine for a whole month, or you can run 750 virtual machines for one hour.
- 750 hours (or roughly a month) of a classic or application load balancer.
- Object store with 5 GB of storage.
- Small relational database with 20 GB of storage, including backup.
- 25 GB of data stored on NoSQL database.

If you exceed the limits of the Free Tier, you start paying for the resources you consume without further notice. You’ll receive a bill at the end of the month.

- You can be billed in the following ways:
  * Based on time of use—A virtual machine is billed per second. A load balancer is billed per hour.
  * Based on traffic—Traffic is measured in gigabytes or in number of requests, for example.
  * Based on storage usage—Usage can be measured by capacity (e.g., 50 GB volume no matter how much you use) or real usage (such as 2.3 GB used).

## Alternatives

Microsoft Azure and Google Cloud Platform (GCP).

Similar: 

- An IaaS offering that provides virtual machines on-demand: Amazon EC2, Azure Virtual Machines, Google Compute Engine
- Highly distributed storage systems able to scale storage and I/O capacity without limits: Amazon S3, Azure Blob Storage, Google Cloud Storage

The GCP seems more focused on cloud-native applications than on migrating your locally hosted applications to the cloud, in our opinion.

## Exploring AWS services

You can manage services by sending requests to the API manually via a web-based GUI like the Management Console, a command-line interface (CLI), or programmatically via an SDK. 

Virtual machines have a special feature: you can connect to virtual machines through SSH, for example, and gain administrator access. This means you can install any software you like on a virtual machine.

Other services, like the NoSQL database service, offer their features through an API and hide everything that’s going on behind the scenes.

Users send HTTP requests to a virtual machine. This virtual machine is running a web server along with a custom PHP web application. The web application needs to talk to AWS services to answer HTTP requests from users. For example, the application might need to query data from a NoSQL database, store static files, and send email. Communication between the web application and AWS services is handled by the API

- The following services are covered in detail in our book:
  * `EC2`—Virtual machines
  * `ECS` and `Fargate`—Running and managing containers
  * `Lambda`—Executing functions
  * `S3`—Object store
  * `Glacier`—Archiving data
  * `EBS`—Block storage for virtual machines
  * EFS—Network filesystem
  * RDS—SQL databases
  * DynamoDB—NoSQL database
  * ElastiCache—In-memory key-value store
  * VPC—Virtual network
  * ELB—Load balancers
  * Simple Queue Service—Distributed queues
  * CodeDeploy—Automating code deployments
  * CloudWatch—Monitoring and logging
  * CloudFormation—Automating your infrastructure
  * IAM—Restricting access to your cloud resources

### Elastic Load Balancing (ELB)

The load balancer distributes traffic to a bunch of virtual machines. Requests are routed to virtual machines as long as their health check succeeds. You’ll use the Application Load Balancer (ALB), which operates on layer 7 (HTTP and HTTPS).

### Elastic Compute Cloud (`EC2`)

This is IaaS (Infrastructure as a Service).

The EC2 service provides **virtual machines**. You’ll use a Linux machine with a distribution called Amazon Linux to install Apache, PHP, and WordPress. You aren’t limited to Amazon Linux; you could also choose Ubuntu, Debian, Red Hat, or Windows.

Virtual machines can fail, so you need at least two of them. The load balancer will distribute the traffic between them. In case a virtual machine fails, the load balancer will stop sending traffic to the failed VM, and the remaining VM will need to handle all requests until the failed VM is replaced.

### Relational Database Service (RDS)

Provided a managed SQL database.

WordPress relies on the popular MySQL database. AWS provides MySQL with its RDS. You choose the database size (storage, CPU, RAM), and RDS takes over operating tasks like creating backups and installing patches and updates. RDS can also provide a highly available MySQL database using replication.

### Elastic File System (EFS)

WordPress itself consists of PHP and other application files. User uploads—for example, images added to an article—are stored as files as well. By using a network filesystem, your virtual machines can access these files. EFS provides a scalable, highly available, and durable network filesystem using the NFSv4.1 protocol.

### Security Groups

It is a virtual firewall for your resources.

firewall rules = security groups

Control incoming and outgoing traffic to your virtual machine, your database, or your load balancer with a firewall. For example, use a security group allowing incoming HTTP traffic from the internet to port 80 of the load balancer. Or restrict network access to your database on port 3306 to the virtual machines running your web servers.

### CloudFormation

Automate the setup process.

## Interacting with AWS

The tools available for communicating with AWS’s APIs: the Management Console, the command-line interface, the SDKs, and infrastructure blueprints.

The **AWS Management Console** is a GUI working on the web browser. It is used to interact with AWS API.

---

The **command-line interface (CLI)** allows you to manage and access AWS services within your terminal.

If you want to automate parts of your infrastructure with the help of a continuous integration server, like Jenkins, the CLI is the right tool for the job.

You can even begin to automate your infrastructure with scripts by chaining multiple CLI calls together.

---

AWS support SDK for: JavaScript, Java, Python, etc

SDKs are typically used to integrate AWS services into applications. If you’re doing software development and want to integrate an AWS service like a NoSQL database or a push-notification service, an SDK is the right choice for the job. Some services, such as queues and topics, must be used with an SDK.

---

A **blueprint** is a description of your system containing all resources and their dependencies. An **Infrastructure as Code tool** compares your blueprint with the current system and calculates the steps to create, update, or delete your cloud infrastructure.

Consider using blueprints if you have to control many or complex environments. Blueprints will help you to automate the configuration of your infrastructure in the cloud. You can use them to set up a network and launch virtual machines, for example.

Automating your infrastructure is also possible by writing your own source code with the help of the CLI or the SDKs. Doing so, however, requires you to resolve dependencies, to make sure you are able to update different versions of your infrastructure, and to handle errors yourself. As you will see in chapter 4, using a blueprint and an Infrastructure-as-Code tool solves these challenges for you.

## AWS Account

You can attach multiple users to an account if multiple people need access to it; by default, your account will have one **root user**.

Muốn tạo AWS account phải có số điện thoại chứng minh identity (send a SMS message về điện thoại để xác nhận) và thông tin credit card.

The **AWS account name** has to be globally unique among all AWS customers.

- Sau khi tạo tài khoản thì
  * Creating a budget alert to keep track of your AWS bill
  * Tạo alert cho budget

## The Dashboard Interface

- The navigation bar
  * Terminal—Spin up a terminal with access to your cloud resources in the browser.
 
## WordPress on AWS

WordPress is written in PHP and uses a MySQL database to store data. Besides that, data like user uploads is stored on disk. Apache is used as the web server to serve the pages. With this information in mind, it’s time to map your requirements to AWS services.