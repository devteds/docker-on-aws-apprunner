# Docker on AWS AppRunner

This source code is for the [short course](https://youtu.be/1BZlUneXLCs) that does a walkthrough of creating a simple website, dockerize as nginx image, create AWS ECR repository, push the image to ECR and finally deploy it to AppRunner along with creating necessary IAM resources.

## Announcement: Course on Kubernetes

> If you want to start deploying your containers to Kubernetes, especially on AWS EKS, [check this course on Kubernetes](https://www.devteds.com/kubernetes-course-aws-eks-terraform) that walkthrough creating Kubernetes cluster on AWS EKS using Terraform and deploying multiple related containers applications to Kubernetes and more. https://www.devteds.com/kubernetes-course-aws-eks-terraform 

---

[Course video link](https://youtu.be/1BZlUneXLCs)

[![Course Video Link](./doc/docker-on-aws-apprunner.png)](https://youtu.be/1BZlUneXLCs)

Visit https://devteds.com to watch all the videos and courses on DevOps and Cloud courses.

## Terminal Window Log

### Get the code ready

```
git clone 
cd docker-on-aws-apprunner
cp .devcontainer/devcontainer.example.json  .devcontainer/devcontainer.json
# Edit .devcontainer/devcontainer.json to update .aws path
# Edit aws/apprunner-nginx-service.yml and update ECR image repo:tag
```

## Build & Open in Container Environment (VSCode DevContainer)

If you don't use VS-Code and you may either use the devcontainer cli or other container alternatives or you just use the CLIs installed on your host OS.

And if you're using DevContainer, `DevContainers: ReBuild & ReOpen in Container`

## Docker Build: Build website image

```
cd web
docker build -t nginx-website .
# or
# docker build --platform linux/amd64 -t nginx-website .
```

### ECR Repository & Docker Push

```
aws ecr create-repository --repository-name nginx-ws
docker tag nginx-website <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/nginx-ws:v1
aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com
docker push <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/nginx-ws:v1
```

## AWS AppRunner

Got to the root of the project folder.

### Create IAM Role & Policy

```
aws iam create-role --role-name demo-apprunner-role --assume-role-policy-document file://$PWD/aws/trust-policy.json
aws iam create-policy --policy-name demo-apprunner-ecr-policy --policy-document file://$PWD/aws/apprunner-nginx-service.json
aws iam attach-role-policy --role-name demo-apprunner-role --policy-arn arn:aws:iam:<AWS_ACCOUNT_ID>:poilcy/demo-apprunner-ecr-policy
```

### Create Nginx Service

Update the aws/apprunner-nginx-service.yml for

- `SourceConfiguration > ImageRepository > ImageIdentifier:` `<AWS-ACCOUNT-ID>`, `<AWS-REGION>`
- `SourceConfiguration > AuthenticationConfiguration > AccessRoleArn:` `<AWS-ACCOUNT-ID>`

```
aws apprunner create-service --cli-input-yaml file://$PWD/aws/apprunner-nginx-service.yml
```

### Test the Service

```
aws apprunner list-services
```

Use the `ServiceUrl" from the above respose to hit the app on browser and verify.


## All done? Let's clean up

- Delete the App Runner service `aws apprunner delete-service --service-arn <SERIVE_ARN>`
- Delete the role & policy
- Delete the ECR repository


## Author

- Chandra Shettigar


## Tools & Versions I used

- MacOS, Apple M1 chip
- Docker 24.0.2, build cb74dfc
- Docker Compose version v2.18.1
- VSCode 1.87.2 (Universal)
- aws-cli/2.16.4 
