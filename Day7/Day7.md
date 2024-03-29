# Application Design and Build (20%)
- Define, build and modify container images
- Choose and use the right workload resource (Deployment, DaemonSet, CronJob, etc.)
- Understand multi-container Pod design patterns (e.g. sidecar, init and others)
- Utilize persistent and ephemeral volumes

# Application Deployment (20%)
- Use Kubernetes primitives to implement common deployment strategies (e.g. blue/green or canary)
- Understand Deployments and how to perform rolling updates
- Use the Helm package manager to deploy existing packages
Kustomize

# Application Observability and Maintenance (15%)
- Understand API deprecations
- Implement probes and health checks
- Use built-in CLI tools to monitor Kubernetes applications
- Utilize container logs
- Debugging in Kubernetes

# Application Environment, Configuration and Security (25%)
- Discover and use resources that extend Kubernetes (CRD, Operators)
- Understand authentication, authorization and admission control
- Understand Requests, limits, quotas
- Understand ConfigMaps
- Define resource requirements
- Create & consume Secrets
- Understand ServiceAccounts
- Understand Application Security (SecurityContexts, Capabilities, etc.)

# Services and Networking (20%)
- Demonstrate basic understanding of NetworkPolicies
- Provide and troubleshoot access to applications via services
- Use Ingress rules to expose applications

## Persistence Volume

kubectl create pv log-volume --capacity=storage=1Gi --spec.hostPath.path=/opt/volume/nginx --spec.storageClassName=manual --spec.accessModes=ReadWriteMany

## Policy
k get netpol

## General

k get pod --show-labels

## Cron job alternative
Amazon EC2 with cron:

If you're using an EC2 instance:

You can simply set up a cron job in the EC2 instance to run your script daily at 10 am.
Steps:

SSH into your EC2 instance.
Open the crontab using crontab -e.
Add your job: 0 10 * * * /path/to/your/script.