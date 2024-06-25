# Docker on AWS AppRunner

## Get the Code Ready

```
git clone 
cd docker-on-aws-apprunner
cp .devcontainer/devcontainer.example.json  .devcontainer/devcontainer.json
# Edit .devcontainer/devcontainer.json to update .aws path
# Edit aws/apprunner-nginx-service.yml and update ECR image repo:tag
```

## Build & Open in Container Environment (VSCode DevContainer)

If you don't use VS-Code and you may either use the devcontainer cli or other container alternatives or you just use the CLIs installed on your host OS.

## Docker Image

### Docker Build

```
```

### Create ECR Repository

```
```

### Tag and Push Docker Image

```
```

## AWS AppRunner

### Create IAM Role & Policy

```
```

### Create Nginx Service

Update the aws/apprunner-nginx-service.yml for

- `SourceConfiguration > ImageRepository > ImageIdentifier:` <YOUR_ECR_IMAGE_REPO_TAG>
- `SourceConfiguration > AuthenticationConfiguration > AccessRoleArn:` <IAM_ROLE_ARN>

```
```

### Test the Service


## All done? Let's clean up

- Delete the App Runner service
- Delete the role & policy
- Delete the ECR repository


## Author

- Chandra Shettigar