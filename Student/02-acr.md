# Challenge 2: The Azure Container Registry

[< Previous Challenge](01-prereqs.md) - **[Home](../README.md)** - [Next Challenge >](./03-k8sintro.md)

## Introduction

The first step in our journey will be to take our application and package it as a container image, which will be stored in a container registry.

## Description

- If you haven't done so already, clone the git repo **https://github.dev/onemtc/AKSHack**.  You will find the source code for `content-web` and `content-api` in the folder **Student/Resources/Challenge 2**.  Review how the provided Dockerfiles correspond to each of these applications.
- Deploy an Azure Container Registry (ACR)
- Ensure your ACR has proper permissions and credentials configured
- Use the cloud-based container image building feature of ACR Tasks to build and store images for `content-web` and `content-api`
- List all images in your ACR


## Success Criteria

1. You have provisioned a new Azure Container Registry
1. You have built and deployed your container images to the registry.
2. You can log into the registry and see all images.
