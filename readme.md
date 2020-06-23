# Welcome to Udagram

This project make part of the Udacity's program: Cloud Developer Nanodegree Program. Here I will demonstrate that I have learned how to deploy microservices on the Amazon Web Service (AWS) - Elastic Kubernetes Service (EKS).

# Project Settings

## Prerequisites

You need to have the following applications installed in your local environment:

- Docker;
- AWS CLI;
- Kubectl;
- Code Editor (Visual Studio Code);
- Nice to have Postman to help you out debug and test your project.

Furthermore, you will need:

- Amazon Web Services (AWS) account and
- DockerHub account.

## Cloning this project

```
git clone https://github.com/flaviofrancisco/udagram.git
```

This command will clone the following projects:

- [Udagram UI](https://github.com/flaviofrancisco/udagram-ui/tree/19887d2f9831aaf824748491c1cd3e6cadb970bc) (Front-end application developed with Ionic/ Angular);
- [Udagram Api Users](https://github.com/flaviofrancisco/udagram-api-users/tree/bb7da86e82cb185c20825c315591a699d801e940) (Node.js Api)
- [Udagram Api Feed](https://github.com/flaviofrancisco/udagram-api-feed/tree/7905cde8e7a54b2f77390d8119c25285e31c3917) (Node.js Api)
- [Reverse Proxy](https://github.com/flaviofrancisco/udagram-reverse-proxy/tree/f0c93bbe92225bd594d0c9bf6f5ae562bbafe7d9) that will work a gateway for the api services;
- [Deployment](https://github.com/flaviofrancisco/udagram/tree/master/deployment) - folder with the manifest files to deploy it against AWS EKS.
