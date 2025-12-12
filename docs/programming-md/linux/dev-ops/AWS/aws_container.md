# AWS Container

ECS (Elastic Container Service) and EKS (Elastic Kubernetes Service) are both fully managed container orchestration services provided by AWS. They serve the same primary goal: to simplify the deployment, management, and scaling of your containerized applications (Docker images) in the AWS cloud.

The difference lies in the orchestration engine they use and the level of control and complexity they offer.

- ECS offers two key deployment modes for its clusters:
  * ECS on EC2: You manage a cluster of EC2 instances (worker nodes) that run your containers. This gives you more control over the underlying servers (OS, GPU support, reserved instances) but requires you to manage the scaling and patching of the EC2 instances.
  * ECS on Fargate: The serverless option. AWS handles the management of the underlying compute infrastructure. You simply define the CPU and memory for your tasks, and AWS provisions and scales the resources needed.

---

A `cluster` (ECS or EKS) only contains the compute resources (like EC2 instances or Fargate tasks) needed to run your containers. S3 is a separate, managed storage service that exists outside of the cluster.

The purpose of an ECS or EKS cluster is to provide the compute resources (the processing power, memory, and networking) necessary to run your containerized applications.

- The Cluster Contains:
  * Worker Nodes (EC2 Instances): These are the virtual servers that run the Docker engine and the Kubernetes/ECS agent. They are the physical/virtual machines.
  * Pods/Tasks (Containers): These run on the worker nodes.

Cluster Responsibility: Orchestrating CPU, RAM, and Network for the running application code.

While S3 is not part of the cluster, applications running inside the cluster constantly interact with S3. This is the standard cloud pattern for modern applications.