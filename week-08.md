# Week 8

## What I did
- Built a small Node.js app that counts visits in MongoDB, reading its settings from environment variables, and ran MongoDB in a container while developing.
- Wrote a Dockerfile (dependencies before code for layer caching, npm ci, running as USER node) and a .dockerignore, and ran the app and database together on a user-defined Docker network.
- Docker Compose: app, MongoDB and Mongo Express in one file, a random password in an ignored .env, a healthcheck with depends_on: service_healthy, and restart: unless-stopped.
- Debugged a broken container on purpose: status, logs, inspect (exit code and restart count), then the config.
- Kept the database across restarts with a named volume.
- Pushed the image to a private Azure Container Registry with the admin account off, and put the database password in Key Vault.
- Deployed the app to a VM with Terraform and cloud-init through the pipeline. The VM pulled the image and read the password with its own managed identity (AcrPull and Key Vault Secrets User only), then I tore it down.

## Ways to run a container on Azure
- **A VM with Docker:** when I need full control of the machine, e.g. Docker Compose with several containers and volumes on one host, special software or settings, or a lift-and-shift of something already running that way. I patch and look after the OS myself, and pay for the VM all the time it's running.
- **Azure Container Instances (ACI):** when I want to run one container, or a small group, quickly with no server to manage, e.g. a scheduled job, a build or test task, or a short-lived tool. Billed per second while it runs, but no autoscaling or built-in HTTPS ingress, so not ideal for a busy web app.
- **Azure Container Apps:** when I'm running web apps, APIs or background workers in production. It's serverless, with built-in HTTPS ingress, autoscaling (including down to zero), revisions for safe rollouts, and managed identity and Key Vault integration, without managing Kubernetes myself.

## What clicked
- A container shares the host's kernel; its writable layer disappears with it, so anything worth keeping goes in a volume, and a volume still isn't a backup.
- Layer order matters: copy package files and install before copying code, so code changes rebuild in seconds.
- Containers on a user-defined network find each other by service name, so the database needs no published port.
- "Running" isn't "healthy": a healthcheck tells Docker the service actually answers.
- Bind admin tools to 127.0.0.1, and run apps as a normal user, not root.
- One image runs anywhere because only the environment changes; the password reaches the VM from Key Vault at boot, and never goes into Git or Terraform state.
- $$ in a Terraform template leaves a literal ${...} for Docker Compose to fill in later.
- A start-up script should retry when it depends on things that only become ready later, like role assignments.

## What didn't
- I created the repo without the Node .gitignore template, so I recreated it with Node selected; without it, node_modules would have been committed.
- I put the named volume under the app service instead of mongo, so the visit count still reset on every restart. The volume has to be mounted on the container that writes the data, at /data/db.
