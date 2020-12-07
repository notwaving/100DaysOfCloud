![https in action](/Journey/040/https.png)

# 🔒 Cloud Resume Challenge - Setting up HTTPS with CloudFront 🔒

## Introduction

After hosting the static website CV in an S3 bucket and purchasing a domain name on Route 53, securing the site with HTTPS (as per instructions) seems like the next logical step.

## Prerequisite

- A decent video guide. I've personally found AWS's documentation to be verbose and really hard to follow (no point posting visuals when the UI changes every few weeks?), and blogs haven't been detailed enough. Start about halfway through [this](https://youtu.be/lB4DTqMEumY), it's perfect for this use case.

- If deciding to purchase a domain name on `Route 53`, I'd suggest you do that first and _then_ name the `S3 bucket` with that same domain name. The correct bucket name will automatically populate all the drop down forms that are coming your way...

## Use Case

- SSL (the 'S' in HTTPS), prevents man-in-the-middle attacks, and builds trust with users
- Places the website higher on Google searches

## Cloud Research

- In yesterday's log I shared a blog that detailed buying a domain name, requesting a SSL certificate via `Certificate Manager`, and finished waiting for my verification email. That email never came 😨. Turns out that AWS sent it to email addresses that don't exist for me (rather than the email associated with my AWS)

![🤯](/Journey/040/eh.png)

Kind of unhelpful when your website doesn't have any domain-related emails associated with it.

- This was just the beginning of shenanigans 🤬, so today I determined to find a comprehensive video guide to

  - Fix everything
  - Detail it here for future-reference

- There are three main 'gotchas', you've been warned!

## Try yourself

![architecture](/Journey/040/day40-architecture.png)

Here's how to add HTTPS to your website, using CloudFront and AWS Certificate Manager

### Step 1 — AWS Certificate Manager

- Navigate to `AWS Certificate Manager`

![first gotcha](/Journey/040/us-east-1.png)

- **GOTCHA #1: Make sure your region is set to N. Virginia (us-east-1), or this will not work**
- `Provision certificates`
- `Request a public certificate`
- `Add domain names`
- Select `DNS validation`
- Click through until you're here 🔽🔽🔽

![aws certification manager](/Journey/040/pending.png)

- **GOTCHA #2: nothing will happen until you find the _secret menus_**

![gotcha 2.1](/Journey/040/menu1.png)

![gotcha 2.2](/Journey/040/menu2.png)

_Toggle those tiny black arrows!_

- Hit the blue button, `create record in Route 53`. This automatically creates a CNAME record for you.
- Success! ✅
- N.B It will take a few minutes for the status to change from `pending validation` to `issued` - about the same time as takes to make a cup of tea ☕️

### Step 2 — Create a Distribution in CloudFront

- In the CloudFront console, click `Create distribution`
- Select `get started` in the `web` option

You will see lots of options, but you only need to set up four things:

#### 1️⃣ Origin Settings

![gotcha 3](/Journey/040/do-not-do-this.png)

- **GOTCHA #3: In `Origin Domain Name` _do not_ select your S3 bucket in the dropdown menu**
- Instead, open `S3` in another tab, click on `<bucket name>` => `properties`
- In the `static website hosting` section locate the endpoint. Select this url _without_ the http part:

![endpoint](/Journey/040/endpoint.png)

- Copy and paste _that_ into `Origin Domain Name`

- The `Origin ID` field will populate itself

#### 2️⃣ Default Cache Behavior Settings

- In `Viewer Protocol Policy`, select the radio button
  🔘 `Redirect HTTP to HTTPS`

#### 3️⃣ Distribution Settings

- `Alternative Domain Names (CNAMEs)` - here is where you put your domain name(s).
  N.B. Make sure there's no `http://` in front of your domain name, and no `/` at the end

#### 4️⃣ SSL Certificate (also in Distribution Settings)

- Click on the
  🔘 `Custom SSL Certificate` radio button
- Clicking into the associated field will cause your SSL certificate to appear. Select that.

Scroll to the bottom of the page and click `Create distribution`.

It will take approximately 40 minutes for the CloudFront distribution to be `deployed`, so now's the time to read that chapter of that book 📖, do some yoga 🧘🏽‍♂️, have a bath 🛀🏿 ... Just do something nice for yourself, you deserve it.

### Step 3 — Connecting CloudFront to Route 53

![cloudfront domain name](/Journey/040/cf-domain-test.png)

- Still in the CloudFront Distributions part of the console, copy the associated domain name, and paste into a new tab

- You should see your static website, now with HTTPS. If not, then something's gone wrong with CloudFront...

- At the moment this won't work with your domain name, as CloudFront isn't connected to Route 53.

- Open Route 53 in a new tab, and navigate into your domain name

- `Create Record Set`

- The only things we're going to touch are in `value/route traffic to`
  - Endpoint - `Alias to CloudFront Distribution`
  - Region - `us-east-1` (this might be filled in for you)
  - Distribution - the domain name associated with your CloudFront distribution (the one that worked with HTTPS at the top of this section)
- Click `create`, this will take a couple of minutes, plenty of time to make another cup of tea ☕️
- Check that HTTPS is being applied to your domain name 🔒
  _N.B. There's a chance it's being cached, so before you have a nervous breakdown, be sure to check it on another device, incognito browser window, etc. first_
- Celebrate! 🥳

## ☁️ Cloud Outcome

I've learned how to apply HTTPS protocols to static websites using CloudFront. It was not in any way straightforward, and you can be caught out by some of the pre-populated dropdown menus, but I'm glad to have the resources and time to document everything.

## Next Steps

Next on the list is setting up that tricksy JavaScript counter that bridges the front and backend, but I think the next sensible thing to do is to set up GitHub Actions to automatically update the S3 buckets every time I deploy new code...

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1335660080226840577?s=20)
