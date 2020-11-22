![A badge with "I heart S3" on it](/Journey/028/20201122_140627.jpg)

# Amazon S3 Static Website - Cloud Resume Challenge 🌐

## Introduction

_Phase 1 - Create HTML Resume_ is now complete and sitting in a private GitHub repo. It's time for _Phase 2 - Deploy HTML Resume To AWS_.

## Prerequisites

- IAM user account with admin permissions
- Some familiarity with the AWS Management Console will stop things being too overwhelming, but this setup is relatively easy to understand and configure

## Use Case

- S3 buckets can be used to host host static websites

## Cloud Research

- I found this pretty straightforward using the AWS Management Console - you can easily drag and drop files and folders into your S3 bucket
- The Console UI changes very frequently, so remember to take your time when following guides

## Try it yourself

Here's how to add your website assets to S3 and make it accessible to the internet.

### Step 1 — Upload website files and folders to S3

![Screenshot](/Journey/028/img-1.png)

![Screenshot](/Journey/028/img-2.png)

### Step 2 — Set a bucket policy

![Screenshot](/Journey/028/img-3.png)

Unchecking `Block public access` allows your S3 bucket to become public _if_ it has the right policy.

![Screenshot](/Journey/028/img-4.png)
![Screenshot](/Journey/028/img-5.png)
![Screenshot](/Journey/028/img-6.png)

Click `edit bucket` then `policy generator`

![Screenshot](/Journey/028/img-7.png)

Don't forget to add `/*` to the ARN

Click `Add Statement`, then `Generate Policy`

Copy and paste the JSON object that's just been generated into the Bucket policy editor (where you clicked 'policy generator' before).

Hit save. Your bucket is now public!

![Screenshot](/Journey/028/img-8.png)

### Step 3 — Deploy as an S3 static website

Click on the your bucket > `Properties` > `Static website hosting` > `Edit`

![Screenshot](/Journey/028/img-9.png)

N.B. the Index document must be named `index.html`

Click `save`

If you click the Bucket website endpoint, you should see it all working!

**Remember to enable bucket versioning. It protects against unintended deletes, and you can easily roll back to a previous version of a file.**

## ☁️ Cloud Outcome

The website is now live and hosted in an S3 bucket!

## Next Steps

Next time, we will purchase a domain name in Route 53, create an SSL certificate (as we'll be serving the site over HTTPS), and a CloudFront distribution to enable HTTPS.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1330519514740645895?s=20)
