# Docker, Kubernetes notes

## Terminologies

Container vs. Virtual Machines

- Virtual Machines (VMs):
  - Hardware virtualization: creates fake CPUs, fake RAM, and a fake hard drive. You have to install a complete operating system (Guest OS) inside every single VM.
  - If you take an Ubuntu VM file created on Windows, you can open it on a Mac or Linux machine running VirtualBox/VMware.
- `Container` (docker):
  - Uses Operating-System-level virtualization (containerization). A container doesn't carry its own kernel - it directly uses the host computer's operating system kernel.
  - A Docker image is locked to an OS kernel. A Linux Docker container requires a Linux kernel to execute system calls (like managing memory or opening files). It cannot run on a Windows kernel natively.
  - Process isolation

- "Wait, then how do I run Linux containers on my Windows laptop using Docker Desktop?"
- Docker Desktop on Windows secretly runs a tiny, lightweight Linux Virtual Machine in the background (using WSL 2 or Hyper-V). When you run a Linux container on Windows, it isn't actually running on the Windows kernel—it is running inside that hidden Linux VM!

image = container package

`Hypervisor` is a software, firmware, or hardware that creates and manages virtual machines by pooling and allocating resources from a single physical machine to multiple guest operating systems. CPU, RAM, storage, network, cards
  
Docker and Kubernetes provides **horizontal scaling**.

- `Vertical scaling` involves increasing the resources of a single server instance.
  * Adding more power to one machine.
- `Horizontal scaling` involves distributing the workload across multiple, often identical, lower-powered servers or instances.
  * Adding more machines to the cluster.
- AWS EC2 (Elastic Compute Cloud) fully offers both kinds of scaling: vertical scaling (scaling up) and horizontal scaling (scaling out).
 
## Docker

`docker ps -a`

`Docker` is a software **platform** that simplifies the process of building, deploying, and running applications by using containerization. It packages an application and all its dependencies (libraries, system tools, code, and runtime) into a standardized unit called a container.

Build app vào docker, run locally, sau đó deploy lên aws.

Khi dùng docker, app không bị phụ thuộc vào host machine. Nó chạy local host cũng giống như chạy trên cloud. Không có chuyện "chạy trên máy này nhưng không chạy trên máy kia."

The `Dockerfile` is a text file containing instructions for building `docker image` from your source code. Từ image file tạo ra containers.

`compose.yaml` is for defining and running multi-container Docker applications.

- **docker-compose** allows you to coordinate MANY containers in the **same computer** using 1 YAML file instead of manually running commands for each container. One API & one database containers run on the same host.
- k8s allows you to coordinate MANY containers in different computers, using MANY YAML files. It's a lot more complicated than docker-compose, but also much more powerful.

- Image is your app (code, runtime, environment variables, libraries, configuration files). Image dựa trên một **base image** like Ubuntu base image.
- A **Docker container** is a runnable instance of a Docker image. It is the actual, isolated environment where an application executes. Multiple containers can be created from the same image, each running independently with its own isolated environment.

