# NGINX Proxy Pt 1

## Introduction

I'm currently setting up a DevOps tooling environment and this was the next step

## Prerequisite

- Familiarity with git
- GitHub would help too

## Use Case

- Django requires you to use a proxy in front of your application in order to serve static files
- While Django can already serve static files it's not particularly efficient at doing it.

## Cloud Research

1. **Add a new project in GitLab for proxy application**
2. Setup AWS account with new ECR repo
3. Create a Dockerfile and NGINX configurations
4. Create a CI cluster account for GitLab to authenticate with AWS in order to push our image
5. Set up pipeline jobs in GitLab to run workflow

## Try yourself - Create, configure and clone GitLab project

### Step 1 — Create new project in GitLab

- `Create new project` > `Create blank project`
- Name it
- Make it `public`
- Select `initialise with a Readme`

### Step 2 — Configure project in GitLab

- Go to `settings` > `general` > `visibility, features and permissions`
- Find `pipelines` and change the setting to `Only Project Members`
- Click `save changes`

This prevents the possiblity of sensitive data being exposed in CI/CD pipeline outputs in the event of a bug, etc. To make sure this selection is protected configure it again, but in this different area:

- Go to `Settings` > `CI/CD` > `General Pipelines`
- Uncheck `public pipelines`
- Click `save changes`

![public pipelines](/Journey/044/public-pipelines.png)

### Step 3 — Set up protected branches and tags (so only maintainers can trigger deployments in our project) with a wildcard branch `-release`

- `Settings` > `Repository` > `Protected Branches`

![protected branches](/Journey/044/branches.png)

_Set up protected tags in the same way_

### Step 4 - Clone the project to our local machine with a descriptive Readme

- Click on the project's home button (top left of screen, next to project name)

- Click `clone` with `ssh`

- In Terminal, navigate to the directory of your choice

`git clone <paste ssh link>`

N.B. If your GitLab SSH key is saved under anything other than the default file name `id_rsa`, you're going to have to set this in your ssh/config file:

![config file](/Journey/044/config.png)

- Open the project in VS Code

- Open the README and write a description, plus the names of the environment variables someone would need to run the app.

- Commit this with git

- Push to GitLab with `git push origin`

## ☁️ Cloud Outcome

GitLab project has been created and configured, and is linked to my remote machine

## Next Steps

Step 2 as above, create ECR repo in AWS

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1337108837405511680?s=20)
