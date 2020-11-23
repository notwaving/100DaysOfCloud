![Docker logo](/Journey/028/docker-hello-world.png)

# 🐳 Docker - Introduction

## Introduction

It's time to learn about DevOps tools, so I'm starting with one I've heard LOTS about, but have no practical experience with: Docker! KodeKloud's `Docker for the Absolute Beginner` course uses hands-on labs with their own in-browser IDE, so no setup on my end is required.

## Prerequisite

If you've ever been on a team and put an app together (whether in a technical or non technical role) you'll be fine for this course.

- Awareness of Virtual Machines and what they do. Some experience would be even better
- Knowledge of operating systems and the kernal, but not essential

## Use Case

- Docker eliminates compatibility matrix issues between different components/dependencies/OS as application architectures grow and change.
- Every time you add a new developer to a project they need to take lots of time to configure their own environments to that of the project, running hundreds of commands in the process. Docker solves this problem.
- Docker makes different development, test and production environments compatible with each other.

## Cloud Research

- This compatibility matrix issue is also known as The Matrix From Hell

![Matrix From Hell slide](/Journey/028/matrix-from-hell.png)

- Docker solves the problem of "it works on my machine"

![meme](/Journey/028/works-on-my-machine.jpg)

- With Docker, you can run each separate component of your app in its own 'container', with its own dependencies and its own libraries; all on the same Virtual Machine (VM) and OS, but in separate environments, aka containers

  ![slide](/Journey/028/what-can-it-do.png)
  You only need to build the Docker configuration once, and all your developers can get started with a simple Docker run command.

- Containers are completely isolated environments - they can have their own processors, interfaces, networks, mounts etc, _except_ they all share the same OS kernal.

- Containers existed long before Docker was created. However, setting these other ones up is hard as they are very low level. Docker offers a high level tool with powerful funtionalities, making it easy for developers to use.

- Docker containers share the underlying kernal, e.g. Linux. Docker can run any number of containers on top of it, as long as they're all based on the same kernal.

- Here are some differences between virtual machines and containers

![slide](/Journey/028/vm-vs-containers.png)

- **The difference between containers and images**
  An image is a package or template. It is used to create one or more containers. Containers run instances off images that are isolated and have their own environments and set of processes.

- **Docker in DevOps**
  In the past, Developers would submit their code to the Operations team with a guide and dependencies to get it to work. If there was an issue, Operations would have to go back to the Developers to resolve it. With Docker, the Devs and Ops work _hand-in-hand_ to transform the guide into a Docker file with both of their requirements. This file is then used to create an image for their applications. This image can now run on any host with Docker installed on it, and is guaranteed to run the same way everywhere. Ops can now use the image to deploy the application.

## ☁️ Cloud Outcome

This was a fantastic introduction to Docker. I've learned loads today - what Docker is and what containers are, and how it all fits into DevOps culture. I didn't go into setting up Docker on different kinds of OS and running it in a Linux virtual machine as there's lots of documentation out there. For the record I'm running Docker Desktop for Mac, but other course material will be done on KodeKloud's IDE.

## Next Steps

Tomorrow we'll run our first Docker commands.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1330865464386445312?s=20)
