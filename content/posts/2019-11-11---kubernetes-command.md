---
title: Kubernetes command
date: '2019-11-11 17:43'
template: "post"
draft: false
slug: "/posts/kubernetes-command/"
category: "Kubernetes"
tags:
  - "Kubernetes"
description:
socialImage: "/media/image-2.jpg"
---
## Nodes
- Get all nodes of the cluster
```
kubectl get nodes
```

***

## Pods
- List all the pods in all namespaces.
```
kubectl get pods --all-namespaces
```

- list the pods in the default namespace
```
kubectl get pods
```

***

## Deployments
- get all deployments
```
kubectl get deployments
```
