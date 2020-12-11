**Add a cover photo like:**
![placeholder image](https://via.placeholder.com/1200x600)

# NGINX Proxy Pt 2

## Introduction

This is the next stage in setting up our NGINX proxy

## Prerequisite

Familiarity with the AWS console

## Use Case

- AWS ECR (Elastic Container Registry) is a service that allows you to store Docker images.
- It's very similar to Docker Hub (which allows you to build and push images), except it's within Amazon and allows us to control access with IAM policies

## Cloud Research

1. Add a new project in GitLab for proxy application
2. **Setup AWS IAM account with new ECR repo**
3. Create a Dockerfile and NGINX configurations
4. Create a CI cluster account for GitLab to authenticate with AWS in order to push our image
5. Set up pipeline jobs in GitLab to run workflow

## Try yourself - Set up AWS for NGINX Proxy

### Step 1 — Create a new registry in ECR

- Log in to AWS console with administrator account (setup on day 42 of #100DaysOfCloud)
- Head over to ECR
- Make sure you're in the right Region - us-east-1 for this course
- Click `create a repository`
- Give it a name (it's recommended to give it the same name as your GitLab repo for easy identification)
- Select `scan on push` - this will scan your image for vulnerabilities every time you push.
- Click `create repository`

### Step 2 — Create a new IAM policy

The only thing this user needs to do is push a new image to AWS ECR

- Go to `IAM` in a new tab
- Click on `policies` > `create policies` > `JSON`
- Copy and paste the policy provided as part of the course. It allows pushing to the names repository _only_.

![push to specific repo only policy](/Journey/045/policy.png)

`Resource` names the specific repo we're using. The user that has this policy assigned can only push to our ECR repo.

`Action` - the `ecr: GetAuthorizationToken` is required for the user in our GitLab CI to be able to authenticate the ECR. We need to allow this in order to get the credentials to log into Docker later on (when we create our CI/CD pipeline)

- Click `review policy`
- Name it `RecipeAppApi-ProxyCIPushECR`. It's long, but descriptive!
- Click `create policy`

### Step 3 — Create a new (programmatic) IAM user with grant least privilege principles

The user is what the CICD tools are going to use to login, to push our image

- Go to `Users` > `Add user`
- Name it `recipe-app-api-proxy-ci`
- Under `Access type` only click `Programmatic access`, as this user is programmatic and not a human who will log in to AWS to access the console. This user will connect to the programmatic APIs for AWS.
- Click `Next: Permissions` > `Attach existing policies directly`
- Select the dropdown `Filter policies` and tick `Customer managed` and add the policy we created in Step 2.
- Click `Next: Tags`, leave blank; click `Next: Review` > `Create User`

- Leave this keys page open for next steps where we set the variables!

### Step 4 - Configure GitLab project with appropriate variables

- Open the project in GitLab
- Go to `Settings` > `CI / CD` > `Variables`
  This is where we can set variables on the project that are accessible by our CI/CD tool. It's commonly used for things like credentials for cloud services, and that's exactly what we're going to use it for!
- Click `Add Variable`
- `Key` = `AWS_ACCESS_KEY_ID`
- `Value` = access key ID for the IAM user created in Step 3
- `Flags`
  - select `Protect variable` (as we set up protected branches and tags in a previous session), so nobody can access our credentials on different branches.
  - select `Masked` to mask the variable from being outputted to the CI/CD logs
- Click `Add variable` to save and close
- Add second variable
  - `Key` = `AWS_SECRET_ACCESS_KEY`
  - `Value` = as before, but with the secret access key
  - Select `Protected` and `Masked` as before, then save.
- Add third variable for our ECR repo
  - `Key` = `ECR_REPO`
  - `Value` = URI from the ECR repo we created in AWS
  - Select `Protected` then save. You can't mask, as the value is too long.

## ☁️ Cloud Outcome

✍️ (Result) Describe your personal outcome, and lessons learned.

## Next Steps

✍️ Describe what you think you think you want to do next.

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[Twitter](link)
