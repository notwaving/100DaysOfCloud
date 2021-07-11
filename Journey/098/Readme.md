# VPC practical - manual setup - for AWS-SAA

## Cloud Research

This is HARD.

* Manual creation and config of VPC and subnets (for better appreciation of automation tools later on)
* Internet Gateway
* Bastion Host / Jumpbox
* Route Tables
* Routes

Converted three subnets to be public: 
- Created an Internet Gateway
- Associated that with the VPC
- Created a route table
- Associated that with those subnets
- Added two routes
- Pointed those routes at the Internet Gateway
- Configured the three subnets to allocate a public IPv4 address to any resources launched into those subnets

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1414269247317069831?s=20)
