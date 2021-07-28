# AWS Proton Demo

This repository provides resources that can be used to demo functionality of the [AWS Proton service](https://aws.amazon.com/proton/).

This includes:
- Environment and service templates for various scenarios (see below)
- Sample applications compatible with the above

## Templates

Quick links to the environment and service templates:

### Environment Templates

| Name | Description | Link |
|------|-------------|------|
| ECS Public VPC | VPC with public access and compute, and an ECS Cluster | [Docs](/templates/environment/ecs-public-vpc/README.md) |
| ECS Public/Private VPC | VPC with public access, private compute, and an ECS Cluster | [Docs](/templates/environment/ecs-public-private-vpc/README.md) |
| App Runner Environment | An environment for App Runner services | [Docs](/templates/environment/apprunner-env/README.md) |

### Service Templates

| Name | Description | Link |
|------|-------------|------|
| Loadbalanced ECS Fargate Service | ECS Fargate Service with an Application Load Balancer | [Docs](/templates/service/ecs-fargate-loadbalanced/README.md) |
| App Runner Service from Image (Dockerfile) | App Runner service deployed from a container image built using a Dockerfile | [Docs](/templates/service/apprunner-image/README.md) |
| App Runner Service from Image (Buildpacks) | App Runner service deployed from a container image built using Cloud Native Buildpacks | [Docs](/templates/service/apprunner-image-buildpack/README.md) |

## Sample Applications

| Name | Description | Link |
|------|-------------|------|
| Java | Spring Boot web application | Docs |