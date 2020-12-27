![what a difference a second time makes](/Journey/055/first-go.png)

# 🤖 Cloud Resume Challenge - Github Action for the Back End

## Introduction

It's the day after Boxing Day, it's time to get this project completed! I've got an Action in mind, and I've done similar for the front end (day 47), so I hope this is smooth sailing... To recap, this action will create a CI/CD pipeline, which automates a SAM build and deploy every time the developer pushes code.

## Prerequisite

- CloudFormation template in project folder
- Project folder in a GitHub repository
- Working knowledge of YAML

## Use Case

- Creates a pipeline between the developer and the client. At the moment, `sam build` and `sam deploy` are two seperate commands run in the terminal by the developer. The appropriate GitHub action will automate this, so everything is built and deployed every time the developer does a `git push` to the GitHub repository.

## Cloud Research

- Safety first, remember to create a new branch to do this in...

- I built all this while referring to my notes for the front end. Really glad I took the time to document the process as I did today's task in a fraction of the time!

## Try yourself

### Step 1 — Identify a GitHub Action from the Marketplace

![github action](/Journey/055/github-action.png)

Thinking of npm packages and how I judge the safety and efficacy of them, I decided to go with [the most popular one](https://github.com/marketplace/actions/aws-sam-github-action). If it's popular it's more likely that it's stable and being maintained regularly...

### Step 2 — Create and configure .github/workflows/main.yml in root of project directory

![Screenshot](https://via.placeholder.com/500x300)

- Copy and paste the github action YAML into main.yml
- Remember to configure (in this case, AWS region and Python version) before saving

### Step 3 — Set Secrets on Github

`Settings` > `Secrets` in the repo as you did before on the front end.
For this Action we only need to know AWS access key and secret access keys.

### Step 4 - Push main.yml to Github

![watch it live](/Journey/055/live-build.png)

- Remember to merge with the main branch once you've succeeded

## ☁️ Cloud Outcome

- Greatful to Past Sam for documenting the process - it took about 30 mins to implement today!

## Next Steps

Good thing I planned this project out on a Trello board - I need to write a short blog to summarise the experience!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1343169830548086786?s=20)
