<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# Docker Images, Env Variables, CMD vs Entrypoint

## Docker Images

### Use Case (Image Creation)

- The service you want to use as part of your app doesn't already exist on Docker Hub.
- You decide that the app you're developing will be Dockerized for ease of shipping and deployment.

#### Create

![dockerfile](/Journey/031/dockerfile.png)

Create a `Dockerfile` and write down the instructions for setting up your application in it.

![dockerfile](/Journey/031/instructions.png)
_N.B. Everything on the left in capitals is an instruction; everything to the right are arguments to those instructions._

![dockerfile](/Journey/031/tut1.png)

![dockerfile](/Journey/031/tut2.png)

#### Build

Build your image using the the Docker build command. Remember to be in the same directory. The `-t` is a tag name for the image.
`docker build -t <account-name>/<image-name>` This will create an image locally (on your system).

#### Layered Architecture

Docker builds images in a layered archictecture: each line of instruction in the build command creates a new layer, with only the changes from the previous layer.
In our Dockerfile example above:
Layer 1 is a base Ubuntu OS
Layer 2 installs the apt packages
Layer 3 changes in pip packages
Layer 4 copies the source code over
Layer 5 updates the entrypoint with `flask` command
Each layer only stores the changes from the previous layer, and this is reflected in the size too, i.e. small.
`docker history <image-name>` displays the layers and their respective sizes.

#### Failure

Docker caches the layers built; if one of the layers fails, you can fix the issue, and pick the build again from where you left off. Docker reuses the cached layers to build the remaining layers. So when you update the source code of your app, Docker only needs to rebuild that, and not the entire app.

#### Push

To make it available on the Docker registry, run the push command:
`docker push <account-name>/<image-name>`
You'll need to be logged in - `docker login` - to your account to do this.

## Environment Variables

e.g. `export APP_COLOR=blue; python app.py`

Find the ENV variable in container that's already running:
`docker inspect <image-name>`

ENV Variables in Docker:
`docker run -e APP_COLOR=blue <image-name>`

## Command vs Entrypoint

When you `run` a container, why does it exit immediately after doing so?

- Unlike virtual machines, containers aren't meant to host an OS.
- Containers are meant to run a _specific_ task or process.
- A container only lives as long as the process inside it is alive.
- A way around this is to add another command and option to the run command, e.g. `docker run ubuntu sleep 5`. This will override the default command specified within the image.

![command-1](/Journey/031/cmd1.png)
![command-2](/Journey/031/cmd2.png)

The entrypoint instruction is like the cmd instruction, except you can specify the programme will run when the container starts. And whatever you specify on the command line will get _appended_ to the entrypoint.

![entrypoint-1](/Journey/031/entrypoint1.png)
![entrypoint-2](/Journey/031/entrypoint2.png)

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1332036488209571840?s=20)
