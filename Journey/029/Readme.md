<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# Docker Commands

## Cloud Research

### Run Commmands

- `docker run` starts a container from an image

  e.g `docker run nginx` will run an instance of the NGINX application on the Docker host if it already exists. If it's not there it will go to Docker Hub and pull the image from there. For subsequent executions the same image will be used.

- `docker ps` lists all running containers
- `docker ps -a`lists all containers, including ones that aren't running
- `docker stop <name>` stops named container
- `docker rm <name>` deletes named container
- `docker images` lists available images
- `docker rmi nginx` removes named image.
  Delete all dependant containers to remove image
- `docker pull nginx` only pulls the image to store on host (but doesn't run it)

A container only lives (i.e. runs) as long as the process inside it is alive. It doesn't sit in an idle state like a VM. There is no process or application living in it by default.

- To execute a command inside the image:
  `docker exec <name> cat /etc/hosts` (for example)

- `docker run -d <name>` runs the container in detached mode. It'll run it in the background and you'll be returned to your prompt immediately.

- `docker attach <name/id*>` reattaches the running container

N.B. if using ID in a Docker command, you only need to need to use the first few characters of it. Just enough to differentiate it from the other IDs.

## Social Proof

[Twitter](link)
