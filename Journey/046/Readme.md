![GitHub Actions](/Journey/046/gh-actions.png)

# Cloud Resume Challenge - GitHub Actions with GitHub Learning Lab 🤖

## Introduction

Considering the JavaScript counter is going to take a bit of tinkering I thought I'd put GitHub Actions into the mix, so pushing any changes to my local files automatically update the S3 bucket on AWS.

## Prerequisite

- A working knowledge of GitHub

- A high level understanding of CI/CD, so you understand what this is trying to achieve

## Use Case

As required by the Challenge:

> You do not want to be updating either your back-end API or your front-end website by making calls from your laptop. You want them to update automatically whenever you make a change to the code. (This is called continuous integration and deployment, or CI/CD.)

## Cloud Research

- GitHub Actions create workflows (in YAML, stored in `.github/workflows`) that can handle common build tasks, like CI/CD. You can use an action to compress images, test your code, and push to the site to AWS when the main branch changes.

- Actions are fully integrated with vanilla GitHub, so should be easy to use on an existing repo...

- Before attempting to place a GitHub Action on my project repo, I'm going to practice with a tutorial or two.

- You can create your own GH Actions or incorporate an existing (community created) one in the Marketplace.

- There's a hint in the Challenge rules that I'll need to invalidate the CloudFront cache (so the JavaScript visitor counter can show updates in real time), so I need to include that in my Action.
- There's a warning not to commit AWS credentials to source control. I believe Actions can handle this in [Secrets](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets), so I've made a mental note to research this too.

- Turns out I've not been pushing my branches to main, just merging them locally once I'm happy. I'm about to collaborate on group project with GitHub Organisation, so I've just learnt that in time...

## Try yourself in GitHub Learning Lab - GitHub Actions: Hello World

I found a Hello World [tutorial](https://lab.github.com/githubtraining/github-actions:-hello-world) via GitHub Learning Lab.

### Step 1 — Set up local machine

- `git clone` `hello-github-actions` onto local machine

- Open the repo in your code editor

### Step 2 — Create a `Dockerfile`, open a pull request

- Create a new branch called `first-action`

- Create a folder called `action-a` and a file called `Dockerfile`

- Copy and paste the Dockerfile code from the tutorial into your file

```go
FROM debian:9.5-slim

ADD entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

- Commit and push this to GitHub

- Open and resolve a pull request

### Step 3 — Add an entrypoint script and commit it to your branch

- In this same branch create a file in the `action/a` directory called `entrypoint.sh`

- Add this code to the new file...

```bash
#!/bin/sh -l

sh -c "echo Hello world my name is $INPUT_MY_NAME"

```

...where `$INPUT_MY_NAME` is an environment variable called `MY_NAME`

- Stage, commit and push changes to GitHub

### Step 4 - Add an action metadata file

- On the same branch, create a file called `action-a/action.yml`

- Paste this:

```yaml
name: 'Hello Actions'
description: 'Greet someone'
author: 'octocat@github.com'

inputs:
  MY_NAME:
    description: 'Who to greet'
    required: true
    default: 'World'

runs:
  using: 'docker'
  image: 'Dockerfile'

branding:
  icon: 'mic'
  color: 'purple'
```

- Stage, commit and push changes to GitHub

### Step 5 - Start your workflow file - name and trigger your workflow

- Create a file called `.github/workflows/main.yml`

- To `main.yml` add:

```yaml
# This name appears on any pull request or in the Actions tab
name: A workflow for my Hello World file
# This indicates that your workflow will execute any time
# code is pushed to your repo, using the push event
on: push
```

- Stage, commit and push changes to GitHub

N.B. Workflows piece together jobs, and jobs piece together steps. We'll now create a job that runs an action. Actions can be used from within the same repo, from any other public repo, or from a published Docker container image.

### Step 6 - Run an action from your workflow file

- Add this to `main.yml`:

```yaml
jobs:
  build:
    name: Hello world action
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: ./action-a
        with:
          MY_NAME: 'Sam'
```

`jobs`: is the base component of a workflow run
`build`: is the identifier we're attaching to this job
`name`: is the name of the job, this is displayed on GitHub when the workflow is running
`runs-on`: defines the type of machine to run the job on. The machine can be either a GitHub-hosted runner or a self-hosted runner.
`steps`: the linear sequence of operations that make up a job
`uses: actions/checkout@v1` uses a community action called checkout to allow the workflow to access the contents of the repository
`uses: ./action-a` provides the relative path to the action we created in the action-a directory of the repository
`with`: is used to specify the input variables that will be available to your action in the runtime environment. In this case, the input variable is MY_NAME, and it is currently initialized to "Sam".

- Stage, commit and push changes to GitHub

![Here's where we're at](/Journey/046/first-action.png)

### Step 7 - Trigger the workflow

In the same window, click the workflow you want to view, and you'll see the log (with your name in it)!!!!

![Success!](/Journey/046/success.png)

### Step 8 - Incorporate the workflow

- As a final step, merge this pull request so the action will be part of the `main` branch. Push to `main`

- Delete your branch

![Success!](/Journey/046/yay.png)

## ☁️ Cloud Outcome

The Lab is really nicely integrated with my repo and the next set of instructions being triggered by successful commits is ingenious. It was really helpful to go this deep today but for such a 'simple' action, i.e. outputting a string, it wasn't all that intuitive. Utilising branches in this manner, is a really nice way to build things up slowly before making everything live. Simple code, _really_ powerful.

## Next Steps

This introductary lab is about authoring your own Actions. Next time, I'll look at how to choose, edit, and integrate Actions from the `Marketplace`

## Social Proof

[Twitter](link)
