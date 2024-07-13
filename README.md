# kubernetes-intro

### What is Kubernetes?
 -------------------------------------------------

Kubernetes is a popular container orchestrator. Container orchestration means making many servers act like one. Orchestrator actually takes our containers that we ask it to run and it takes series of servers or nodes and decides how to run those container workloads across servers/nodes. It runs on top of docker usually as a set of APIs in containers. Kubernetes provides sets of apis and cli to manage containers across servers/nodes. Many clouds providers have integrated kubernetes into their infrastructure being a popular orchestrations tools such as eks, aks,gks etc.


### Why Kubernetes?
 -------------------------------------------------

Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more.
Kubernetes provides you with:

- Service discovery and load balancing Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- Storage orchestration Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- Automated rollouts and rollbacks You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- Automatic bin packing You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- Self-healing Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- Secret and configuration management Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
- Batch execution In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
- Horizontal scaling Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
- IPv4/IPv6 dual-stack Allocation of IPv4 and IPv6 addresses to Pods and Services
- Designed for extensibility Add features to your Kubernetes cluster without changing upstream source code.

### Kubernetes Architecture Terminology
 -------------------------------------------------

- Kubernets     : The whole orchestration system. K8s or kube for short
- kubectl       : Cli to configure kubernetes and manage apps. Kubecontrol is the short form means we can control kubernetes using kubectl
- node          : A singel server in kubernetes cluster
- kubelet       : kubernetes agents running on nodes
- Control Plane : manages Kubernetes clusters and the workloads running on them. Include components like the API Server, Scheduler,etcd and Controller Manager.
- Pod           : one or more containers running together on one Node. Basic unit of deployment. Containers are always in the pod.
- Controller    : For creating/updating pods and other objects. Types of controllers include Deployment, Replicaset, Stateful,Job,CronJob, Stateful set, Daemonset and so on. 
- Service       : network endpoint to connect to a pod
- Namespace     : Filtered group of objects inside a cluster.
- Secrects,Configmaps and more

### Creating Pods in Kubernetes:
 -------------------------------------------------
we get three ways to create pods from the kubectl CLI:
- **kubectl run**     : use to create a single pod per command. Similar to docker run
  
  **Syntax:**     
    kubectl run service-name --image imagename
    
  **Usage:**        
    kubectl run my-nginx --image nginx
   -------------------------------------------------

- **kubectl create**  : use to create some resources via cli or yml file. Similar to docker create i.e network, container, volume
- **kubectl apply**   : create/update anything via yml file. Similar to docker stack deploy where we use yml file to deploy containers
