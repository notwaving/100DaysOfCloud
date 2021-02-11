![docker](https://i.pinimg.com/originals/8b/e3/3e/8be33e8e6a287496ac3c9b7202d0d8e2.gif)

# Manipulating Containers with the Docker Client

## Introduction

Now that the Tech Returners course is over 😢, I've got more time to catch up on a deep dive into Docker 🎉. It's

## Use Case

- Frequently used commands/concepts in Docker

## Cloud Research

- Execute a command inside a container:
  `docker exec -it {container-id} {command}`
- Full terminal access inside the context of the container:
  `docker exec -it {container-id} sh`
  You'll see a `#` as a prompt and you can type out the commands that you could expect to run in the environment
  This last command is extremely powerful for debugging
- Hit `ctrl` + `c` to exit (`ctrl` + `d` or `exit` work too)!

#### What Just Happened?

`exec -it`

- `-i` "when we execute this new command inside the container we want to attach our terminal to the STDIN channel of that new running process". Anything you type gets directed to STDIN.
- `-t` makes sure that the text you're entering shows up in a nicely formatted manner (put simply).

`sh` is the name of a programme being executed inside of the container. `sh` is a command processor (like bash, powershell, zsh etc). It allows us to type commands and have them be executed inside the container

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1359985450262409217?s=20)
