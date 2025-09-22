# Docker & Kubernetes notes

## Docker

`docker ps -a`

Build app vào docker, run locally, sau đó deploy lên aws.

Khi dùng docker, app không bị phụ thuộc vào host machine. Nó chạy local host cũng giống như chạy trên cloud. Không có chuyện "chạy trên máy này nhưng không chạy trên máy kia."

A **Dockerfile** is a text file containing instructions for building your source code. Từ dockerfile build ra **image**.

Image is your app (code, runtime, environment variables, libraries, configuration files). Image dựa trên một **base image** like Ubuntu base image.

A **Docker container** is a runnable instance of a Docker image. It is the actual, isolated environment where an application executes. Multiple containers can be created from the same image, each running independently with its own isolated environment.

## commands

`docker --version`

`docker init`

## Kubernetes

Kubernetes (or K8s) is a **container orchestration tool**. It keep your docker containers up. If the containers become unhealthy, k8s can take them out behind the barn and shoot them, then spin up another container(s) automatically.

Compare Docker vs Virtual Machines [here](https://www.freecodecamp.org/news/docker-vs-vm-key-differences-you-should-know/)

The error [here](https://github.com/docker/for-mac/issues/6898)

azure data studio [doc](https://learn.microsoft.com/en-us/azure-data-studio/quickstart-sql-server)

Công ty có thể có private **container repository**.

The container is its own isolated operating system.

## Terms

[containerized applications](https://cloud.google.com/discover/what-are-containerized-applications): applications run in isolated packages of code called containers.

