# ⚛️ ❤️ 🐳 Docker - Creating a Production-Grade Workflow

## Introduction

We've learned about Docker containers, images, Docker CLI and Docker Compose. We still don't know a lot about how to actually use Docker in a production-type environment. How do we develop an application that uses Docker and then eventually push it to some outside hosting service, like AWS or Digital Ocean? Today, we're going to learn how to do that by walking through an entire workflow that uses Docker to author and publish the application.

'Flow' refers to the cycle of development, testing and deployment.

Our workflow will be created around a GitHub repo, then deployed to an outside hosting service. Our repo will have two branches, `feature` and `main`. The `feature` branch is a development branch. We'll only pull and push code to the feature branch. We'll create a pull request when ready to merge changes to `main`. On the pull request, our code will be sent to Travis CI. This tool will run (pre-generated) tests on it. If the tests pass, the changes will be merged to the main branch and Travis CI will take our codebase and push it to AWS Elastic Beanstalk.

![high-level](/Journey/081/high-level.png)

![detailed-1](/Journey/081/flow-1.png)
![detailed-2](/Journey/081/flow-2.png)

To execute this workflow we _do not_ have to make use of Docker. However, it'll make some of these tasks a lot easier. Today's lesson is how all these tools (i.e. Docker, AWS, Travis CI can work together).

## Prerequisite

- Previous lessons in this course, or a working knowledge of all the Docker tools mentioned above.
- Knowledge of version control technologies, like GitHub
- Some knowledge of workflows - at least knowing what they are
- An AWS account
- Familiarity with React?

## Use Case

- A workflow more closely resembles how you'd use Docker in a commercial environment
- Creating a CICD pipeline for your app

## Cloud Research

