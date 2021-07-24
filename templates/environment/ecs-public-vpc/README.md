# AWS Proton Environment Template: ECS Public VPC

This AWS Proton environment template provisions the basic foundations for an ECS Fargate service running in public VPC subnets

It provides the following features:
* A VPC with two public subnets
* A Route53 hosted zone that can be used for service instances to create domains
* An ECS cluster with associated task execution IAM role

## Reference

### Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `vpc_cidr` | The CIDR range for your VPC | `string` | `10.0.0.0/16` | yes |
| `subnet_one_cidr` | The CIDR range for subnet one | `string` | `10.0.0.0/24` | yes |
| `subnet_two_cidr` | The CIDR range for subnet two | `string` | `10.0.1.0/24` | yes |
| `root_hosted_zone` | The Route53 root hosted zone ID (ex. `Z8VLZEXAMPLE`) | `string` | | yes |
| `route53_domain` | The Route53 sub-domain to create (ex. `prod.example.com`) | `string` |  | yes |

### Outputs

| Name | Description |
|------|-------------|
| `ClusterName` | The name of the ECS cluster |
| `ECSTaskExecutionRole` | The ARN of the ECS role |
| `VpcId` | The name of the ECS cluster |
| `PublicSubnetOne` | Public subnet one |
| `PublicSubnetTwo` | Public subnet two |
| `ContainerSecurityGroup` | A security group used to allow Fargate containers to receive traffic |
| `HostedZone` | The Route53 hosted zone ID for DNS records |
| `DnsDomain` | The Route53 DNS domain |