Compare Docker (a container) vs Virtual Machines [here](https://www.freecodecamp.org/news/docker-vs-vm-key-differences-you-should-know/)

**Docker Volumes** ensure that data remains intact even when containers are stopped, removed, or replaced.  
You can map a file in your machine with as the volume of a container.

---

There used to be two options for migrating an app to the cloud: infrastructure as a service (IaaS) and platform as a service (PaaS).

- IaaS provides the foundational building blocks of cloud infrastructure. It is the closest model to having an on-premises data center, but over the internet.
- PaaS provides a complete, ready-to-use environment for developing, running, and managing applications. The provider handles everything required to run the code, allowing developers to focus purely on the application logic.
  * Example: AWS Elastic Beanstalk, Azure App Service, Google App Engine, Heroku, AWS RDS (Managed Database).

In the IaaS model, every component runs in its own VM for isolation. VMs are likely to be underutilized and expensive to run (billed monthly).

Docker offers a third option. You migrate each part of your application to a container, and then you can run the whole application in containers using Azure Kubernetes Service (AKS), using Amazon’s Elastic Container Service (Amazon ECS), or on your own container platform in the datacenter.

The application components all run in containers. They are isolated like VMs but lightweight.

You do not need Kubernetes for only one container, and using it would be severe overkill.  
Kubernetes (or any cluster orchestrator) is designed for solving the complex problems that arise when you have many containers, many servers, and high demand. For a single container, the complexity of setting up and maintaining a Kubernetes cluster far outweighs any benefit.

It does take some investment to migrate to containers: you’ll need to build your existing installation steps into scripts called Dockerfiles and build your deployment documents into descriptive application manifests using the Docker Compose or Kubernetes format. But you don’t need to change code, and the end result runs in the same way using the same technology stack on every environment, from your laptop to the cloud.

## Container

Container share host OS. Virtual machine virtual OS.

Containers are built for a particular platform. A container that packages a Linux app for an Arm processor won’t run on Windows, and a container for a Windows app on an Intel processor won’t run on Linux. In a production environment, you’ll need Windows servers to run your Windows apps in containers and Linux servers to run Linux containers. 

## Run Container from Image

k

## Build Source Code into Image

The main goal is to build, package & run an app from source code with only Docker installed; no need to install `node 24.12.0 LTX`, `mvn`, `npm`, `jdk21`, nothing; only need to install Docker (and source code).

- `Java` applications are compiled, so the source code gets copied into the build stage, which generates a JAR file. The JAR file is the compiled app, and it gets copied into the final application image, but the source code is NOT. It’s the same with `.NET`, where the compiled artifacts are Dynamic Link Libraries (DLLs).  
- `Node.js` is different—it uses JavaScript, which is an **interpreted language**, so there’s no compilation step. Dockerized Node.js apps need the Node.js runtime and the source code in the application image.
- With a web application written in `Go`. Go is a modern, cross-platform language that compiles to **native binaries**. That means you can compile your apps to run on any platform (Windows, Linux, Intel, or Arm), and the compiled output is the complete application. You don’t need a separate runtime installed, like you do with Java, .NET, Node.js, or Python, and that makes for extremely small Docker images.

- `docker image build -t image-name .`
  - the `-t` (tag) option is for naming the resulting `image`.
  - `.` is the **building context**; here it is specified as the current directory

## multi-stage `Dockerfiles`

Each stage in a multi-stage build has its own cache.

### `dockerfile` commands & syntax

The `COPY` command has two forms:

Standard Form: `COPY <source> <destination>`

- The source path is relative to the build context (the directory you are in when you run `docker build .`).
- The destination path is an absolute path inside the container image.

Multi-stage Form (for copying from another build stage): `COPY --from=<name_of_stage> <source_path> <destination_path>`

copy files from a previous build stage (like a "builder" stage that contained the compiler and source code) into the final, clean stage.

## Docker Commands

`docker --version` or `docker version`

`docker compose version`

`docker init`

- `docker ps` lệnh `ls` list containers
  - `-a, --all` Show all containers (default shows just running)

`docker stop` Stop one or more running containers

Docker doesn’t automatically clean up containers or application packages for you. When you quit Docker Desktop (or stop the Docker service), all your containers stop and they don’t use any CPU or memory, but if you want to, you can clean up at the end of every chapter by running this command:

`docker container rm -f $(docker container ls -aq)`

`-a` (all) delete all container

If you want to reclaim disk space after following the exercises, you can run this command:

`docker image rm -f $(docker image ls -f reference='diamol/**' -q)`

`docker rm` Remove one or more containers

- `docker image` manage images
  * `docker image ls` List images

`docker container top` lists the processes running in the container. You’ll need to use your own container ID (I’m using a4 as a short form of the ID a41ad305d64d):

```bash
> docker container top a4
UID     PID    STIME    TIME    CMD
root    670    15:48    0:00    /bin/sh
```

## Docker Compose

`Docker Compose` is a tool for defining and running multi-container applications on a **single host machine**.

Kubernetes is an open-source platform designed to automate the deployment, scaling, and management of containerized applications across a cluster of machines.

Docker can run multiple containers and join them in a virtual network so you can run a complex distributed application on your laptop. However, Docker doesn’t connect multiple physical machines together, so you can’t spread containers across servers to get more scale or high availability. For that, you need a platform that manages containers for you—Kubernetes and cloud services. 

## Kubernetes

`Kubernetes (or K8s)` is a **container orchestration tool**. It keep your docker containers up. If the containers become unhealthy, k8s can take them out behind the barn and shoot them, then spin up another container(s) automatically.

A **pod** typically includes several containers, which together form a functional unit.

**Containers** are standardized executable components that combine application source code with operating system libraries. A container could be a database, a web application, or a backend service…  
In Kubernetes, each container is isolated from other processes and runs on a computer, physical server, or virtual machine. Furthermore, containers are pretty lightweight, fast, and portable because, unlike a virtual machine, containers do not need to include an operating system in each instance and can instead leverage the functionalities and resources of the host machine’s operating system.

A **Kubernetes cluster** is a collection of interconnected nodes that run containerized applications and the Kubernetes control plane. It serves as the fundamental unit for deploying, managing, and scaling containerized workloads within the Kubernetes ecosystem.

- Key components of a Kubernetes cluster:
  - Control Plane (Master Node): This component manages the overall state of the cluster. The master node can only run on Linux not Window.
  - **(Worker) Nodes**: These are the machines (physical or virtual) that run the actual containerized applications. Một cluster phải có ít nhất một worker node.

- A **pod** is a set of processes running within a cluster node. A pod within a node has:
  - A local IP address.
  - One or more Linux containers. For instance, Docker is commonly used as a container runtime.
  - One or more volumes that are associated with these containers are persistent storage resources.
  - Simply put, a Kubernetes pod is a collection of containers

Công ty có thể có private **container repository**.

You can absolutely use Kubernetes without Docker as the underlying container runtime. While Docker was historically a common choice for running containers with Kubernetes, Kubernetes itself uses a Container Runtime Interface (CRI) to interact with various container runtimes. This means you can use other compatible container runtimes instead of Docker.  
Popular alternatives include: containerD, CRI-O

Kubernetes deprecated Docker as a default container runtime after v1.20.

- What linux you need to know:
  - Networking
  - software defined storage

## AWS

Some common service: EC2, VPC, S3, Route 53, and SES

AWS is a cloud platform.

Route53 is AWS's Domain Name System (DNS) service.

Đối thủ của aws: **GCP, or Google Cloud Platform** (now referred to as Google Cloud)

### EC2

Amazon Elastic Compute Cloud (EC2) **virtual machines instance** run on aws "cloud" to host backend web server (nodejs, java).  
You can ssh into this cloud virtual machine from your local computer.

An **Amazon Machine Image (AMI)** is an image that provides the software that is required to set up and boot an Amazon EC2 instance.

Elastic Kubernetes service; Elastic Container service (ECS)

ECR (elastic container repository) equivalent to docker hup but owned by AWS. Muốn deploy container app lên AWS thì phải lưu trong ECR.

### S3

S3 is a service that allows you to store files in the cloud. It's a simple service that you can use to store files and serve them to your users.

## Terms

[containerized applications](https://cloud.google.com/discover/what-are-containerized-applications): applications run in isolated packages of code called containers.
