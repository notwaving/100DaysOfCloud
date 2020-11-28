<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# Docker Engine, Storage, and Registry

## Cloud Research

- Looked at how Docker works under the hood (Docker Engine).

- Where Docker stores its files and in what format:

![docker file system tree](/Journey/033/filesystem.png)

- How Docker utlizes the cache when building Dockerfiles, using layered architecture.

![layered architecture](/Journey/033/architecture.png)

- If we want data to persist once the container's life is over we create a volume. N.B. the more verbose `-mount` is now used over `-v`.

![docker volume](/Journey/033/volumes.png)

Storage in Docker : https://docs.docker.com/storage/

Storage Drivers: https://docs.docker.com/storage/storagedriver/

- Docker Registry is a central repository of all Docker images.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1332756327320444930?s=20)
