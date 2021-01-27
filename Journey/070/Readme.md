# Kubernetes Documentation - What is Kubernetes?

## Introduction

I had such positive results from working through official Terraform documentation and demos before doing a beginner's course, that I'm going to do the same again with Kubernetes (k8s). Starting with docs ensures I know and understand offical terms at the most basic level before taking on a course with an external provider who assumes a certain level of practical experience.

## Prerequisite

- Knowing what containers are? No prerequisites otherwise

## Use Case

- As more businesses adopt containers, it is important to use an orchestration platform to scale and manage them

![use case](/Journey/070/k8s-use-case.png)

- CI/CD tool

## Cloud Research

- [Official documentation](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)
- "Kubernetes" is a Greek word, meaning "helmsman" or "pilot". You're piloting a container ship...
- Open source since 2014, created by Google

![from physical servers to container deployment](Journey/070/time-travel.png)

#### Advantages of Containers over Virtual Machines

- Lighter - containers share a single OS, rather than each running on its own one
- Dev and Ops separation of concerns: create app container images at build/release time rather than deployment time, thereby decoupling apps from infrastructure
- Observability not only surfaces OS-level info and metrics, but also app health and other signals
- Environmental consistency across development, testing, and production: Runs the same on a laptop as it does in the cloud.
- Cloud and OS distribution portability: Runs on Ubuntu, RHEL, CoreOS, on-prem, major public clouds and anywhere else
- App-centric management: raises the level of abstraction from running an OS on virtual hardware to running an application on an OS using logical resources
- Loosely coupled, distributed, elastic, liberated micro-services: applications are broken into smaller, independent pieces and can be deployed and managed dynamically – not a monolithic stack running on one big single-purpose machine.
- Resource isolation: predictable application performance.
- Resource utilization: high efficiency and density.

#### Why Kubernetes?

Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system? Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example, Kubernetes can easily manage a canary deployment for your system.
![what k8s can do](Journey/070/kuber-yes.png)

#### What Kubernetes Is Not

Kubernetes is not a traditional, all-inclusive PaaS (Platform as a Service) system. Since Kubernetes operates at the container level rather than at the hardware level, it provides some generally applicable features common to PaaS offerings, such as deployment, scaling, load balancing, and lets users integrate their logging, monitoring, and alerting solutions. However, Kubernetes is not monolithic, and these default solutions are optional and pluggable. Kubernetes provides the building blocks for building developer platforms, but preserves user choice and flexibility where it is important.

## ☁️ Cloud Outcome

Started reading the official documenentation to understand what Kubernetes is, where it comes from, and where it sits in the cloud/DevOps ecosystem.

## Next Steps

Kubernetes components

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1354130801265405957?s=20)
