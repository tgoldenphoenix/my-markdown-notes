# Docker & Kubernetes notes

## Docker

`docker ps -a`

Docker is a software platform that simplifies the process of building, deploying, and running applications by using containerization. It packages an application and all its dependencies (libraries, system tools, code, and runtime) into a standardized unit called a container.

Compare Docker (a container) vs Virtual Machines [here](https://www.freecodecamp.org/news/docker-vs-vm-key-differences-you-should-know/)

The container is its own isolated operating system.

Build app vào docker, run locally, sau đó deploy lên aws.

Khi dùng docker, app không bị phụ thuộc vào host machine. Nó chạy local host cũng giống như chạy trên cloud. Không có chuyện "chạy trên máy này nhưng không chạy trên máy kia."

The `Dockerfile` is a text file containing instructions for building **docker image** from your source code.

`compose.yaml` is for defining and running multi-container Docker applications.

- docker-compose allows you to coordinate MANY containers in the same computer using 1 YAML file instead of manually running commands for each container.
- k8s allows you to coordinate MANY containers in different computers, using MANY YAML files. It's a lot more complicated than docker-compose, but also much more powerful.

Image is your app (code, runtime, environment variables, libraries, configuration files). Image dựa trên một **base image** like Ubuntu base image.

A **Docker container** is a runnable instance of a Docker image. It is the actual, isolated environment where an application executes. Multiple containers can be created from the same image, each running independently with its own isolated environment.

## commands

`docker --version`

`docker init`

## Kubernetes

Kubernetes (or K8s) is a **container orchestration tool**. It keep your docker containers up. If the containers become unhealthy, k8s can take them out behind the barn and shoot them, then spin up another container(s) automatically.

A **pod** typically includes several containers, which together form a functional unit.

**Containers** are standardized executable components that combine application source code with operating system libraries. A container could be a database, a web application, or a backend service…  
In Kubernetes, each container is isolated from other processes and runs on a computer, physical server, or virtual machine. Furthermore, containers are pretty lightweight, fast, and portable because, unlike a virtual machine, containers do not need to include an operating system in each instance and can instead leverage the functionalities and resources of the host machine’s operating system.

A **Kubernetes cluster** is a collection of interconnected nodes that run containerized applications and the Kubernetes control plane. It serves as the fundamental unit for deploying, managing, and scaling containerized workloads within the Kubernetes ecosystem.

- Key components of a Kubernetes cluster:
  * Control Plane (Master Node): This component manages the overall state of the cluster.
  * **(Worker) Nodes**: These are the machines (physical or virtual) that run the actual containerized applications. Một cluster phải có ít nhất một worker node.

- A **pod** is a set of processes running within a cluster node. A pod within a node has:
  * A local IP address.
  * One or more Linux containers. For instance, Docker is commonly used as a container runtime.
  * One or more volumes that are associated with these containers are persistent storage resources.
  * Simply put, a Kubernetes pod is a collection of containers

Công ty có thể có private **container repository**.

You can absolutely use Kubernetes without Docker as the underlying container runtime. While Docker was historically a common choice for running containers with Kubernetes, Kubernetes itself uses a Container Runtime Interface (CRI) to interact with various container runtimes. This means you can use other compatible container runtimes instead of Docker.  
Popular alternatives include: containerD, CRI-O

Kubernetes deprecated Docker as a default container runtime after v1.20.

## Terms

[containerized applications](https://cloud.google.com/discover/what-are-containerized-applications): applications run in isolated packages of code called containers.

