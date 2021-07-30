# AWS Proton Service Template: App Runner Service from Image (Cloud Native Buildpacks)

This AWS Proton service template provisions an AWS App Runner service using a container image source, with the image being built with [Cloud Native Buildpacks](https://buildpacks.io).

It provides the following features:
* Builds the container image in the pipeline to be deployed to App Runner
* Cloud Native Buildpacks are used to build the container image, meaning `Dockerfile` is not used
* AWS Elastic Container Registry scan is performed in the pipeline, blocks till complete and kills pipeline is critical vulnerabilities found

## Requirements

This section documents the requirements that this service template has of related resources.

### Environment Template

This service template has no requirements from an environment template.

### Workload

This service template has the following requirements for workloads to be deployed with it:
1. Serves HTTP traffic
1. Can pass an HTTP health check
1. Is compatible with a Cloud Native Buildpacks builder

## Usage

First, create the service template.

```
aws proton create-service-template \
  --name "apprunner-image-buildpack" \
  --display-name "App Runner Service from Image (Buildpacks)" \
  --description "App Runner service which uses a container image build from using Cloud Native Buildpacks"
```

Now create a version which contains the contents of the sample service template. Compress the sample template files and register the version:

```
tar -zcvf svc-template.tar.gz templates/service/apprunner-image-buildpack

aws s3 cp svc-template.tar.gz s3://proton-cli-templates-${ACCOUNT_ID}/svc-template.tar.gz --region us-west-2

rm svc-template.tar.gz

aws proton create-service-template-version \
  --template-name "apprunner-image-buildpack" \
  --description "Initial version" \
  --source s3="{bucket=proton-cli-templates-${ACCOUNT_ID},key=svc-template.tar.gz}" \
  --compatible-environment-templates '[{"templateName":"apprunner-env","majorVersion":"1"}]'
```

Wait for the service template to be successfully registered:

```
aws proton wait service-template-version-registered \
  --template-name "apprunner-image-buildpack" \
  --major-version "1" \
  --minor-version "0"
  
aws proton get-service-template-version \
  --template-name "apprunner-image-buildpack" \
  --major-version "1" \
  --minor-version "0"
```

You can now publish the service template version, making it available for users in your AWS account to create Proton services.

```
aws proton update-service-template-version \
  --template-name "apprunner-image-buildpack" \
  --major-version "1" \
  --minor-version "0" \
  --status "PUBLISHED"
```

## Reference

### Service Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `port` | The port the application serves traffic on | `number` | `8080` | yes |
| `health_check_path` | The path used to perform an HTTP health check | `string` | `/` | yes |
| `task_size` | The size of the task you want to run | `string` | `x-small` | yes |
| `image` | The name/url of the container image | `string` |  | no |

### Pipeline Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `app_directory` | The sub-directory where the application code is located | `string` | `.` | yes |
| `cnb_builder` | The Cloud Native Buildpacks builder to use to build the container image | `string` | `paketobuildpacks/builder:full` | yes |
| `unit_test_command` | The command to run to unit test the application code | `string` | `echo 'No tests'` | yes |
| `environment_account_ids` | The environment account ids for service instances using cross account environment, separated by , | `string` |  | yes |

### Outputs

| Name | Description |
|------|-------------|
| `ServiceEndpoint` | The URL to access the service |