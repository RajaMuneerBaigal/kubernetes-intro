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
we get three ways to create pods from the kubectl CLI: kubectl run, kubectl create, kubectl apply. Pods are specifically a kubernetes concept. Unlike docker we can't directly create a container in k8s so we create pods and then k8s creates the containers inside the pod. Kubernetes uses kubelet(agent running on node) to create containers which in turn tells the container runtime(docker,contained,podmon) resulting in creating containers. Every resource type in kubernetes that wants to create containers does it via pods.
![image](https://github.com/user-attachments/assets/27fcb4db-4d93-4e25-9f68-acbe74206a97)


- **kubectl run**     :use to create a single pod per command. Similar to docker run command.

  **Syntax:**     
    kubectl run anypodname --image imagename
  
  **Usage:**
    kubectl run my-alpine --image alpine 
    
   -------------------------------------------------
  
- **kubectl get**     :use to list all the pods i.e similar like docker ps The kubectl get is an important command as thats how we get info back from api. resource types can be pods,services, all and can be 70-80 resource types. 
  
  **Syntax:** 
    kubectl get pods/all/services

  
  **Usage:**
    kubectl get pods
    kubctl get all
    


   -------------------------------------------------

- **kubectl create**  : used to create many type of resources(pod,service,deployment and so on ) via cli or yml file. Similar to docker create i.e network, container, volume.
- 
   **Syntax:**
     
    kubectl create anyresourcetype resourcename --image imagename
  
  **Usage:**
  
    kubectl create deployment my-nginx --image nginx:alpine
-   When we request a deployment, it is sent to the control plane which has a web api to which we have been talking and it takes that record and stores it in the etcd database and that etcd database stores the data for deployment. There is a control manager inside the control plane and the manager's job is to look at all the different type of resources and make sure to assign a controller that is assigned to that resource type that we have requested in our case it is deployment or pod. And the job of a deployment resource is to do one thing to create replicaset. A deployment doesn't really creates pod's directly it creates replicasets because thats what help us in rolling updates making sure we can create new deployments without having to remove the old ones so incase of failure we can roll back to our previouse replicaset. When the control manager sees a record for deployment in etcd database it assings a replicaset controller and see's that it needs to create some pods so it creates the definition of those  pods by adding them to the etcd database and now a scheduler which continously takes to etcd notices that pods have not be scheduled to a node and once the pods have been scheduled to a node then kubelet creates those pods and that is why we run kubectl get all for a deployment we see all those replicaset and other stuf.
![image](https://github.com/user-attachments/assets/a0f6df07-6a38-43d1-8aea-67cc5466e012)
   -------------------------------------------------

- **kubectl delete**  : used to delete any type of resources(pod,service,deployment and so on ) via cli or yml file. Similar to docker rm i.e network, container, volume.
  
   **Syntax:**
    
    kubectl delete anyresourcetype resourcename
  
  **Usage:**
  
    kubectl delete deployment my-nginx
  
    kubectl delete pod my-nginx
    
   -------------------------------------------------
- **kubectl apply**   : create/update anything via yml file. Similar to docker stack deploy where we use yml file to deploy containers
  
    -------------------------------------------------

- **kubectl scale deployment**  : used to add more replicas to a deployment. It will change the deployment record. Controll Manager will see that only replica count has changed so it won't create a new replica set. It would only create if we change the image or tag
  
   **Syntax:**
    
    kubectl scale resourcetype resourcename --replicas anynumber
  
  **Usage:**
  
    kubectl scale deployment my-nginx --replicas 2
  
    kubectl scale deploy/my-nginx --replicas 2

    **Note**: deploy=deployment=deployments i.e we can use any of these

  -------------------------------------------------

### Inspecting Kubernetes Resources
It is very important when dealing with deployments/pods/services to know the status of that, how is it running,what resources it is creating,Is there any failure in creating resources. So to inspect kubernetes resources Kubectl provides us with the following commands.
- **kubectl get**: Used to get information about pods,deployments,replicasets,services and so on.
    **Usage**:
  
    kubectl get services
  
    kubectl get --help for more on how to use it
  
    kubectl get deployment my-nginx
  
    kubectl get deploy my-apache -o wide
  
    kubectl get service -o wide
  
    kubectl get pods -o yaml

    kubectl get pods -w

    kubectl get events --watch-only
  
    kubectl get nodes
  
   -------------------------------------------------
  
- **kubectl describe**: Kubectl get has a weakness that it can only show one resource at a time.We need a command that combines related resources. i.e parent/child, events of that resources so that where describe commands comes. It only shows important details about a pod,deployment,replicaset or service.
   
  **Syntax:**
  
    kubectl describe anyresourcetype 
  
    kubectl describe anyresourcetype resourcename
  
  **Usage**:
  
    kubectl describe deployment my-nginx
  
    kubectl describe pod my-nginx
  
    kubectl describe replicaset

    kubectl describe node nodename
  
    kubectl describe --help to get more details on how to use it.

------------------------------------------------------------

### Logs in Kubernetes
The kubernetes logs aren't stored in api or etcd database these logs are stored by default on each node within the container runtime. the kubernetes logs command tells the api to tell then the kubelet on each node to start send the logs back to the API so it can show us.

- **kubectl logs**: used to get the logs for a container running in pods
   
  **Syntax:**
  
    kubectl logs deploy/deploymentname

    kubectl logs pod podname
  
  **Usage**:

     kubectl logs deploy/my-nginx -c httpd     &emsp; &emsp; &emsp; &emsp; &emsp;  -c specifies image name

     kubectl logs deploy/my-nginx                        &emsp; &emsp; &emsp;   &emsp; &emsp; &emsp; &emsp; &emsp;    picks a random replica, only logs the first container logs defined in yaml file

     kubectl logs deploy/my-nginx --follow --tail 1      &emsp; &emsp; &emsp;               follows new log entries with the latest log line

     kubectl logs -l app=my-nginx                    &emsp; &emsp; &emsp;   &emsp; &emsp; &emsp; &emsp; &emsp;    to do a label search via deployment name which we can get using describe cmd


     kubectl logs pod my-nginx-5bd7979764-clxdp

     kubectl logs deploy/my-nginx --all-containers=true

