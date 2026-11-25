# Week 8

## Ways to run a container on Azure
- **A VM with Docker:** when I need full control of the machine, e.g. Docker Compose with several containers and volumes on one host, special software or settings, or a lift-and-shift of something already running that way. I patch and look after the OS myself, and pay for the VM all the time it's running.
- **Azure Container Instances (ACI):** when I want to run one container, or a small group, quickly with no server to manage, e.g. a scheduled job, a build or test task, or a short-lived tool. Billed per second while it runs, but no autoscaling or built-in HTTPS ingress, so not ideal for a busy web app.
- **Azure Container Apps:** when I'm running web apps, APIs or background workers in production. It's serverless, with built-in HTTPS ingress, autoscaling (including down to zero), revisions for safe rollouts, and managed identity and Key Vault integration, without managing Kubernetes myself.
