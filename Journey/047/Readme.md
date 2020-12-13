![eleven's the charm](/Journey/047/eleven.png)

# Cloud Resume Challenge - Github Action for Front End 🤖

## Introduction

Time to try applying what I learnt yesterday to get this part of the Cloud Resume Challenge completed. I wanted to try out a GitHub Action that I thought fitted the brief in the Marketplace, couldn't find a tutorial in Labs, so just gave it a go!

## Prerequisite

- Being able to read and interpret errors (dare I say test driven development?) 👾
- A working knowledge of Git and GitHub
- Nerves of steel 💪

## Use Case

- Creates a CI/CD pipeline between source code, GitHub repo, and AWS S3 bucket, so the AWS-hosted website is updated every time you `git push` in the terminal. Automates S3 bucket updates from the command line 🤓
- Invalidates the CloudFront cache so that changes appear on the website instantly. This is being put in place for the JavaScript visitor counter that needs to update in real-time

## Cloud Research

- There was a _lot_ of trial and error today. It took many deployments to get this working. Read the logs, don't be discouraged - there's so much going on under the hood!

## Try yourself

If you're trying this on a real project repository, then remember to create a branch for the purpose of testing.

### Step 1 — Create workflow with Marketplace on local machine

- **Double check that your project repo is _private_**

- Create `.github/workflows/main.yml` in the project repo.
- Copy and paste a GitHub action of your choice into main.yml
  _N.B. At this point you could push this empty file to GitHub and follow Step 3, allowing you to browse the Marketplace for an appropriate Action_
- Modify this code - e.g. variables
  for S3 bucket region, source directory etc - as much as you can. `SOURCE_DIR` caught me out - I mistakenly thought it wanted a local file path - it's looking at the GitHub repo you're already in, so it's `'./'`
- Push to repo as usual

N.B. As soon as you push this new file to your regular project repo, it will try and run the action. It won't work. Don't worry.

### Step 2 - Encrypt secrets on GitHub

Like GitLab, GitHub allows you to store secrets in a dedicated place on the site - i.e. _not_ in a .env file in your repo, risking being exposed by hackers, or because you forgot to list it in a `.gitignore`, etc.

- [Documentation](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets)
- In your project repo, go to `Settings` > `Secrets` and set your variable names to match the ones specified in your `main.yml`

![secrets](/Journey/047/secrets.png)

The `AWS_ACCESS_KEY`s are exactly what you'd expect, but you can get caught out by the `AWS_S3_BUCKET`. Remember how S3 bucket names have to be unique on AWS? That's what needs to go here (not the arn, or the endpoint).

### Step 3 — Check you've got the right action for the job

So, the deployment ran with no errors, the S3 buckets had updated, but the change I made in `index.html` wasn't rendering when I went to the website's URI. This suggested that the CloudFront cache wasn't being invalidated, even though the Action stated that this was a feature. To be fair, I'd read through the code and couldn't find anything there, but assumed it would work as described... As soon as the main.yml is connected to your repo, you can browse the Marketplace (I couldn't find a way to search before pushing this up).

- On GitHub, browse to `main.yml`, click to edit, find the secret Marketplace search feature!

![step 1](/Journey/047/step1.png)

![step 2](/Journey/047/step2.png)

This is how I located the [Action](https://github.com/marketplace/actions/s3cmd-sync-action) that worked for me. It utilises S3cmd, and clearly invalidates the CloudFront cache with `S3CMD_CF_INVALIDATE: 'true'`, and saved me having to learn and code S3cmd commands into a blank yaml file...

- If this is the case, copy and paste into your code, remember to check the variables are correct!

## ☁️ Cloud Outcome

After interpreting _many_ errors in the deploy code, mainly due to formatting errors on my side, I got the right Action for the job, and my website is being updated in real time from the command line! 🥳

Thanks to anyone and everyone who shares their Actions in the Marketplace, you've made life so much easier 🏆

## Next Steps

I'm going to take a stab at adding the JavaScript counter - the final part of the front end - next weekend. Getting it done will give me the Christmas break to get to grips with the SAM CLI.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1338244382613516290?s=20)
