# AWS Proton Environment Template: ECS Public/Private VPC

This AWS Proton environment template provisions the basic foundations for an ECS Fargate service running in public VPC subnets

It provides the following features:
* A VPC with two public subnets
* A Route53 hosted zone that can be used for service instances to create domains
* An ECS cluster with associated task execution IAM role

## Usage

First, create an environment template, which will contain all of the environment template's versions.

```
aws proton create-environment-template \
  --name "ecs-public-private-vpc" \
  --display-name "ECS Public/Private VPC" \
  --description "VPC with public access, private compute, and an ECS Cluster"
```

Now create a version which contains the contents of the sample environment template. Compress the sample template files and register the version:

```
tar -zcvf env-template.tar.gz templates/environment/ecs-public-private-vpc

aws s3 cp env-template.tar.gz s3://proton-cli-templates-${ACCOUNT_ID}/env-template.tar.gz

rm env-template.tar.gz

aws proton create-environment-template-version \
  --template-name "ecs-public-private-vpc" \
  --description "Initial version" \
  --source s3="{bucket=proton-cli-templates-${ACCOUNT_ID},key=env-template.tar.gz}"
```

Wait for the environment template version to be successfully registered:

```
aws proton wait environment-template-version-registered \
  --template-name "ecs-public-private-vpc" \
  --major-version "1" \
  --minor-version "0"
  
aws proton get-environment-template-version \
  --template-name "ecs-public-private-vpc" \
  --major-version "1" \
  --minor-version "0"
```

You can now publish the environment template version, making it available for users in your AWS account to create Proton environments.

```
aws proton update-environment-template-version \
  --template-name "ecs-public-private-vpc" \
  --major-version "1" \
  --minor-version "0" \
  --status "PUBLISHED"
```

## Reference

### Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `vpc_cidr` | The CIDR range for your VPC | `string` | `10.0.0.0/16` | yes |
| `public_subnet_one_cidr` | The CIDR range for public subnet one | `string` | `10.0.0.0/24` | yes |
| `public_subnet_two_cidr` | The CIDR range for public subnet two | `string` | `10.0.1.0/24` | yes |
| `private_subnet_one_cidr` | The CIDR range for private subnet one | `string` | `10.0.2.0/24` | yes |
| `private_subnet_two_cidr` | The CIDR range for private subnet two | `string` | `10.0.3.0/24` | yes |
| `root_hosted_zone` | The Route53 root hosted zone ID (ex. `example.com`) | `string` | | yes |
| `dns_domain` | The Route53 sub-domain to create (ex. `prod`) | `string` |  | yes |

### Outputs

| Name | Description |
|------|-------------|
| `ClusterName` | The name of the ECS cluster |
| `ECSTaskExecutionRole` | The ARN of the ECS role |
| `VpcId` | The name of the ECS cluster |
| `PublicSubnetOne` | Public subnet one |
| `PublicSubnetTwo` | Public subnet two |
| `PrivateSubnetOne` | Private subnet one |
| `PrivateSubnetTwo` | Private subnet two |
| `ComputeSubnetOne` | Compute subnet one |
| `ComputeSubnetTwo` | Compute subnet two |
| `ContainerSecurityGroup` | A security group used to allow Fargate containers to receive traffic |
| `HostedZone` | The Route53 hosted zone ID for DNS records |
| `DnsDomain` | The Route53 DNS domain |