# CI/CD with Travis CI & AWS

## Introduction

Today's lesson concludes yesterday's React/Docker project. We'll connect GitHub, Travis CI and AWS to complete the workflow.

## Prerequisite

- GitHub account
- AWS account
- React/Docker project from yesterday

## Use Case

Completing a CI/CD workflow. Deploying a containerised React app to the cloud with automation.

## Cloud Research

- Tradionally, people use Travis CI for testing or deployment, we're going to use it for both.
- AWS Elastic Beanstalk is the easiest way to get started with production Docker instances, when running a single container.
- The user's request to our React app will be handled by a load balancer that as already been created as a part of the Elastic Beanstalk application. When a request comes into it the load balancer will route that request to a virtual machine that is running Docker (with our application inside of it).
- Elastic Beanstalk monitors the amount of traffic coming in to our virtual machine. As soon as that traffic reaches a certain threshold Elastic Beanstalk will scale out and add more virtual machines to handle that traffic.

## Try yourself

Let's hook it all up!

### Step 1 — Push project folder to GitHub repo

- Create a new repo on Github
  - Give it a name
  - Make it public
  - Don't initialise with a Readme
  - Click `create repository`
  - Grab the link to the repo
- Connect local repo to Github
  - `git remote add origin {link to repo}`
  - `git branch -M main`
  - `git push -u origin main`

### Step 2 — Travis CI setup

- Sign up for an account at `https://travis-ci.org/`
- Choose GitHub or email
- Go into settings to authorise Travis CI access to the repo we added in the last step. In the dashboard we'll see that there are no builds just yet

![Travis CI](/Journey/082/travis-auth.png)

Now we need to create a YAML file. For now, we'll focus on getting our tests to run on Travis

- In the project root file, create `.travis.yml`

![travis.yml](/Journey/082/travis-file.png)

- `sudo: required` - any time we make use of Docker we need to have super user permissions

- `before_install` - things that need to be executed before our tests are run

- `script` contains all the commands that need to be executed to actually run our test suite

The default run command `npm run test` hangs there and never exits. To get it to run the tests and then exit back to the command line add `-- --coverage`

- Push this to GitHub
- Check in TravisCI dashboard

![success](/Journey/082/travis-success.png)

### Step 3 — Create Elastic Beanstalk instance in AWS

Log into AWS console. Navigate to Elastic Beanstalk.

- Click `create application`
- Give it a name
- Set up the platform like this. If you go for Amazon Linux 2 it will look to build from `docker-compose` so don't select this!

![platform options](/Journey/082/platform.png)

- Leave application code options for now

![application code options](/Journey/082/application-code.png)

- Click `create application`

This will take a few minutes - time to make a cup of tea. ☕️

Here it is!

![EBS instance](/Journey/082/elb-instance.png)

...and here's the boilerplate page that comes with th EBS instance
![boilerplate](/Journey/082/boilerplate-ebs.png)

### Step 4 — Configure Travis CI for deployment

Add this deployment code to the `.travis.yml` file

![travis deploy](/Journey/082/travis-deploy.png)

- `bucket_name` - Elastic Beanstalk has automatically created an S3 bucket, so navigate to the S3 page and locate it

- `bucket_path` - this won't be set until the first deployment, so the default is to give it the same name as `app`

At this point of the lesson, we're given the notice that Travis-ci.org is becoming .com and to follow instructions to migrate our account...

### Step 5 — Create and store access keys

- In AWS, create a new IAM user with programmatic access only, with full permissions for Elastic Beanstalk

- In the settings for the repo in Travis, plug them into the environment variables section:

![AWS keys in Travis](/Journey/082/AWS-keys.png)

- Connect it to the travis.yml file like so:

![link to keys in Travis](/Journey/082/correctly-formatted-aws.png)

### Step 6 — Expose ports through the Dockerfile

- Add the instruction `EXPOSE 80` to the `Dockerfile`:

![config](/Journey/082/expose-80.png)

In the EBS console:
![pending](/Journey/082/docker-aws.png)

This is taking some time and it's pancake day, so now's a good time to get those pancakes in! 🥞

![ebs success](/Journey/082/aws-success.png)

![bucket success](/Journey/082/bucket-success.png)

TADA! 🎉

### Step 7 — Test the workflow

- Create new feature branch in the project repo, and make a change in /src/App.js
- Add, commit and push to the new feature branch on GitHub
- In GitHub, create a pull request (notice TravisCI runs checks on the code)
- Merge the PR
- Navigate to the Travis console to see the workflow being run
- Be patient, go to the EBS dashboard and when it's been deployed...
- Refresh the EBS bucket url

![final change](/Journey/082/final-change.png)

![freedom!](https://media1.giphy.com/media/5jT0jaNDsM6Ik7X9yq/giphy.gif)

### Step 8 - Environment cleanup

- Go to the Elastic Beanstalk dashboard
- Click into the application
- Click `actions` > `delete application`

#### ☁️ Cloud Outcome

Lots of debugging connecting the AWS keys in the `.travis.yml`. The course taught putting quotes around it, which was incorrect syntax, and Travis CI's documentation was unclear. In the end I looked at the course's source code, which was completely different to what was taught in the videos!!!! Yet again, feeling grateful that I was already familiar with AWS and workflows...

## Next Steps

Next, I'm going to prepare for upcoming job interviews, but after that I'll return to build a multi-container application!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1361718824215666689?s=20)
