# Welcome to Udagram

This project make part of the Udacity's program: Cloud Developer Nanodegree Program. Here I will demonstrate that I have learned how to deploy microservices on the Amazon Web Service (AWS) - Elastic Kubernetes Service (EKS).

I am assuming that you already has an AWS account with: EKS; RDS (Postgres) and S3 Bucket; a DockerHub account and TravisCI account set up.

# Project Settings

## Prerequisites

You need to have the following applications installed in your local environment:

- Docker;
- AWS CLI;
- Kubectl;
- Code Editor (Visual Studio Code);
- Nice to have Postman to help you out debug and test your project.

Furthermore, you will need:

- Amazon Web Services (AWS) account (with EKS; S3 and RDS set up already) and
- DockerHub account.

## Cloning this project

```
git clone https://github.com/flaviofrancisco/udagram.git
```

This command will clone the following projects:

- [Udagram UI](https://github.com/flaviofrancisco/udagram-ui/tree/19887d2f9831aaf824748491c1cd3e6cadb970bc) (Front-end application developed with Ionic/ Angular);
- [Udagram Api Users](https://github.com/flaviofrancisco/udagram-api-users/tree/bb7da86e82cb185c20825c315591a699d801e940) (Node.js Api)
- [Udagram Api Feed](https://github.com/flaviofrancisco/udagram-api-feed/tree/7905cde8e7a54b2f77390d8119c25285e31c3917) (Node.js Api)
- [Reverse Proxy](https://github.com/flaviofrancisco/udagram-reverse-proxy/tree/f0c93bbe92225bd594d0c9bf6f5ae562bbafe7d9) Docker files that will create an image that later will be use as service to be a gateway for the api services;
- [Deployment](https://github.com/flaviofrancisco/udagram/tree/master/deployment) - folder with the manifest files to deploy it against AWS EKS and create the Docker images for each project (UI, APIs and Reverse Proxy).

## Settings

**IMPORTANT**

Before you start setting up your environment there is a small tweak that I left and need to be re-factored. The files in the folder: udagram-ui\src\environments must be updated accordingly with your environment and set up: environment.ts and environment.prod.ts.

```
export const environment = {
  production: false,
  appName: 'Udagram',
  apiHost: 'http:localhost:8080/api/v0'
};
```

The apiHost: property must be localhost:8080 to make it work locally or with your AWS Service where is located your NGINX reverse proxy.  

You will need to include some files like secrets; environment variables and nginx.conf that make part of the process to set up your environment and was not included in the project that I will describe in the next section.


### Deployment Folder

[This deployment folder](https://github.com/flaviofrancisco/udagram/tree/master/deployment) has two sub-folder as you can see:

- docker and
- kubernetes

#### Docker Folder

In this folder you will need to create tree files:

- aws-config.json
- nginx.conf and
- service.env

Don't forget to include those files in your .gitignore files for safty and use the following command to remove them from git:

```
git rm --cached aws-config.json
git rm --cached service.env
```
The content of each file will be shown next.

*aws-config.json*

```
{ "accessKeyId": "Your AWS Access key ID", "secretAccessKey": "Your AWS secret key generated when you create your AWS Account", "region": "Your preferable AWS region" }
```

*service.env*

```
POSTGRES_USERNAME=Your Database User Name
POSTGRES_PASSWORD=Your Database Password
POSTGRES_DB=Your Database Name
POSTGRES_HOST=Your AWS RDS database URL
AWS_REGION=Your preferable AWS region
AWS_PROFILE=Your Profile
AWS_BUCKET=Your S3 Bucket Name
URL=http://localhost:8100
JWT_SECRET=Your JWT Secret
```

*nginx.conf*

```
worker_processes 1;
  
events { worker_connections 1024; }
error_log /dev/stdout debug;
http {
    sendfile on;
    upstream user {
        server udagram-api-users:8080;
    }
    upstream feed {
        server udagram-api-feed:8080;
    }
    upstream ui {
        server udagram-ui:8100;
    }    
    
    proxy_set_header   Host $host;
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Host $server_name;
    
    server {
        listen 8080;
        location / {
          proxy_pass         http://ui;
        }
        location /api/v0/feed {         
            proxy_pass         http://feed;
        }
        location /api/v0/users {
            proxy_pass         http://user;
        }            
    }
}
```

Once you have the files properly set up you can run the following command:

```
$ docker-compose up -d --build
```

This will create the Docker images locally and create the container locally in your environment.

#### Kubernetes Folder

As you can see there is a config-files folder. In this config folder you need to include two files:

- aws-config.json and
- service.env

Don't forget to include those files in your .gitignore files for safty and use the following command to remove them from git:

```
git rm --cached aws-config.json
git rm --cached service.env
```
The content of each file will be shown next.

*aws-config.json*

```
{ "accessKeyId": "Your AWS Access key ID", "secretAccessKey": "Your AWS secret key generated when you create your AWS Account", "region": "Your preferable AWS region" }
```

*service.env*

```
POSTGRES_USERNAME=Your Database User Name
POSTGRES_PASSWORD=Your Database Password
POSTGRES_DB=Your Database Name
POSTGRES_HOST=Your AWS RDS database URL
AWS_REGION=Your preferable AWS region
AWS_PROFILE=Your Profile
AWS_BUCKET=Your S3 Bucket Name
URL=http://localhost:8100
JWT_SECRET=Your JWT Secret
```
Once you have your file in place you can create/ apply the manifest file to deploy it against your Kubernetes Cluster.
