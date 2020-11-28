<!-- This template removes the micro tutorial for a quicker post and removes images for a full template check out the 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# YAML & Docker Compose

## YAML

### Introduction

- I was about to start the class on Docker Compose, but was advised to get familiar with YAML first, so here I am.

### Use Case

- Docker configuration

### Cloud Research

- A YAML file is used to represent data, in this case, configuration data.

![XML, JSON, YAML syntax](/Journey/032/syntax.png)

- As in Python, indentation, whitespace - matters.

![XML, JSON, YAML syntax](/Journey/032/indentation.png)

![differences between YAML data structures](/Journey/032/dictionary-array.png)

## Docker Compose

### Use Case

If you wanted to set up a complex application running multiple services, a best practice is to use Docker `compose` (rather than `run`). This is easier to implement, run and maintain, as all changes are always stored in the docker-compose config file. However, this is only applicable to running containers on a single Docker host.

![run vs compose](/Journey/032/compose-use-case.png)

### Cloud Research

- Can create a configuration file in YAML format called `docker-compose.yml`, putting together the different services and the options specific to running them.

- `docker-compose up` brings up the entire application stack.

- `docker run --link` has been [deprecated](https://docs.docker.com/network/links/), but worth having a look at, even as it gets replaced by advanced features such as Docker Swarm and networking

- Use the run commands to make your compose file, then use `docker-compose up` to bring up the stack

![from run to compose](/Journey/032/run-to-compose.png)

- N.B. There are three versions of docker-compose.

![versions 1-3 of docker compose](/Journey/032/docker-compose-versions.png)

- Here's an example of how the Docker compose file relates to the application architecture:

![docker compose file and app architecure](/Journey/032/compose-and-architecture.png)

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1332460198791946243?s=20)
