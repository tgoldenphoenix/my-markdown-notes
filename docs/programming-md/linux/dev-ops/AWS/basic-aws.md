# AWS Basics Notes

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

AMI (Amazon Machine Image). Khác với IAM.

- **AWS Management Console**: the web-based, graphical user interface (GUI) that serves as the central point for managing and accessing all services and resources within your Amazon Web Services (AWS) account.
- AWS Systems Manager Session Manager: the web-based console used to connect to your EC2 instance

## S3

Bucket (`S3`) is the term AWS uses for what we would call a directory. 

a bucket name must be unique across the entire S3 system

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

## Billing Cost & Estimation

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

Keep in mind that this is only an estimate. You’re billed based on actual use at the end of the month. Everything is on demand and usually billed by seconds or gigabyte of usage. The following factors might influence how much you actually use this infrastructure:

- Traffic processed by the load balancer—Expect costs to go down in December and in the summer when people are on vacation and not looking at your blog.
- Storage needed for the database—If your company increases the amount of content in your blog, the database will grow, so the cost of storage will increase.
- Storage needed on the NFS—User uploads, plug-ins, and themes increase the amount of storage needed on the NFS, which will also increase the cost.
- Number of virtual machines needed—Virtual machines are billed by seconds of usage. If two virtual machines aren’t enough to handle all the traffic during the day, you may need a third machine. In that case, you’ll consume more seconds of virtual machines.

## Alternatives to AWS

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
  * `EC2` (Elastic Compute Cloud) — Virtual machines
  * `ECS` and `Fargate`—Running and managing containers
  * `Lambda`—Executing functions
  * `S3`—Object store
  * `Glacier`—Archiving data
  * `EBS`—Block storage for virtual machines
  * `EFS` (Elastic File System) —Network filesystem
  * `RDS` (Relational Database Service) —SQL databases
  * `DynamoDB`—NoSQL database
  * `ElastiCache`—In-memory key-value store
  * `VPC`—Virtual network
  * `ELB` (Elastic Load Balancing) —Load balancers
  * `Simple Queue Service`—Distributed queues
  * `CodeDeploy`—Automating code deployments
  * `CloudWatch`—Monitoring and logging
  * `CloudFormation`—Automating your infrastructure
  * `IAM`—Restricting access to your cloud resources

### Elastic Load Balancing (ELB)

The load balancer distributes traffic to a bunch of virtual machines. Requests are routed to virtual machines as long as their health check succeeds. You’ll use the Application Load Balancer (ALB), which operates on layer 7 (HTTP and HTTPS).

### Relational Database Service (`RDS`)

Provided a managed SQL database.

WordPress relies on the popular MySQL database. AWS provides MySQL with its RDS. You choose the database size (storage, CPU, RAM), and RDS takes over operating tasks like creating backups and installing patches and updates. RDS can also provide a highly available MySQL database using replication.

### Elastic File System (`EFS`)

WordPress itself consists of PHP and other application files. User uploads—for example, images added to an article—are stored as files as well. By using a network filesystem, your virtual machines can access these files. EFS provides a scalable, highly available, and durable network filesystem using the NFSv4.1 protocol.

The Elastic File System (EFS) is used to store files and access them from multiple virtual machines. EFS is a storage service accessible through the **NFS protocol**. To keep things simple, all files that belong to WordPress, including PHP, HTML, CSS, and PNG files, are stored on EFS so they can be accessed from all virtual machines.

To mount the Elastic File System from a virtual machine, mount targets are needed. You should use two mount targets for fault tolerance. The network filesystem is accessible using a DNS name for the virtual machines.

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

Common web applications use a database to store and query data. That is true for WordPress as well. The content management system (CMS) stores blog posts, comments, and more within a MySQL database.

WordPress also stores data outside the database on disk. For example, if an author uploads an image for their blog post, the file is stored on disk. The same is true when you are installing plug-ins and themes as an administrator.

## Deleting Infrastructure

Your evaluation has confirmed that you can migrate the infrastructure needed for the company’s blog to AWS from a technical standpoint. You have estimated that a load balancer, virtual machines, and a MySQL database, as well as a NFS capable of serving 1,000 people visiting the blog per day, will cost you around $75 per month on AWS. That is all you need to come to a decision.

Because the infrastructure does not contain any important data and you have finished your evaluation, you can delete all the resources and stop paying for them.

CloudFormation is an efficient way to manage your infrastructure. Just as the infrastructure’s creation was automated, its deletion is also. You can create and delete infrastructure on demand whenever you like. You pay for infrastructure only when you create and run it.