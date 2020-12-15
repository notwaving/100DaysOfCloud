![like living in the matrix](/Journey/049/matrix.png)

# NGINX Proxy Pt 3

## Introduction

Today we're creating our config files in NGINX

## Prerequisite

Familiarity with Linux commands

## Use Case

This is all part of setting up a proxy to serve a Django app efficiently

## Cloud Research

1. Add a new project in GitLab for proxy application
2. Setup AWS IAM account with new ECR repo
3. **Create a Dockerfile and NGINX configurations**
4. Create a CI cluster account for GitLab to authenticate with AWS in order to push our image
5. Set up pipeline jobs in GitLab to run workflow

## Try yourself - Create NGINX config files

### Step 1 — Set a new branch in `recipe-app-api-proxy` repo

- In terminal, navigate to the repo
- Enter `git checkout -b feature/nginx-proxy` to create a new feature branch
- Create a new file in the root of the project: `default.conf.tpl`. This will be our NGINX configuration template file. The reason we end it in `.tpl` is because this is a template which will get passed through something called `env substitute` to populate values based on environment variables
- Define a server block: this is a default block that all NGINX configuratations need to have:

```
server {
  listen ${LISTEN_PORT};

  location /static {
    alias /vol/static;
  }

  location / {
    uwsgi_pass  ${APP_HOST}:${APP_PORT};
  }
}
```

- Grab the uwsgi params link from the course
- Create a new file called `uwsgi_params` and paste them there
- Go back to the template file and add a line to include this new file in the locaton code block, as well as one about maximum body size of requests that can be sent:
  - `include /etc/nginx/uwsgi_params;`
  - `client_max_body_size 10M`
    This is to help us when uploading images.
- Create the entrypoint - new file in the root of the project, `entrypoint.sh`. This is a shell script to run the app in Docker.

### Step 2 — Create NGINX Dockerfile

Now that we've configured our NGINX server, let's create a Dockerfile to put all this together, allowing us to launch an NGINX instance via Docker.

- Create new file in root of project, called `Dockerfile`. Inside this:
  - `FROM nginxinc/nginx-unprivileged:1-alpine`.
    The version of NGINX with least privilege, followed by the most lightweight version of the NGINX image. Alpine is a Linux OS, specifically designed for Docker containers.
  - Set maintainer as email address
  - `COPY` the template `default.conf.tpl` and `uwsgi_params`
  - Define env variables:
  ```
  ENV LISTEN_PORT=8000
  ENV APP_HOST=app
  ENV APP_PORT=9000
  ```
  - Make sure root user is enabled with `USER root` for certain permissions. We'll switch back to an NGINX least-privilege user once we're done
  ```
  RUN mkdir -p vol/static
  RUN chmod 755 /vol/static #This is permissions
  RUN touch /etc/nginx/conf.d/default.conf
  RUN chown nginx:nginx /etc/nginx/conf.d/default.conf
  ```
  - `COPY ./entrypoint.sh /entrypoint.sh`
  - `RUN chmod +x /entrypoint.sh` This ensures the entrypoint file is executable
  - `USER nginx` to change user back to default
  - `CMD ["/entrypoint.sh"]` "The default command is our entrypoint.sh"

### Step 3 — Build Docker image

- Make sure Docker is running on your machine. I'm using Docker desktop for Mac

- From the project folder, in terminal: `docker build -t proxy .` (tag it with 'proxy'), and run!

## ☁️ Cloud Outcome

Learned a painstakingly tricksy way to set up config files. In practice I'd want to be using templates... But hey, it worked!

## Next Steps

Set up GitLab CI/CD pipeline build and push jobs

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1338915564488679429?s=20)
