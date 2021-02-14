![shocked pikachu](https://en.meming.world/images/en/thumb/2/2c/Surprised_Pikachu_HD.jpg/600px-Surprised_Pikachu_HD.jpg)

# Docker Compose with Multiple Local Containers

## Introduction

Time to build on what we learned yesterday with something a little more complicated. We're going to learn some more advanced Docker features.

We're going to make a Docker container that contains a web app that simply displays within the browser the number of times that someone has visited this server. A visitor counter, if you will.

- We're going to need a web server (something to respond to HTTP requests, and generate some HTML inside the browser), as a Node App.

- A Redis server will store the number of times the web server has been visited. Remember that Redis is an in-memory data store. You can think of it like a tiny database that sits entirely inside of memory. The only purpose of the Redis server is to contain this visitor count.

We could absolutely store this count inside the Node App, but to make today's class sufficiently complicated we'll make use of Redis.

We're going to have separate Docker containers for both the Node application and one for the Redis server. Each of the Node containers will connect to the Redis instance in that separate container.

## Prerequisite

Docker Desktop (or equivalent)
Familiarity with containers

## Use Case

- This is quite a common use case for Docker containers, but yes if you had a Node app with a visitor counter that you wanted to scale, this might be a solution.

## Cloud Research - additional to today's project

**Dealing with a server crash:**

- Look back to the terminal window to see error message. Will say something like
  `visits_node-app_1 exited with code 0`
- Open second terminal tab and run `docker ps` to see which containers are actually running.

**Automatic container restarts**

Status codes:
0 = we exited and everything is ok. Not counted as a 'failure'.
1, 2, 3, etc = we exited because something went wrong.

| Restart policies | Description                                                          |
| ---------------- | -------------------------------------------------------------------- |
| "no"             | Never attempt to restart this container if it stops or crashes       |
| always           | If this container stops for any reason, always attempt to restart it |
| on-failure       | Only restart if the container stops with an error code               |
| unless stopped   | Always restart unless we (the developers) forcibly stop it           |

In `docker-compose.yml`, underneath the service in question, add `restart: {name-of-policy}`. N.B. Notice this is done per service, i.e. you need to add a restart policy for each server.

`docker-compose ps`
Shows status of running containers inside current working directory.

## Try yourself

- Create a new project directory called `visits`, and follow along with the video to get the code/copy and paste provided code.
- As in yesterday's class, we're going to have `package.js`, `index.js` and `Dockerfile` files.

After writing the code for the three files above, follow along to get everything working.

### Step 1 — Build and tag the app

- `docker build .`

- `docker build -t notwaving/visits:latest .`

### Step 2 — Use Docker Compose

If you try running the container now, you'll get the error that there is no Redis server to connect to. In this section we'll get a separate container running a Redis server.

- `docker run redis`

- In another tab in your terminal, run `docker run notwaving/visits`

You get the same error message. This is because these containers aren't connected. We need to setup some networking infrastructure between the two.

**Options for connecting these containers**

**1. Use Docker CLI's Networking Features**
This is a pain, as the same handful of commands need to be rerun _every single time_ you start up the different containers. We could make a script, but it's going to involved a lot of typing and a lot of thought... Course tutor states he's never seen developers using Docker CLI's built-in networking features to connect two containers together.
**2. Use Docker Compose**
You'll see this used much more frequently. We're using a separate CLI tool called Docker Compose. It's a separate tool that gets installed alongside Docker. It's used to start up multiple Docker containers at the same time, and connect them together with networking. It exists to automate some of the long-winded arguments we were previously passing to `docker run`

- run `docker-compose` to see a list of available commands.

BRING ON THE YAML

![automation](https://blog.chef.io/wp-content/uploads/2012/02/automate-all-the-things-1024x767.png)

![docker-compose file](/Journey/080/docker-compose.png)

`services` = a type of container

`build: .` = look inside the current directory for a Dockerfile and use that to build this image.

Dashes `-` in yaml signify an array, so we can technically map many different ports inside of a single `docker-compose` file for a single service

- Update the `index.js` file to connect with the `redis-server`, as defined in docker-compose. That port is applied by default, but we're being explicit for the purposes of this exercise

![index.js](/Journey/080/index-js.png)

To create an instance of all the containers/images/services listed inside of a docker-compose file, we run the command `docker-compose up`. It's the equivalent of `docker run`.

Previously, we ran two seperate commands to build, then run the image; but if we ever want to rebuild the Docker images listed inside our docker-compose file it's `docker-compose up --build`

- Inside the terminal navigate to the project directory and run `docker-compose up`

![dc-up-1](/Journey/080/dc-up-1.png)

Check out the highlighted line. As stated above, when you create a new set of containers with docker-compose it's going to automatically make a network for you, joining those containers together. After that it creates an image for the Node application, and then:

![dc-up-2](/Journey/080/dc-up-2.png)

`Creating visits_node-app_1` denotes a single instance of our node-app service. Below that you can see a single instance of the redis-server as well. Then we see output from both of those services.

Check it out in `localhost:4001`. Refresh the page to increment the visitor count!

![part3](/Journey/080/part3.png)

### Step 3 — Debug sneaky port mapping gotchas in the source code

There's been an error in the way the port mapping has been set up in the videos, and how it has been documented in the shared source code... Fixes below:

![part1](/Journey/080/part1.png)
In line 8, the first port is local, the second belongs to the container. So far, so good.

![part2](/Journey/080/part2.png)
In the `index.js` file, the listener should be set to the _container's port_, NOT the local port, as taught in the video.

![part3](/Journey/080/part3.png)

In the browser, you need to navigate to local port 4001. Don't use `https` in the URL, as it won't work.

![phew](https://i.pinimg.com/originals/65/06/e0/6506e0b55eba4fceb4d4fb0a9c14bd5a.gif)

### Step 4 — Stopping Docker Compose containers

With one command you can launch and stop multiple containers.

Launch in background:
`docker-compose up -d`

Stop containers:
`docker-compose down`

![stop](/Journey/080/stop.png)

## ☁️ Cloud Outcome

Learned about Docker Compose in great detail, recovering from server crashes and restart policies.

## Next Steps

Create a production-grade workflow.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1361045516960870400?s=20)
