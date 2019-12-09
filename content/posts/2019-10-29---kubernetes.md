---
title: Kubernetes
date: '2019-10-29 11:40'
template: "post"
draft: false
slug: "/posts/kubernetes/"
category: "Kubernetes"
tags:
  - "Kubernetes"
description:
socialImage: "/media/image-2.jpg"
---
- At its base, Kubernetes brings together individual physical or virtual machines into a cluster using a shared network to communicate between each server. This cluster is the physical platform where all Kubernetes components, capabilities, and workloads are configured.

- The machines in the cluster are each given a role within the Kubernetes ecosystem. 
  + **Master server**
    + One server (or a small group in highly available deployments) functions as the **_master server_**. 
    + This server acts as a gateway and brain for the cluster by exposing an API for users and clients, health checking other servers, deciding how best to split up and assign work (known as “scheduling”), and orchestrating communication between other components.
    + The master server acts as the primary point of contact with the cluster and is responsible for most of the centralized logic Kubernetes provides.

  + **Nodes**
    + the other machines in the cluster are designated as **_nodes_**: servers responsible for accepting and running workloads using local and external resources. To help with isolation, management, and flexibility, Kubernetes runs applications and services in containers, so each node needs to be equipped with a container runtime (like Docker or rkt).
    + The node receives work instructions from the master server and creates or destroys containers accordingly, adjusting networking rules to route and forward traffic appropriately.
    + Each node has a **_Kubelet_**, which is an agent for managing the node and communicating with the Kubernetes master.

- To start up an application or service, a declarative plan is submitted in JSON or YAML defining what to create and how it should be managed. The master server then takes the plan and figures out how to run it on the infrastructure by examining the requirements and the current state of the system.

## Kubernetes objects and workloads
* While containers are the underlying mechanism used to deploy applications, Kubernetes uses additional layers of abstraction over the container interface to provide scaling, resiliency, and life cycle management features. 
* Instead of managing containers directly, user's define and interact with instances composed of various primitives provided by the Kubernetes object model.
	
### Pods
* A pod is the most basic unit that Kubernetes deals with. 
* Containers themselves are not assigned to hosts. Instead, one or more tightly coupled containers are encapsulated in an object called a pod.
* A pod generally represents one or more containers that should be controlled as a single application. Pods consist of containers that operate closely together, share a life cycle, and should always be scheduled on the same node. They are managed entirely as a unit and share their environment, volumes, and IP space. 
* In spite of their containerized implementation, you should generally think of pods as a single, monolithic application to best conceptualize how the cluster will manage the pod’s resources and scheduling.
* Usually, pods consist of a main container that satisfies the general purpose of the workload and optionally some helper containers that facilitate closely related tasks.
  + For example, a pod may have one container running the primary application server and a helper container pulling down files to the shared filesystem when changes are detected in an external repository. 
* Generally, users should not manage pods themselves, because they do not provide some of the features typically needed in applications (like sophisticated life cycle management and scaling). Instead, users are encouraged to work with higher level objects that use pods or pod templates as base components but implement additional functionality.
		
### Replication Controllers and Replication Sets
* **A replication controller** is an object that defines a pod template and control parameters to scale identical replicas of a pod horizontally by increasing or decreasing the number of running copies.
* The replication controller is responsible for ensuring that the number of pods deployed in the cluster matches the number of pods in its configuration.
* Replication controllers can also perform rolling updates to roll over a set of pods to a new version one by one, minimizing the impact on application availability.
* **Replication sets** are an iteration on the replication controller design with greater flexibility in how the controller identifies the pods it is meant to manage.
* Replication sets are beginning to replace replication controllers because of their greater replica selection capabilities, but they are not able to do rolling updates to cycle backends to a new version like replication controllers can.
* Instead, replication sets are meant to be used inside of additional, higher level units that provide that functionality.
* Like pods, both replication controllers and replication sets are rarely the units you will work with directly. While they build on the pod design to add horizontal scaling and reliability guarantees, they lack some of the fine grained life cycle management capabilities found in more complex objects.
		
### Deployments
* Deployments are one of the most common workloads to directly create and manage. Deployments use replication sets as a building block, adding flexible life cycle management functionality to the mix.
* Deployments are a high level object designed to ease the life cycle management of replicated pods. Deployments can be modified easily by changing the configuration and Kubernetes will adjust the replica sets, manage transitions between different application versions, and optionally maintain event history and undo capabilities automatically. Because of these features, deployments will likely be the type of Kubernetes object you work with most frequently.
		
### Stateful Sets
* Stateful sets are specialized pod controllers that offer ordering and uniqueness guarantees. Primarily, these are used to have more fine-grained control when you have special requirements related to deployment ordering, persistent data, or stable networking. 
  + For instance, stateful sets are often associated with data-oriented applications, like databases, which need access to the same volumes even if rescheduled to a new node.
* Stateful sets provide a stable networking identifier by creating a unique, number-based name for each pod that will persist even if the pod needs to be moved to another node. Likewise, persistent storage volumes can be transferred with a pod when rescheduling is necessary. The volumes persist even after the pod has been deleted to prevent accidental data loss.
* When deploying or adjusting scale, stateful sets perform operations according to the numbered identifier in their name. This gives greater predictability and control over the order of execution, which can be useful in some cases.
		
### Services
* a service is a component that acts as a basic internal load balancer and ambassador for pods. A service groups together logical collections of pods that perform the same function to present them as a single entity.
* A service’s IP address remains stable regardless of changes to the pods it routes to. By deploying a service, you easily gain discoverability and can simplify your container designs.
* Any time you need to provide access to one or more pods to another application or to external consumers, you should configure a service.
  + Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service.
  + For instance, if you have a set of pods running web servers that should be accessible from the internet, a service will provide the necessary abstraction. 
  + Likewise, if your web servers need to store and retrieve data, you would want to configure an internal service to give them access to your database pods.
* The set of Pods targeted by a Service is usually determined by a 'LabelSelector'.
  + Labels can be attached to objects at creation time or later on. They can be modified at any time.
			
### Volumes and Persistent Volumes
* Kubernetes uses its own volumes abstraction that allows data to be shared by all containers within a pod and remain available until the pod is terminated. This means that tightly coupled pods can easily share files without complex external mechanisms. 
* Container failures within the pod will not affect access to the shared files. Once the pod is terminated, the shared volume is destroyed, so it is not a good solution for truly persistent data.
* **Persistent volumes** are a mechanism for abstracting more robust storage that is not tied to the pod life cycle. Instead, they allow administrators to configure storage resources for the cluster that users can request and claim for the pods they are running. Once a pod is done with a persistent volume, the volume’s reclamation policy determines whether the volume is kept around until manually deleted or removed along with the data immediately. 
* Persistent data can be used to guard against node-based failures and to allocate greater amounts of storage than is available locally.
