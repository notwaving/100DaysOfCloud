<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# DevOps - The Introduction Course: 3

## Puppet

- Ruby-based
- Created in 2005
- Puppet resourses can perform almost any IT automation task
- Master-client model - primary deployment model - pull-based (like Chef)
- Self-contained standalone model - dev, testing, proof of context
- OS
  - Puppet Master - Linux only
  - Puppet Agent - Linux and Windows
- Declarative programming
- Consistent delivery
- Scaleable
- Simple

## Kubernetes

### Architecture

- A `node` is a worker machine (physical or virtual), running k8s. This is where containers are launched from. Also known as 'minions' in the past
- A `cluster` is a set of multiple nodes, built for redundancy.
- A `master` is another node, configured as a master. It is responsible for container orchestration on the worker nodes.

#### Components on Master Nodes

- `API server` - front end for K8s
- `etcd` service - key-value store
- `controller` - brains behind orchestration. Responsible for noticing and responding when nodes, containers or endpoints go down. They make decisions to bring up new containers in such cases.
- `scheduler` - distributes work/containers across multiple nodes. It looks for newly created containers and assigns them to nodes.

#### Components on Worker Nodes

- `kubelet` service - agent that runs on each node in the cluster. Responsible for making sure that the containers are running on the nodes as expected.
- `container runtime` - underlying software used to run containers (e.g. Docker)

#### kubectl (kube command line tool / kube control)

- Used to deploy and manage applications on a k8s cluster
- Also used to get cluster information, status of other nodes, and other management
- `kubectl run` command is used to deploy an application on the cluster
- Other commands include `kubectl cluster-info` and `kubectl get nodes` (lists nodes in cluster)

## OpenShift

OpenShift is Red Hat's open source container application platform for developing and hosting enterprise-grade hosting and applications.

PaaS. OpenShift takes care of managing the underlying infrastructure components

Four different flavours of OpenShift, but we're going to focus on OpenShift Origin

- based on top of Docker containers and the K8s cluster manager, with added developer and operational centric tools that enable rapid app development, deployment and lifecycle management.

### Architecture

#### Components

- OpenShift Container Registry (OCR)
- OpenShift Web Console allows devs to browse and manage their apps
- Source Code Management systems for built-in integrations
- CI/CD pipelines (which is then pushed into the OCR)

### Cloud Research

- [The difference between Ansible, Chef, and Puppet](https://www.gangboard.com/blog/ansible-vs-puppet-vs-chef)
- Push-based: Ansible, Saltstack
- Pull-Based: Puppet, Chef

- OpenShift is a hybrid cloud platform

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1334931316727541767?s=20)
