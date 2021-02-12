# Building Custom Images Through Docker Server

## Introduction

Dockerfile - a plaintext file with configuration code inside of it: what different programmes it's going to contain, and what it does when it starts up as a container
Docker Client - Docker CLI
Docker Server - Takes the Dockerfile, builds a usable image allowing us to start up a new container

## Prerequisite

- Some basic familiarity with Docker
- Docker for Desktop (or equivalent) installed on machine

## Use Case

Building our own custom images allows us to run our own applications inside of our own personalised containers.

## Cloud Research

- Learned what a Dockerfile is, and how it relates to the Docker Client and Docker Server.

## Try yourself - Create an image that runs redis-server

Create a new directory called `redis-image` and open it up in your code editor.

### Step 1 — Create a Dockerfile

- Notice the syntax, and the absence of an extension name

Here's what we're going to write inside of the Dockerfile:

![Dockerfile](/Journey/078/dockerfile.png)

- Every line begins with an instruction telling Docker Server what to so, i.e. `FROM`, `RUN` and `COMMAND`. The rest is the arguement relatvice to that instruction.

`FROM` specifies the Docker image we want to use as a base
`RUN` is used to execute some command while we are preparing our custom image
`CMD` (Command) specifies what should be executed when our image is used to start up a brand new container

There are many more instructions (which will be covered by the course), but these are the most important ones to know and understand

#### An analogy

Writing a Dockerfile == is like being given computer with no OS and being told to install Chrome.

What are the steps you'd need to take to install it?

**Specify a base image**

- Install an OS

**Run commands to install additional programmes**

- Start the default browser
- Navigate to chrome.google.com
- Download installer
- Open file/folder explorer
- Execute chrome_installer.exe

**Command to run on setup**

- Execute chrome.exe

#### Why did we use alpine as a base image?

- Why do you use MacOS, Windows or Ubuntu?
- They come with a preinstalled set of programes that are useful to you

**Answer:** Alpine is great for installing and running Redis.

#### What does the RUN line mean?

`apk add --update redis` is not a Docker command. APK is an Apache package manager that comes pre-installed on the Alpine image. We can use that package manager to automatically download and install Redis for us

### Step 2 — Build image

- From inside the `redis-image` directory in the terminal, run `docker build .` (the `.` is the build context)

![build](/Journey/078/docker-build.png)

[1/2] - The docker server looked at our local build cache, and checked to see if it had ever downloaded an image called alpine before

[2/2] Then it downloaded redis: it looked at the image that came out of the previous step, then it took that image and created a new container out of it (though I don't have that output on my computer - in fact it's very different to what the instructor has...), it then executed `apk add --update redis` as its primary running process. Then it closed down that image.

We have also specified our startup command, but my output didn't produce a `[3/3]`, so...

With every instruction we:

- take the image that was generated during the previous step
- create a new container out of it
- execute a command in the container/make a change to its file system
- look at that container
- take a snapshot of its file system
- save it an an output for the next instruction in the chain
- when there are no more instructions to execute the image that was output is outputted as the final image that we really care about

If you were to run this command again, it would used the cached version of the image(s)stored on the local machine!

For this reason, in a large Dockerfile you might want to put changes towards the bottom of the file, to take full advantage of these cached images (saving time and compute power)

### Step 3 — Run image

- With the image ID given as a result of the build, run `docker run {image-ID}`. This creates your container, inside of which is running redis-server

![output](/Journey/078/docker-run.png)

The last message you see should be "Ready to accept connections"

- Hit `ctrl` + `c` to exit

### Step 4 - Tagging the image

Let's run this image with a customisable name.

Convention:

- your Docker ID
- `/`
- repo/project name
- `:`
- version

Community images with shorter names e.g. `hello-world`, `busybox`, etc are open sourced for popular use

`docker build -t notwaving/redis:latest .`

![tag](/Journey/078/docker-tag.png)

Now you can run it with the tag (omitting the version automatically applies the latest version):

![run with tag](/Journey/078/run-tag.png)

Technically, the "tag" in question is the version number, the rest of the command is the project repo/name

## ☁️ Cloud Outcome

- Broke down what every line of code in the Dockerfile meant, found an analogy to explain how and why.

## Next Steps

Making a real project with Docker!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1360304330423738368?s=20)
