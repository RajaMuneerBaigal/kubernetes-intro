# kubernetes-intro

## <ins>What is Kubernetes? </ins>
 -------------------------------------------------

Kubernetes is a popular container orchestrator. Container orchestration means making many servers act like one. Orchestrator actually takes our containers that we ask it to run and it takes series of servers or nodes and decides how to run those container workloads across servers/nodes. It runs on top of docker usually as a set of APIs in containers. Kubernetes provides sets of apis and cli to manage containers across servers/nodes. Many clouds providers have integrated kubernetes into their infrastructure being a popular orchestrations tools such as eks, aks,gks etc.


## Why Kubernetes?
 -------------------------------------------------

Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more.
Kubernetes provides you with:

- **Service discovery and load balancing:**
   Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that 
   the deployment is stable.
- **Storage orchestration:**
   Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks:**
   You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate 
   Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing:**
   You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can 
  fit containers onto your nodes to make the best use of your resources.
- **Self-healing:**
  Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Secret and configuration management:**
  Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
- **Batch execution:**
  In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
- **Horizontal scaling:**
  Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
- **IPv4/IPv6 dual-stack:**
  Allocation of IPv4 and IPv6 addresses to Pods and Services
- **Designed for extensibility:**
  Add features to your Kubernetes cluster without changing upstream source code.

## Kubernetes Important Components and Terminologies
 -------------------------------------------------

- **Kubernets**     : The whole orchestration system. K8s or kube for short
- **kubectl**       : Cli to configure kubernetes and manage apps. Kubecontrol is the short form means we can control kubernetes using kubectl
- **node**          : A single server in kubernetes cluster
- **kubelet**       : kubernetes agents running on nodes
- **Control Plane** : manages Kubernetes clusters and the workloads running on them. Include components like the API Server, Scheduler,etcd and Controller Manager.
- **Pod**           : one or more containers running together on one Node. Basic unit of deployment. Containers are always in the pod. usually preferred to run one container per pod.
- **Controller**    : For creating/updating pods and other objects. Types of controllers include Deployment, Replicaset, Stateful,Job,CronJob, Stateful set, Daemonset and so on. 
- **Service**       : network endpoint to connect to a pod. When a pod dies and gets recreated its ip changes so its not a good sign especially when we are using pods ips to communicate. services have a 
                      permanent ip address which can be attached to a pod as its lifecycle to pod is not connected so even if a pod dies the service ip will still be there. its a permanent ip and load 
                      balancer as well
- **Ingress**       : used to route traffic into the cluster
- **Namespace**     : Filtered group of objects inside a cluster.
- **Configmaps**    : Config maps used for external configurations of our app. i.e database_url. Its connected to a pod so the pod can get the data from the configmap.
- **Secrects**      : used to store secrets such as db_pass etc. Much like config map but it stores the important secrets in base64 encoded mode.
- **Volumes**       : used for persistency of data because if a pod restarts it lost its data.
- **Deployments**   : blueprints for an application pods.
- **Stateful sets** : statefulsets used for applications like databases. It allows us to read/write to a database without having to worry about any conflicts.

## Worker Node components :
 - **Kubelet**             : a component  that interacts with both container runtime and node
 - **Kubeproxy**           : a component responsible for forwarding request from services to pods
 - **Container Runtime**   : A component necessary for running containers (Docker,Podman,CRIO)

## Master Node Components:
- **Api Server**           : A component that lets us interact with kubernetes cluster using a cli(kubectl). Performs important operations like cluster gateway and also acts as a gatekeeper for 
                            authentication.
  
- **Scheduler**            : A component that based on request came through api server schedules the job of creating a deployment intelligently by choosing a node.
  
- **Controller Manager**   : A component responsible for detecting the state changes. e.g dying of a pod, or creation of a pod

- **Etcd**                 : Brain of the cluster stores every information e.g cluster changes get stored in this database, all the processes that are related to scheduler and controller manager are 
                             done by the information it gets from etcd
  
