# Hoist: Terraform Tutorial Pt 6

## Introduction

It's the final part of the course. Today we've got a [brief](https://courses.mastermnd.io/fea01da5d6ff4fe49d952f04f151b61f) to follow!

## Prerequisite

- Understand what's been taught in previous classes
- AWS account

## Use Case - Hoist Lab!

#### Let's set the scenario...

It's April 2020. You've been on lockdown for a little more than 2 weeks. Everyone thought this was temporary, but it looks like we'll be locked down much longer. You work for a medium sized ed-tech software company called Hoist, where you're "Raising the bar on education". Due to the quarantines, the company is pivoting a bit and throwing some resources at a few new internal projects. You're responsible for getting them up an running.

#### Requirements

This new team is very proficient at developing software, but they kind of suck at the rest of it. Lets use Terraform to help them out and configure the following resources.

#### AWS Users 🤼

First up we'll need to ensure there are IAM users created for each developer. No need to worry about Roles or Policies right now as we're not sure which directions these projects will take yet. Just create a user with the format: firstname.lastname

#### Github 👩🏾‍💻

Obviously we need to include some version control. No one will have access to be able to create repos in the org, so we'll need a way to do it programmatically. Set up the following Github repos:

- hydra

- draco

- blaze

These repos are top secret so keep them private. More repos may be coming down the pipeline.

After the repos are ready, we'll grab the engineers github handles and they will need to be added to our new GitHub Organization, hoist-education.

#### Development Servers 📥

Each engineer that joins this team needs its own development server for you know.....development. Ensure a server is configured for every dev that joins the team. Although we love ubuntu, we'll need to provision RedHat servers as certain software they need only works there. Ensure these servers have a public ip address and make sure to use our development key for ssh. The keyname is hoist_dev

#### Old Data 💾

There has been a mysql database created in the new AWS account in RDS. It houses data that will be used in the development of these new application. It was provisioned manually so it's management needs to be brought under the umbrella of Terraform....do ya thing boss.

## Cloud Research

This is entirely practical, so here goes...

## Try yourself

In a new project folder, create a `main.tf` file

### Step 1 — Set up AWS as the provider in `main.tf`

- Check out the docs for the code.
- run `terraform init` to get dependencies, configuration etc

### Step 2 — Create IAM users

- Read the docs carefully. Only take the code you need!
- For this, we're adding the `name` and `force_destroy` parameters. Force destroy means that the IAM user will be destroyed if you run `terraform destroy`. The brief for this lab states we don't need to create a IAM policies.
- The `name` needs to be in the format `firstname.lastname`, so we can create a variable to handle that. Let's put into practice what we learned yesterday - a for loop:

![variable](/Journey/065/iam2.png)
![main](/Journey/065/iam1.png)

- To make `count` more dynamic (i.e. allowing any number of IAM users to be created), use the length() function: `count = length(var.engineer_name)`

- Change the default value back to "firstname.lastname" (good leaving clues for debugging)

- Move the names from that var definition, into a new file, `terraform.tfvars` and declare them there:

`engineer_name = ["aaron.brooks", "angela.yu", "tim.buchalka", "gene.kim"]`

- Run `terraform plan` to check that the variable in `terraform.tfvars` overrides the default in `variables.tf`

- Run `terraform apply`

![iam success](/Journey/065/iam3.png)

### Step 2 — GitHub

As none of our IAM users will have access to creating repos. we'll have to do it programatically ourselves. Here's the [documentation](https://registry.terraform.io/providers/integrations/github/latest/docs). We're using a second provider in our config!

![github-provider](/Journey/065/github-provider.png)

We need to set up an _organisation_ and a _token_ to access it, so do that on GitHub, note those vars somewhere safe, and then we'll commit (i.e. export) to `TF_VARS_`.

![org](/Journey/065/github-org.png)

Here's GitHub documentation for setting up [tokens](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

`export TF_VAR_GITHUB_ORGANIZATION=hoist-education-notwaving`

`export TF_VAR_GITHUB_TOKEN=<token>`

- You've updated the provider, so remember to run `terraform init`

- We need to create three GitHub repositories, so navigate to the correct part of the [documentation](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/repository) and have a look. Note which parameters are required and which are optional.

**Tip:** to speed this up, highlight 'optional' or 'required', and hit command + f (i.e. 'find') to show occurances there are in the documentation!

- As we have to set up a number of repositories at once, we could do so in a similar way to how we set up the IAM user names (i.e. with a loop).

![loop](/Journey/065/github-loop.png)

- Even though you set TF_VAR_s earlier, you still need to declare them in config. Go to `variables.tf`

![secrits](/Journey/065/secrits.png)

- You know the drill. Once everything's formatted and validated and planned, run `apply`!

![terminal](/Journey/065/repo-create-1.png)

![github](/Journey/065/repo-create-2.png)

- To add GitHub accounts (I'm added automatically, so can't do this):
  Under `github_membership` in the docs, this will send an invite to the named GitHub account holder

![add github user](/Journey/065/add-github-user.png)

### Step 3 — Development servers

- From now on we're setting things up as modules. Set up `developmentServers` folder and file

## ☁️ Cloud Outcome

- Feeling fully confident in my Terraform skills and IaC, and Hashicorp's documentation is the best I've ever seen (so far?)

- An appreciation that Terraform config is self-documenting

- Didn't realise GitHub was a provider - thought it was just cloud services!

- It's much easier to understand

- Great fun using for loops for creating resources

![Hi five!](https://media.giphy.com/media/CW27AW0nlp5u0/giphy.gif)

## Next Steps

Time runs out on the [class](https://youtu.be/liYjjSSd56s), and things get gnarly/confusing with implementation, so I stopped after getting the dev servers running. But I used `length(users)` in a for loop to create a development server! Could try doing the final step, but seemed to involve a lot of set up prior to the exercise...

Oh yeah, and put everything into modules!

Outside of Terraform, next I'm going to work on anything Cloud that coincides with my course. Otherwise, I got on so well with Aaron's teaching style I'm going to get onto his Linux class next - I need a good practice

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1350897456809082881?s=20)
