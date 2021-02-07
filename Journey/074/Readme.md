![sudo in a nutshell](https://imgs.xkcd.com/comics/sandwich.png)

# Linux Sudo (LUC 3)

## Introduction

Rather than interacting with our server at root level (which is dangerous, as you can destroy the system in a single command), we can log in as a regular user, and utilise the `sudo` command to perform root-level tasks with SuperUser privileges. Ubuntu leaves root logins disabled by default.

## Prerequisite

- A Linux Ubuntu machine (real, VM or cloud instance)

## Use Case

- Commands that require root privileges

## Cloud Research

- Every hacker will be looking to crack the password for the root user account. Since the root account password is locked by default, any attack becomes meaningless as there is no password to crack or guess in the first place.
- When using `sudo` your password is stored by default for 15 mins. After that time you'll need to enter it again
- Your password will not be shown on screen, not even as a row of stars
- To use `sudo` on the command line, _begin_ the command with `sudo`
- **To repeat the last command entered, except with sudo prepended to it, run `sudo !!`**
- [sudo man page for Ubuntu](http://manpages.ubuntu.com/manpages/hirsute/en/man8/sudo.8.html)
- Not every user can use sudo
- Some nice [tips and tricks](https://www.howtoforge.com/tutorial/sudo-beginners-guide/)
- [How password cracking is done](https://null-byte.wonderhowto.com/how-to/crack-shadow-hashes-after-getting-root-linux-system-0186386/)

## Try yourself - Tasks

Here, we're just following along with the tasks for today

### Task 1 — Use `ls -l` to check the permissions of `/etc/shadow` - notice that only root has any access. Can you use cat, less or nano to view it?

- `/etc/shadow` is where hashed passwords are kept, and as such it is a prime target for intruders who aim to grab it and use offline password crackers to discover the passwords
- I viewed it with `sudo cat shadow`

### Task 2 — Test running the `reboot` command, and then via sudo (i.e. `sudo reboot`)

- Only works with sudo
- If you run this with no options it also shuts the server down, and you have to wait some time to be able to power up again. Add `-f` to the command (to 'force' the server not to invoke shutdown)
- Once reconnected, use the `uptime` command to confirm that your server did actually fully restart

### Step 3 — Test fully “becoming root” by running `sudo -i`

This can be handy if you have a series of commands to do "as root". Note the change to your prompt. Run `exit` or `logout` to return to your own "support" login.

### Step 4 — Look up your command history of `sudo`

- Use `less` to view `/var/log.auth.log`, where any use of `sudo` is logged.
- Filter this with `grep "sudo" /var/log/auth.log`

### Step 5 — Change timezone (if you want)

The default is UTC, which is fine for me, but here's some instructions for exploration

- `timedatectl` returns the current setting
- `timedatectl list-timezones` shows a list of available timezones
- Select one, e.g. `sudo timedatectl set-timezone Australia/Sydney`
- Confirm with `timedatectl`

The major practical effects of this are (1) the timing of scheduled tasks, and (2) the timestamping of the logs files kept under `/var/log`. If you make a change, there will naturally be a "jump" in the dates and time recorded.

## ☁️ Cloud Outcome

As a Linux sysadmin you may be working on systems where you have little control - many of these will default to doing everything as `root`. You need to be able to safely work on such systems, where your only protection is to double check before pressing `Enter`.

## Next Steps

Next part of the course!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1356304850993246208?s=20)
