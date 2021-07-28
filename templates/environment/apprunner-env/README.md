# AWS Proton Environment Template: App Runner Environment

This AWS Proton environment template provisions no infrastructure, and only prompts for a placeholder image to be used before the App Runner service is deployed properly.

## Usage

First, create an environment template, which will contain all of the environment template's versions.

```
aws proton create-environment-template \
  --name "apprunner-env" \
  --display-name "App Runner Environment" \
  --description "Environment for App Runner services"
```

Now create a version which contains the contents of the sample environment template. Compress the sample template files and register the version:

```
tar -zcvf env-template.tar.gz templates/environment/apprunner-env

aws s3 cp env-template.tar.gz s3://proton-cli-templates-${ACCOUNT_ID}/env-template.tar.gz

rm env-template.tar.gz

aws proton create-environment-template-version \
  --template-name "apprunner-env" \
  --description "Initial version" \
  --source s3="{bucket=proton-cli-templates-${ACCOUNT_ID},key=env-template.tar.gz}"
```

Wait for the environment template version to be successfully registered:

```
aws proton wait environment-template-version-registered \
  --template-name "apprunner-env" \
  --major-version "1" \
  --minor-version "0"
  
aws proton get-environment-template-version \
  --template-name "apprunner-env" \
  --major-version "1" \
  --minor-version "0"
```

You can now publish the environment template version, making it available for users in your AWS account to create Proton environments.

```
aws proton update-environment-template-version \
  --template-name "apprunner-env" \
  --major-version "1" \
  --minor-version "0" \
  --status "PUBLISHED"
```

## Reference

### Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| `placeholder_image` | Placeholder container image used before the application is deployed | `string` |  | yes |

### Outputs

| Name | Description |
|------|-------------|
| `PlaceholderImage` | See `placeholder_image` |