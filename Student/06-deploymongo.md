# Challenge 6: Deploy MongoDB to AKS

[< Previous Challenge](./05b-scaling.md) - **[Home](../README.md)** - [Next Challenge >](./07-updaterollback.md)

## Introduction

At your daily standup, you heard that your developers will be refactoring the application to V2, and are going to need to store data in MongoDB.  

## Description

In this challenge we'll be installing MongoDB into our cluster.

- Create a manifest for MongoDB. Use the official MongoDB container image from https://hub.docker.com/_/mongo
- MongoDB is a _database_, and thus rather than using a **Deployment**, it's best to use a **Stateful Set** object to deploy it to Kubernetes.
- Confirm it is running with:
	- `kubectl exec -it <mongo pod name> -- mongo "--version"`
- Hint:  Follow the pattern you used in Challenge 4 and create a deployment and service YAML file for MongoDB.
- Hint: MongoDB runs on port 27017
## Success Criteria

1. MongoDB is installed and run in our cluster
1. The `mongo --version` command can be run in a pod and shown to work.

## Learning Resources
* [Stateful Sets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
* [Stateful Set Basics](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/)
* [Deployments vs Stateful Sets](https://stackoverflow.com/questions/41583672/kubernetes-deployments-vs-statefulsets)