# 🐳 Docker/Node.js Project

## Introduction

The goal of this project is to create a tiny Node.js app, wrap it inside a Docker container, and then be able to access that web application from a browser running locally. We're not going to worry about deploying this app right now, we're just focussed on getting a Node.js app working inside of a Docker container. The tutor has placed some common gotchas in there for future reference too.

## Prerequisite

Some familiarity with Node/JavaScript. If you're not, the tutor provides code to use.

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

### Step 2 — Summary of Step

### Step 3 — Summary of Step

## ☁️ Cloud Outcome

✍️ (Result) Describe your personal outcome, and lessons learned.

## Next Steps

✍️ Describe what you think you think you want to do next.

## Social Proof

[Twitter](link)
