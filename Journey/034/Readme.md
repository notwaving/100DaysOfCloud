<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# Container Orchestration - Docker Swarm & Kubernetes

## Docker Swarm

Container orchestration solutions can assist with scaling horizontally and vertically, high availability, advanced networking between containers across different hosts, load balancing, etc

- Solutions include Docker Swarm, Kubernetes (Google), and MESOS (Apache)
- With Docker Swarm, you can combine multiple Docker machines together into a single cluster. DS will distribute your app/instances into separate hosts for high availability and load balancing across different systems and hardware.

![docker swarm](/Journey/034/docker-swarm.png)

## Introduction to Kubernetes

With Kubernetes, using the Kubernetes CLI (kube control) you can run a thousand instances of the same application in a single command.

- Can be configured to do this automatically, so that instances and infrastructure can scale up and down accordingly... among many other things
- Uses Docker host to host applications in the form of Docker containers.
- A `node` is a worker machine (physical or virtual), on which k8s software is installed, where containers will be launched by k8s.
- To recover from failure we have more than one node. A cluster is a set of nodes grouped together for this purpose.
- The `master` is responsible for the orchestration of containers on the worker nodes.

_Kubernetes components_

![kubernetes components](Journey/034/components.png)

- `kubectl` - the kube control tool - is the k8s CLI. The `kubectl run` run command is used to deploy an app on the cluster; `kubectl cluster-info` is used to view info on the cluster; and `kubectl get nodes` lists the nodes in the cluster.

### Social Proof

[Twitter](https://twitter.com/_notwaving/status/1333125786245754881?s=20)
