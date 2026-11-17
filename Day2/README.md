# Day 2

## Info - Container Orchestration Platform
<pre>
- They provide all the essentials features required to make your application highly available (HA)
- it provides in-built load-balancing
- it provides in-built service abstraction
- it provides in-built Service Discovery
- it provides in-built scale up/down feature
- it provides rolling update feature
  - with this we can upgrade our application from one version to other without downtime
- it provides in-built application monitoring features
- it provides features to self-heal your applications
- it provides features to make your application access only locally or externally
- examples
  - Docker SWARM
  - Kubernetes
  - Red Hat Openshift
  - Rancher
</pre>

## Info - Docker SWARM
<pre>
- this is Docker Inc native Container Orchestration Platform
- it supports only Docker containerized application workloads
- Docker SWARM works as cluster of many machines
- Some of those machines acts as a master and some of them act as a slave/worker 
- Applications can be deployed with the help of Docker Images
- In order to deploy your application into Docker SWARM, you need a create a Custom Docker Image
  that has your application and all your application dependencies
- With that custom image, we can deploy our application into Docker SWARM
- it is very easy to setup Docker SWARM on a laptop with very average hardware configuration also
- it is easy to install, easy to learn 
- it is an ideal setup of dev & QA
- it is not a production-grade Container Orchestration Platform
- hence, it is almost never used in production
- we will struck with only Docker, as it won't support other type of Containers
</pre>

## Info - Kubernetes
<pre>
- it is a Container Orchestration Platform developed by Google
- originally it was developed and used within Google as Borg
- Kubernetes is developed in Golang
- After using Borg in several complex projects internally, Google refactored, improved it and made it as a opensource tool
  as Kubernetes
- it is a robust, opensource, production-grade Container Orchestration Platform
- it is command-line only
- it works as a cluster ( group of many servers )
- these servers are classified as master and worker
- there can several masters and worker nodes (servers)
- Master Nodes
  - only the master nodes runs the Control Plane Components
  - on need-basis, this node can be configured to allow deploying user applications as well
- Worker Nodes
  - this is where user applications will be running
- on Master and Worker Node, we can any Linux OS of our choice
- Every Node i.e Master and Worker node, we need to install the Container Runtimes we prefer use in the cluster wide
- Kubernetes supports many different types of Container runtimes and Container Engines
- applications are deploy as Pod
- Pod is the smallest unit we can deploy into Kubernetes
- Kubernetes is also called in short as K8s
- it provides all the basic boilerplate features required to extend Kubernetes
  - For instance
    - it supports adding a new type of Resource in Kubernetes by defining a Custom Resource Definition (CRD) in a yaml file
    - to manage custom resources, we also need to provide a custom controller
    - the combination of one or more Custom Resources and one or more Controllers is called Kubernetes Operator
    - this is way we can extend Kubernetes API ( features )
</pre>

## Info - Red Hat Openshift
<pre>
- it is Red Hat's distribution of Kubernetes
- Openshift supports everything that Kubernetes support + many addition features
- it is developed on top of opensource Kubernetes
- it supports both CLI and Webconsole
- it supports User Management
  - hence Role Based Access Control is possible
- it supports deploying applications
  - from Container Image just like Kubernetes
  - as well from source code, it is called S2I (Source to Image)
- it supports features like
  - Webconsole
  - User Management
  - Build
  - BuildConfig
  - Route
  - Project
  - ImageStream
  - Internal Container Registry
- as it is an enterprise product that requires license, it also comes with world-wide support from 
  Red Hat ( an IBM company )
</pre>
