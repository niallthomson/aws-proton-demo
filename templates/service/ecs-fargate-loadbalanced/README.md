# AWS Proton Service Template: Load-balanced ECS Fargate Service

This AWS Proton service template provisions an application running on ECS Fargate that is load balanced.

It provides the following features:
* Route53 DNS domain is provisioned for each service instance
* ACM certificate automatically provisioned and configured in load balancer listener
* Docker build in pipeline uses `--cache-from` to speed up builds
* Hadolint used to lint Dockerfile
* AWS Elastic Container Registry scan is performed in the pipeline, blocks till complete and kills pipeline is critical vulnerabilities found

## Requirements

This section documents the requirements that this service template has of related resources.

### Environment Template

This service template has the following requirements from an environment template for it to be compatible:
1. Provides a VPC with two subnets
1. Provides an ECS cluster
1. Provides an ECS task execution role

### Workload

This service template has the following requirements for workloads to be deployed with it:
1. Serves HTTP traffic
1. Can pass an HTTP health check
1. Provides a `Dockerfile` in the root of the application source directory

## Reference

### Service Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `port` | The port the application serves traffic on | `number` | `8080` | yes |
| `health_check_path` | The path used to perform an HTTP health check | `string` | `/` | yes |
| `desired_count` | The default number of Fargate tasks you want running | `number` | `1` | yes |
| `task_size` | The size of the task you want to run | `string` | `x-small` | yes |
| `image` | The name/url of the container image | `string` | `ealen/echo-server:0.5.1` | yes |

### Pipeline Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `app_directory` | The sub-directory where the application code is located | `string` | `.` | yes |
| `unit_test_command` | The command to run to unit test the application code | `string` | `echo 'No tests'` | yes |
| `environment_account_ids` | The environment account ids for service instances using cross account environment, separated by , | `string` |  | yes |

### Outputs

| Name | Description |
|------|-------------|
| `ServiceEndpoint` | The URL to access the service |