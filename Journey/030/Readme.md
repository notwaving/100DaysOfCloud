<!-- This template removes the micro tutorial for a quicker post and removes images for a full template check out the 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

# More Docker Run Commands!

## Cloud Research

- Tags
  e.g. `docker run redis:4.0`
  The `:4.0` is a tag. In this case, Docker pulls the 4.0 version of Redis, and runs that. If no tag is specified, Docker runs the latest version by default. You can find a list of all versions of a piece of software on Docker Hub.

- Interative mode & terminal `-it`
  e.g. entering `docker run -it notwaving/simple-prompt` would place the file into interactive mode on the container and Docker will be able to read STDIN (standard input)

- PORT mapping - `p`
  e.g `docker run -p 80:5000 notwaving/simple-app`maps port 80 on local host to port 5000 inside the Docker container, allowing the user to access your app. You can run multiple instances of your app and map them to different ports on the Docker host.

- Inspect container
  `docker inspect <name>`
  This returns a JSON object

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1331632133765558281?s=20)
