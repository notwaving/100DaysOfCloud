# Linux Upskill Challenge Day 1 & Docker

## Introduction

For the forseeable future, I'm going to be concentrating on upskilling my Linux & Docker/Kubernetes - doing them simultanously in the hope that the slow burn will help them stick in my mind. I need Linux for all the tinkering about on the command line that containers and orchestration need, as well as networking knowledge gaps. _I'm going to attempt to work every day on the Linux Upskill Challenge_. There are 21 days in total. Today we're going to create a server on AWS, and access it with SSH. For Docker, it's a thorough course and today is revision of basic commands, which I won't be documenting.

## Prerequisite

- AWS account on free tier
- Familiarity with EC2, virtual private servers

## Resources

- [Linux Upskill Challenge](https://github.com/notwaving/linuxupskillchallenge)
- [Waddle](https://www.youtube.com/playlist?list=PLleOCN2eBn8K1SaW82otMnbxKkdexY8zr)
- [Pipelines: Intro to DevOps/SRE (Apprentice)](https://www.youtube.com/playlist?list=PLleOCN2eBn8KYJlW2kZ90ZNiUaYOy2fI4)
- [Docker and Kubernetes: The Complete Guide](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/)

## Use Case

- Practice makes perfect!

## Cloud Research

Technically, what we'll be doing is creating and renting a VPS ("Virtual Private Server"). In a datacentre somewhere a single physical server running Linux will be split into a dozen or more Virtual servers using the KVM (Kernel-based Virtual Machine) feature that's been part of Linux since early 2007.

## Try yourself

We'll install the latest LTS version of Ubuntu Server (20.04), via EC2.

### Step 1 — Launch EC2 instance

- Once logged into AWS, navigate to EC2. Click `Launch Instance`.
  (Here's a sign of growth: got into a bit of a rabbit hole when I saw the options of an x86 or Arm server, and wanted to know the pros and cons of each. Chose x86 in the end, as it's default and the only choice with most of the machine images on offer. Never noticed this before!)
- Find the Ubuntu Server, correct version, check that it's eligible for free tier. Double check you're in the region you want before hitting `select`.
  ![the ami I'm using](/Journey/072/ubuntu-ami.png)
- Select `t2.micro` as instance type, as it's free tier eligible. Click through to `configure security group`. Make sure you add the type `SSH` and set the `Port Range` to 22, and the `Source` to ::/0
- Here, you can add an already existing security group or add a new one. The default allows all traffic from anywhere, so choose that one.
  **N.B. This would be unwise for a production server, but it's what we want for this course.**
- Select `launch`.

### Step 2 — Create/link a key pair and connect

This will allow us to connect to our server from our computer. If you haven't got one already, create a new one here, and store the resulting `.pem` file somewhere safe. Once this is confirmed, you can view your newly launched instance! 🎉

**Gotcha:** Make sure there are no spaces in your key!

- From the EC2 Instances page, select your instance, then from the `Actions` drop down menu, click `Connect`
- Take a note of the user name (defaults to the name of the AMI)
- Click `connect`

### Step 3 — Remote access via SSH

- [This is a bit advanced](https://cloud-gc.readthedocs.io/en/latest/chapter06_appendix/ssh-config.html), but saves you having to type
  `ssh -i "nameOfKey.pem" ubuntu@IPv4-address`
  every time you want to ssh into the server.
- If you ever stop and restart the server, this IPv4 address will change
- I've configured the .ssh/config file to recognise `ec2` as an alias for the above. All I have to do is run the command `ssh ec2` to get into the server!

### Step 4 - Check to see if you are the sysadmin

- Install some updates:
  `sudo apt update`
  Normally you'd expect this would prompt you to confirm your password, but because you're using public key authentication the system hasn't promoted you to set up a password - and AWS have configured sudo to not request one for "ubuntu"
  `sudo apt upgrade`

- To logout, type `logout` or `exit`.

### Try out some commands

- `ls` to list files
- `uptime` In order of appearance, the command displays the current time as the 1st entry, up means that the system is running and it is displayed next to the total time for which the system has been running, the user count (number of logged on users), and lastly, the system load averages.
- `free` When used without any option, the free command will display information about the memory and swap in kibibyte. 1 kibibyte (KiB) is 1024 bytes.
- `df -h` - report file system disk space usage. The `-h` option is "human readable" - sizes are in powers of 1024.
- `uname -a` prints system information, e.g. kernel name, version, processor, OS, etc.

## ☁️ Cloud Outcome

- This server is now running, and completely exposed to the whole of the Internet
- You alone are responsible for managing it
- You have just installed the latest updates, so it should be secure for now

## Next Steps

Tomorrow is basic navigation

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[Twitter](https://twitter.com/_notwaving/status/1355517712169119744?s=20)