- There were some problems installing the React app, and [this](https://stackoverflow.com/questions/64963796/create-react-app-is-not-working-since-version-4-0-1) was very useful. You have to uninstall React if it was globally applied, clear the cache, and then try again...
- As this is a course that doesn't required the user to be _too_ familiar with Node or React, the tutor does over some basic commands (start, test and build), which I won't cover here.

## Try yourself

Create a new directory, `flow`, and follow along!

### Step 1 — Add dependencies for project setup

- Node.js
- React app generation `npx create-react-app frontend`
- Create development container with Docker, `Dockerfile.dev`. This extension should make it clear what its purpose is. In future, we'll create a `Dockerfile` for the production container.

![Dockerfile dev](/Journey/081/dockerfile-dev.png)

- `docker build .` won't recognise `Dockerfile.dev` so we need to modify the command to:
  `docker build -f Dockerfile.dev .` The `-f` means we're specifiying the file we want to use

- We have duplicated dependencies. In the past, we didn't install any of our dependencies into our working folder, instead we relied on our Docker image to install those dependencies when the image was initially created. We really don't need two copies of the dependencies! The large `node_modules` folder is included in the React project directory, so remember to delete it.

- If you run `docker build -f Dockerfile.dev .` again, notice how much faster it is.

### Step 2 — Start up a container

Due to an update in the Create React App library, we need to change how we start up our containers. You need to add the `-it` flag to run the container in interactive mode. Any time we want to expose a port from our Docker image/container to our machine, we have to add on that `-p` flag to map out the ports we want to expose.

- `docker run -it -p 3000:3000 {IMAGE_ID}`

- This should automatically open the React app in the browser.

- Make a small change to the text in React app file `/src/App.js`. Notice how it doesn't render this change to the browser!

Remember that when we create an image we're taking a "snapshot" of all the source code in our source directory, and building an image with that snapshot, so no hot reload here. We probably don't want to rebuild the image every time we want to change our source code...

### Step 3 — Docker Volumes

How can we get a change in the source code to show up inside a running Docker container, without us having to stop it, rebuild the image, and then restart the container?

Rather than doing a straight `COPY` as defined in the Dockerfile, we're going to adjust the `docker run` command that we use to start up our running container. We're going to make use of a Docker volume. This is a "placeholder" of sorts, inside our Docker container, and we're no longer going to copy over the entire contents of our `/src` or `/public` directories. Instead, we're going to put in a kind of reference instead. It'll point back to our local machine. It's comparable to the logic behind the port mappings we've set up before. We're mapping a folder inside the container to a folder outside the container.

**Syntax:**

- `docker run -it -p 3000:3000`
- `-v /app/node_modules` put a bookmark on the node_modules folder
- `-v $(pwd):/app` map the path to the working directory (pwd) into the `/app` folder. The `:` is what sets the map.
- `{IMAGE_ID}`

Put together, it looks like this:
`docker run -it -p 3000:3000 -v /app/node_modules -v $(pwd):/app sha256:3c716cf1f2`
🤨

Hot reload (via Docker) is back!

### Step 4 - Use Docker compose to create a shorthand for the docker run command in the last step

This is a very long winded command to potentially have to keep calling, so we're going to create a shorthand for it.

- Inside the root directory of the project folder, create a `docker-compose.yml` file.

![docker compose](/Journey/081/docker-compose.png)

Now all the mapping is set up, do we really need that `COPY . .` instruction in `Dockerfile.dev`? Quick answer is 'yes', just in case we make changes to the `docker-compose.yml` in future, and to avoid problems when when we build out our production container in the future, and we want to use `Dockerfile.dev` as a template.

### Step 5 - Executing tests locally

- run `docker build -f Dockerfile.dev .`, grabbing the image id
- To override the existing command (currently `CMD ["npm", "run", "start"]`)we just need to append the command we want to execute on the end of the docker run command:
  `docker run {IMAGE ID} npm run test`

![local test](/Journey/081/test-working.png)

Regarding the options at the bottom of the output, if you hit enter, nothing will happen - a test run will not be triggered. We need to hook up to STDIN with the `-it` option.

![-it](/Journey/081/test-with-it-flag.png)

Hitting enter will work this time, and you'll get full interactivity. Except as before, there's no volumes set up so any changes we make to the test suite won't be updated in real time...

### Step 6 - Docker compose for running tests

We're going to add a second service to our `docker-compose` file, solely for the purpose of testing. There's also a way of piggybacking into an already open container with `exec` but it's not the best solution in the long run.

![update to docker-compose](/Journey/081/compose-tests-1.png)

![output](/Journey/081/compose-tests-2.png)

Make a change in the `App.test.js` file. Here, we just duplicated the test.
![app.test.js](/Journey/081/update1.png)

It should update and run the tests live.
![updated test](/Journey/081/update2.png)

The downside with this solution is that we're getting all the test output to the logging interface of the docker-compose and we don't have the ability to enter any STDIN/OUT to that container, e.g. we can't hit enter to get the test suite to rerun, can't hit `w` to get any of the options inside the test suite to appear, etc.

Can we attach directly to this container and add in some custom input by hitting `enter` or `w`, etc to manipulate the test suite?

- Open up a second terminal window. We're going to run the `attach` command here
- Run `docker ps` to see all running containers. Grab the ID for the test container
- Run `docker attach {IMAGE ID}`

![attach-1](/Journey/081/attach1.png)

This still isn't allowing us to interact with the test suite.

When we attach, we only attach to the _primary_ running process. If the process that we want isn't the primary process, then it can't attach.
![attach-2](/Journey/081/attach2.png)

So for Docker, this is as good as it gets for testing.

### Step 7 - Production server with NGINX

NGINX is a very popular, lightweight server. We're going to use this to serve up the production version of our container. We're going to create a Dockerfile with a multistep build process. We're going to have two different blocks of configuration: a build phase and a run phase (where we pass on our build to be deployed to NGINX).

- Create a `Dockerfile`

![prod dockerfile](/Journey/081/prod-dockerfile.png)

Note that you don't need to define a RUN command for the nginx image, it will run the build automatically. The destination folder as defined in line 9 is what is suggested in the NGINX documentation on Docker Hub.

- Run `docker build .`
- `docker run -p 8080:80 {IMAGE ID}` - 80 is nginx's default port
- Go check this is rendering in localhost!

## ☁️ Cloud Outcome

I'm feeling very appreciative that I know my way around a React app, and that I've previously build workflows to deploy React builds to the cloud. There was a _lot_ to take in today without having to learn React on top of it! I also learned about some of the limitations of Docker, and what might work for one use-case may not work for another.

## Next Steps

Tomorrow we'll hook this up to GitHub, Travis CI and AWS for the CD part of the workflow.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1361352703365042178?s=20)
