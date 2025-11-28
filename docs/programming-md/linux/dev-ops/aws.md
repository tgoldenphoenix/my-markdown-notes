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

## What can you do with AWS?

The web shop consists of dynamic content (such as products and their prices) and static content (such as the company logo). Splitting these up would reduce the load on the web servers and improve performance by delivering the **static content** over a **content delivery network (CDN)**.

The application running the web shop can be installed on virtual machines. Using AWS, John can run the same amount of resources he was using on his on-premises machine but split them into multiple, smaller virtual machines at no extra cost. If one of these virtual machines fails, the load balancer will send customer requests to the other virtual machines. This setup improves the web shop’s **reliability**.

load balancer for reliability, high availability

---

You defines a virtual network in the cloud and connects it to the corporate network through a virtual private network (VPN) connection.