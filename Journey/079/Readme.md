# 🐳 Docker/Node.js Project

## Introduction

The goal of this project is to create a tiny Node.js app, wrap it inside a Docker container, and then be able to access that web application from a browser running locally. We're not going to worry about deploying this app right now, we're just focussed on getting a Node.js app working inside of a Docker container. The tutor has placed some common gotchas in there for future reference too.

## Prerequisite

Some familiarity with Node/JavaScript. If you're not, the tutor provides code to use.

Basic networking

## Use Case

- We're now moving onto making real world projects with Docker, and containerising a Node.js app would be a pretty common thing to do.

## Cloud Research

- Create Node JS web app
- Create Dockerfile
- Build image from Dockerfile
- Run image as container
- Connect to web app from a browser

## Try yourself

Make new directory `simpleweb` for this project and open it up in a code editor

### Step 1 — Node Server Setup

Create a `package.json` file like so:

![package.json file](/Journey/079/package-json.png)

... and an `index.js` file comme ça:

![index.js file](/Journey/079/index-js.png)

This requires the Express library that we listed as a dependency in `package.json`. We use the Express library to create the new app (line 3). We set up a single route handler so any time someone visits the route of our app, we send back the string, "Hi there". Finally we need to set the app to listen on a port.

### Step 2 — Make a Dockerfile

![dockerfile-error](/Journey/079/dockerfile-error.png)

Time for our first gotcha! Just adding npm install to the RUN stage produces the error:
`/bin/sh: npm: not found`

There's no copy of npm available on the temp container. We're seeing this because we're using Alpine as our base image. Alpine is a very small image (~5MB), so it has a very limited amount of default programmes included inside it. We'll need to do some additional setup to get this working. We have two options:

1. Build up the image we're using from scratch

2. Find a different base image (that already has Node and npm pre-installed)

At hub.docker.com/explore you'll see Node is listed as an image
Checkout the docs. Go for an image with the `alpine` tag, as Node with Alpine is the most lightweight version of Node you'll be able to grab.

![docker hub](/Journey/079/hub.png)

![updated dockerfile](/Journey/079/node-alpine.png)

- Run `docker build .` again. You'll get a different error...

![idealTree error](/Journey/079/idealTree.png)

v15 of Node causes known issues with the course code. To fix this add `WORKDIR /usr/app` just after the `RUN` line, et voila!

![working for now](/Journey/079/working-for-now.png)

### Step 3 — COPY Local Dependency Files to Temp Container

With older versions of Node, when you're building an image, none of the associated project files you've written (i.e. index.js and package.json) are available to the container by default. However, with v15 if you run `npm install` in a directory without a `package.json`, an empty package.json is now created for you and an error will no longer be thrown. A properly written package.json listing your dependencies is still required to run a Node based application.

Add in a line of configuration to ensure your `package.json` is available _before_ npm install is executed.

- The `COPY` instruction is used to copy files from your local machine to the file system inside the temp container created during the build process.

Syntax:

- `COPY`
- `./` - relative file path to folder to copy _from_ **on your machine**
- `./` - place to copy stuff _to_ **inside the container**

![copy instruction](/Journey/079/copy.png)

- In the terminal add this tag:
  `docker build -t notwaving/simpleweb .`
- Run:
  `docker run notwaving/simpleweb`
  You'll get a message telling you that Node.js is running
- In the browser, go to `localhost:8080`

![no why](/Journey/079/no-why.png)

The container has its own isolated set of ports that _can_ receive traffic, but by default, no incoming traffic to the local machine is going to be directed into the container. We need to set up explicit port mapping

### Step 4 — Set Up Port Mapping

Port Mapping states that any time anyone makes a request to a given port on your network, _take that request and automatically forward it to some port inside the container_. This is only about _incoming_ requests. Your Docker container, by default, can make requests on its own behalf to the outside world. We've already seen that in action - any time you've installed a dependency. This is specified at runtime, not inside the Dockerfile.

**Docker run with port mapping**

`docker run -p 8080:8080 {image name}`(for the image name, remember you created a tag). These ports do not need to be identical. If you're going to change the container port, remember to also change the port you're listening to (index.js in this case).

![console view](/Journey/079/port-mapping-1.png)

![localhost view](/Journey/079/port-mapping-2.png)

![woo hoo!](https://i.pinimg.com/originals/00/ed/7e/00ed7ea3401fe1605ecaffeca76dc7ec.gif)

### Step 5 - Specifying a Working Directory

So we want to set up a shell:

`docker run -it notwaving/simpleweb sh`

This opens a shell inside the container as expected.

If you haven't specified a WORKDIR in the Dockerfile, anything you've copied over has gone into the root directory. This is bad practice in case you get a name conflict and the default root file gets overwritten... We've already specified this directory we want to work in as an error fix for something else, so you'll see within the container's shell, we're already in
the directory `usr/app`, and a quick `ls` will show that our files have been copied there. N.B. if the specified directory doesn't exist, Docker will create one for you.

![usr/app as WORKDIR](/Journey/079/workdir.png)

**If you're changing your Dockerfile remember to build (and re-tag) it again before running it.**

### Unncessary Rebuilds

On that last note, remember that your image is a 'snapshot' of what was in the files at the time. The container won't pick up on any changes unless you also create a new build. Every time you rebuild you'll have to pull new temp containers for each step/dependency after the update was introduced, which is pretty inefficient.

### Minimizing Cache Busting and Rebuilds

We're going to split up the COPY instruction into two steps, so that npm install only pulls dependencies for the package.json files. You can make as many changes to index.js as you like, and it won't invalidate the cache.

![copy split](/Journey/079/copy-split.png)

## ☁️ Cloud Outcome

- Experienced the quick and dirty way to do this,
- Learned to fix the common bugs when taking this approach
- Took an even deeper dive into how Dockerfiles are executed
- Learned best practices

## Next Steps

A more complicated app with multiple local containers

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1360659048580993024?s=20)