## Creating Pods in Kubernetes:
 -------------------------------------------------
we get three ways to create pods from the kubectl CLI:
 - **kubectl run**
 - **kubectl create**
 - **kubectl apply**

 Pods are specifically a kubernetes concept. Unlike docker we can't directly create a container in k8s so we create pods and then k8s creates the containers inside the pod. Kubernetes uses kubelet(agent running on node) to create containers which in turn tells the container runtime(docker,contained,podmon) resulting in creating containers. Every resource type in kubernetes that wants to create containers does it via pods. Pods once created gets their own internal ip. when a pods gets down or crashes a new pod is created and its ip will be different then the previous one. 
![image](https://github.com/user-attachments/assets/27fcb4db-4d93-4e25-9f68-acbe74206a97)


- <ins>**kubectl run**</ins>     :use to create a single pod per command. Similar to docker run command.

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

- **kubectl create**  : used to create many type of resources(pod,service,deployment and so on ) via cli(except pod) or yml file. Similar to docker create i.e network, container, volume.
- 
   **Syntax:**
     
    kubectl create anyresourcetype resourcename --image imagename
  
  **Usage:**
  
    kubectl create deployment my-nginx --image nginx:alpine
-   When we request a deployment, it is sent to the control plane which has a web api(api server) to which we have been talking and it takes that record and stores it in the etcd database . There is a control manager inside the control plane and the manager's job is to look at all the different type of resources and make sure to assign a controller that is assigned to that resource type that we have requested in our case it is deployment. And the job of a deployment resource is to do one thing to create replicaset. A deployment doesn't really creates pod's directly it creates replicasets because thats what help us in rolling updates making sure we can create new deployments without having to remove the old ones so incase of failure we can roll back to our previouse replicaset. When the control manager sees a record for deployment in etcd database it assings a replicaset controller and see's that it needs to create some pods so it creates the definition of those  pods by adding them to the etcd database and now a scheduler which continously takes to etcd notices that pods have not be scheduled to a node and once the pods have been scheduled to a node then kubelet creates those pods and that is why we run kubectl get all for a deployment we see all those replicaset and other stuf.
![image](https://github.com/user-attachments/assets/a0f6df07-6a38-43d1-8aea-67cc5466e012)
   -------------------------------------------------
- **kubectl edit**  : used to edit any type of resources(pod,service,deployment and so on ) via cli.
  
   **Syntax:**
    
    kubectl edit anyresourcetype resourcename
  
  **Usage:**
  
    kubectl edit deployment my-nginx
  
    kubectl edit pod my-nginx

   -------------------------------------------------

  - **kubectl exec**  : used to get inside a container in a pod/deployment. i.e just like docker exec
    
   **Syntax:**
    
    kubectl exec -it  podname/deploymentname -- /bin/sh
  
  **Usage:**
  
    kubectl exec -it deployment/my-nginx -- /bin/sh
  
    kubectl exec -it nginx-b4ccb96c6-nt2lc -- /bin/sh

   -------------------------------------------------
- **kubectl delete**  : used to delete any type of resources(pod,service,deployment and so on ) via cli or yml file. Similar to docker rm i.e network, container, volume.
  
   **Syntax:**
    
    kubectl delete anyresourcetype resourcename
  
  **Usage:**
  
    kubectl delete deployment my-nginx
  
    kubectl delete pod my-nginx

    kubectl delete service/httpenv service/httpenv-np service/httpenv-lb deployment/httpenv

    kubectl delete deployment --all

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

## Inspecting Kubernetes Resources
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

## Logs in Kubernetes
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
  
------------------------------------------------------------

## Exposing Ports in Kubernetes
Once we have pods running in our Kubernetes we probably want to expose them to external services which means allowing them to access connections in our cluster from outside cluster. Unlike docker it doesn't expose them directly we need to create services for them via which we can expose them. A service is an endpoint that is consistent so that the other things inside or outside the cluster might be able to access it. A service is a stable address for pod(s). When we create a Pod that doesn't automatically get a DNS name for external connectivity with an IP address we need to do it via service on top of that existing pod. There are various ways besides the expose command below to create a service. CoreDns being a part of the control plane is a reason that allows us to resolve services by name. How we get traffic to our service essentially is the job of one of the four different service types mentioned below. Cluster IP and node port are always available in Kubernetes

- ClusterIP(default)
   - Single, Internal virtual ip allocated
   - only reachable from within the cluster
   - pods can reach service on app port number
- NodePort: Designed for something outside the cluster to talk to our services via IP address on the nodes themselves
    - High port allocated on each node
    - port is open on every node's IP
    - anyone can connect if they can reach the node
    - other pods need to be updated to this port
    
- LoadBalancer: The idea is to control the external load balancer via the Kubernetes command line. When we create this load balancer it automatically creates the clusterIp and NOdePort and then it takes to an external system which could be AWS load balancer or any other.
    - Controls an LB endpoint external to the cluster
    - only available when the infra provider gives you an LB(AWS ELB, etc)
    - Creates Nodeports+Clusterip services, tells LB to send to a NodePorts
      
- ExternalName: This service is more about stuff needing to talk outside the cluster rather than inbound. So we need to create DNS names in the core DNS system so that the cluster can resolve external names
    - Adds CNAME DNS record to CoreDNS only
    - Not used for Pods, but for giving pods a DNS name to use for something outside Kubernetes
    - 
------------------------------------------------------------

**- kubectl expose**: used to create a any of the above four service for existing pods. 

------------------------------------------------------------
**Creating a ClusterIP:**
 
**- kubectl expose for ClusterIP service**: Before exposing we need to have a deployment in our kubernetes, and need to create another pod which will ping this service as cluster IP works internally only reachable within the cluster.

**Syntax:**
    kubectl expose deployment deploymentname --port portonwhichappisrunning
    
    kubectl expose deployment deploymentname --port portonwhichappisrunning --type ClusterIP
    
**Usage:**
    kubectl  expose deployment httpenv --port 8888

    ------------------------------------------------------------
    
**Creating a NodePort Service:**
 
**- kubectl expose for NodePort**: Before exposing we need to have a deployment in our kubernetes

**Syntax:**
    kubectl expose deployment deploymentname --port portonwhichappisrunning --name anyname --type NodePort
    

**Usage:**
    kubectl  expose deployment httpenv --port 8888 --name httpenv-np --type NodePort

**Note** This creates a service named httpenv which we could get by kubectl get services and we will notice a port mapping there. Unlike docker the port on left is container inside port and port on right is the port exposed on the node the cluster is running so we can can access it via localhost:port_on_right. This will be a high port randomly assigned. The NodePort service also creates ClusterIp and Loadbalancer service creates both NodePort and clusterIP.

------------------------------------------------------------

**Creating a LoadBalancer Service:**
 
**- kubectl expose for LoadBalancer**: Before exposing we need to have a deployment in our kubernetes

**Syntax:**
    kubectl expose deployment deploymentname --port portonwhichappisrunning --name anyname --type LoadBalancer
    

**Usage:**
    kubectl  expose deployment httpenv --port 8888 --name httpenv-lb --type LoadBalancer

   
------------------------------------------------------------

## Kubernetes Management techniques
When we were writing kubectl commands either it be get,create,delete  use helper templates called generators. Every resource in kubernetes has a specification or spec. Generators are different for each type of service we are creating either it be deployment,service,pod,job and so on.We can output those templates using --dry-run=client -o yaml. We can look at examples below:
**Examples:**

- **kubectl create deployment sample --image nginx --dry-run=client -o yaml**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: sample
  name: sample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sample
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: sample
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}


```
- **kubectl create job test --image nginx --dry-run=client -o yaml**

```
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: test
spec:
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - image: nginx
        name: test
        resources: {}
      restartPolicy: Never
status: {}


```
- **kubectl expose deployment my-nginx --port 80 --dry-run=client -o yaml**
- For this to work we need to first create a deployment using
  kubectl create deployment my-nginx --image nginx:alpine

```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: my-nginx
  name: my-nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: my-nginx
status:
  loadBalancer: {}

```

## Imperative vs Declaritive
**Imperative:** 

- Focus on how a program operates
- Examples include  kubectl run, kubectl expose, kubectl create deployment, kubectl update and so on.
- Better for learning,dev
- we know the current state and what we need to do i.e create deployment, using expose to expose the service and so on.

**Declaritive:**

- Focus on what a program should accomplish
- kubectl apply -f myresources.yaml
- We don't know the current state and we just want the end result to be (yaml contents)
- Resources can all be in a single yaml file or many files (whole dir)
- Used in production and easiest way to automate i.e iaac

## Three Management approaches defined by Kubernets

- **Imperative commands**: run, expose,scale,edit,create deployment,delete
  
    - Best for learning,dev,personal projects
    - Easy to learn but hardest to manage over time
- **Imperative Objects**: create -f file.yaml, replace -f file.yaml,delete and so on.
  
    - Good for prod of small environments, single file per command
    - stores our changes in git-base yaml files
    - hard to automate
- **Declaritive objects**: apply -f file.yaml or dir, diff
  
    - Best for production
    - Easier to automate
    - Harder to understand and predict changes
      
## Declaritive Kubernetes Yaml
 Declaritive kubernetes is more of a DevOps style and used in production where all we have to do is to create,update,delete resources via yaml. It gives more control and is widely used in production.

**Command**:

**kubeclt apply** : used to apply the services defined in yaml format. We can use single file, whole dir containing yml files and even a url as well.

**Syntax**:
   kubectl apply -f filename.yml
   kubectl apply -f mydirwithyamlfile/
   kubectl apply -f urlpath

**Usage**:
   kubectl apply -f test.yml
   kubectl apply -f yamlfiles/
   kubectl apply -f https://bret.run/pod.yml

## Kuberenetes Configuration via File
- Kubernetes configuration can be done via yaml file or json file.
- We can create a single file per resource or stack multiple resources into a single file using " --- " as separators and each file contains or more manifests. A full description of a resource is called manifest.
- Each manifest describes an api object (deployment, job, secret)
- Each manifest needs four parts( root key:values in the file)
    - **apiVersion**
    - **kind**
    - **metadata**
    - **spec**
      
**NOTE:** You can see the examples of these manifests in this repo name pod.yml for single pod, deployment for only deployment and then combined together via single file named combined.yml.

## Building our own yaml files:
The four mandatory parts for a manifest are defined above and are common for each resource. But how to get more information about these parts as we might notice that the Kubernetes has evolved over time so the API versions and etc. 

- **kind**: we can get a list of resources the cluster supports
     - kubectl api-resources
- **apiVersion**: We can get the api version the cluster supports
     - kubectl api-versions
- **metadata**: only name is required
- **spec**: where we define all the resource specifications. We can dig deep using the resource type. Attributes of the "spec" are specific to the kind. i.e deployment, service etc. The template attribute inside the spec has its own spec and metadata section which is used for the creation of pods because deployment manages pods.
     - we can get all the keys each kind supports
       - kubectl explain services --recursive
       - kubectl explain services.spec
       - kubectl explain servies.spec.type
       - kubectl explain deployment.spec
       - kubectl explain deployment.spec.replicas
       - kubectl explain deployment.spec.template.spec.volumes.nfs.server
         
         ![image](https://github.com/user-attachments/assets/66482dcd-5327-44f7-b429-36c97a1c924b)
 
    **NOTE**: There is always kubernetes.io/docs or search kubernetes api on google to how to buid yaml spec part.
  
**kubectl diff -f pod.yml**: To check the difference once you changed anything in yml file after deployment. Useful if you want to see which changes you made. it check the yaml we have applied before and changes we have made to the current yaml.

**kubectl apply -f pod.yml --dry-run=server**: to check what is changed

## Namespaces in Kubernetes
 
 - Used to organise resources in namespaces
 - its like a virtual cluster inside a cluster
 - Used to group resources in a namespace   i.e namespace for db, namespace for monitoring tools and so on
 - Used to limit access to resources on namespaces i.e two teams have their own isolated environment. we can also limit resources usage like cpu,ram based on namespaces
 - Used to share common resources against different environments i.e test,prod,dev environment could have a same dependency like logging so creating a namespace for logging which can be used by any env.
 - Used to avoid conflicts in deployment of resources . i.e two teams deployment same name resources with different configuration could end up in overwriting the work done by one time
 
 By default when we create resources in kubernetes cluster they are created in a default namespaces which comes along with the kubernetes installation by default along with three other name spaces 
 kube-system, kube-public and kube-node-lease. To check what namespaces we have in our cluster we can use command:
 
  **kubectl get namespaces**

 **IMPORTANT NOTE**
 
 **We can't access most of the resources from other namespace. i.e a configmap in one namespace can't be refrenced in another namespace but we can access service defined in another namespace using servicename.namespace**
 
 ![image](https://github.com/user-attachments/assets/12b47bb0-8954-46b3-8128-6350a720f013)
 
 ![image](https://github.com/user-attachments/assets/7ce451f9-920f-4282-ad0b-4540a6e0df83)
 
![image](https://github.com/user-attachments/assets/04c6baed-c249-48e8-a921-c642b14775cb)

**To check which resources can be namespaced and which can't be namespaced we can using the following commands:**
- kubectl api-resources --namespaced=true
- kubectl api-resources --namespaced=false

 **Creating a Namespace**
  Namespaces can be created via 3 methods:
   - via cli                                      : kubectl create namespace newnamefornamespace
   - via apply -f filename.yml                    : define a yaml file for namespace
   - via apply command on command line            : kubectl apply -f mydeploy.yml --namespace=my-new-namespace
## Volumes in Kubernetes

![image](https://github.com/user-attachments/assets/653a4f84-6610-4c5f-9e8e-d5f57ed7faca)

## Update a container image running in kubernetes cluster
**Command**

kubectl set image deployment deploymentname containername=newimagename

**Usage:**

  kubectl set image deployment myapp mywebapp=httpd:latest
## Ingress Usage for path or sub domain:

![image](https://github.com/user-attachments/assets/21cd8e4b-ff1d-4d27-bcde-7cfbc55b16f4)

## Configuring tls in ingress:

![image](https://github.com/user-attachments/assets/65547083-89c3-418f-b49d-ca96237662c7)

## **Some Important commands**:
- kubectl rollout status deployment deploymentname &emsp;&emsp;&emsp;&emsp;    : To check the status of rollout for a deployment
- kubectl rollout undo deployment deploymentname    &emsp; &emsp;&emsp;&emsp;        : To undo a failed or faulty deployment to previouse deployment

## Helm Charts
- its a bundle of yml file someone put in public or private helm repositories
- they are actually template files created by someone else so to avoid repetitive tasks. i.e deployment of a mongodb, elasticstack or some other application
- helm is also a templating engine. e.g deployment and service configuration of several microservices are same except for few values so we can create a common blueprint and then generate new yaml files by just passing the values that needed to be changed. The blueprint is called template file and the values that needed to be changed can either come from  command line using --set flag or can come from a file named values.yml

![image](https://github.com/user-attachments/assets/c4079413-0fa4-47e7-8c3a-aad360b772b8)

