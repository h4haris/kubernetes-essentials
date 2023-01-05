# Kubernetes Essentials
Kubernetes essentials

## Kubernetes Overview

### Intro to Kubernetes
- also known as K8s
- is an open source orchestration tool
- containers + orchestration

#### Problem

- We have often heard the statement : "but it was working on my local machine"
- It happens because the OS, software version, dependencies etc of local machine/test environment is different than the Production

#### Solution
- Containers came to rescue

### Container Overview

#### What is Container?

- as name suggest, they are used to contain any thing
- used to contain softwares, its dependencies and configurations in an isolated place.

#### Why Container?

- `Portability`: to run software/application anywhere without worrying about dependencies
- `Resource Efficiency`: containers do not require separate OS to be run on a system
- `Speed`: can be created, started, stopped and deleted in seconds/minutes
- `Scalability`: we can create multiple parallel containers of same application

#### Virtual Machine vs Containers

- `Virtual machine`: 
    - a complete OS, as well as the application, is included when using virtual machine technology. 
    - A physical device running 2 virtual machines consists of a hypervisor and 2 separate top layer OS.

- `Containers`:
    - For docker (containers) running a single OS, the physical engine runs 2 containerized programs, and all containers use the same OS kernel. 
    - Share only the read-only part of OS among them, which means that the boxes are very lightweight and use fewer resources than virtual machines. 

#### Docker Containers

- Built in 2013 as an open source Docker Engine
- most widely used containerization technology
- has wide support available

### Orchestration

- Orchestration is the coordination and management of multiple computer systems, application and/or services, stringing together multiple tasks in order to execute a larger workflow or process.
- processes can consist of multiple tasks that are automated and can involve multiple systems.
- saves time, increaces efficiency and eliminate redundancies

#### Automation vs Orchestration

- Automation is programming a task to be executed without the need for human intervention.
- Such as if i am building a project, I don't want to build it each and every time manually. Instead, I would like to automate it by creating a CI/CD or some other automation process, to execute the build automatically.
- Setting up one task to run on its own

- Orchestration is the configuration of multiple tasks (some may be automated) into one complete end-to-end process or job.
- Orchestration software also needs to react to events or activities throughout the process and make decisions based on outputs from one automated task to determine and coordinate the next tasks.
- automating many tasks as a process or workflow

#### Orchestration Technologies

1. `Docker Swarm`:
    - east to setup/configure
    - lacks some of advance features required for complex applications
    - from docker

2. `MESOS`:
    - quite difficult to setup
    - supports many advanced features
    - from apache

3. `Kubernetes`:
    - most popular
    - difficult to setup
    - provides alot of options to customize and supports deployments of complex architectures
    - from google
    - supported on all public cloud providers i.e. GCP, AWS, Azure

#### Kubernetes Advantages

- application is highly available as hardware failure do not bring the application down, as we have multiple instances of application running on different nodes
- user traffic is load balanced across various containers
- deploys instances seamlessly within a matter of seconds when demand increases (scalable)
- can do this all with a set of declarative configuration files

## Kubernetes Architecture

### Architecture
- A Kubernetes cluster is a form of Kubernetes deployment architecutre.

- Based on 2 major components: control plane & node part (computer machines)

- Kubernetes (K8s) architecture components include the Kubernetes control plane and the nodes in the cluster also called as master and worker nodes respectively.

#### Node
- Each node could be either a physical or virtual machine and is its own Linux environment.
- Each node also runs pods, which are composed of containers installed in them.
- If a nodes fails and goes down, the application can go down too. To overcome this problem, we have clusters

#### Clusters
- Set of multiple nodes 
- have similar nodes installed in them
- to monitor or operate these clusters/nodes we need a `master node`

#### Master Node
handles the worker nodes

#### Master Node Components

1. `API server`:
    - interacts with human
    - acts as a frontend on Kubernetes, users, command line interfaces
    - management of devices all talk to the api server to interact with kubernetes

2. `Scheduler`:
    - responsible for finding the best suitable node for a pod to run on

3. `Controller manager`:
    - manages various controllers in Kubernetes.

    - `Controllers` 
        - are control loops that continuously watch the state of your cluster, then make or request changes where needed.
        - controls nodes automatically
        - they continuously talk to the kube-apiserver and kube-apiserver receives all information of nodes through Kubelet

4. `Etcd`:
    - a strong key value store database

#### Worker Node
node which does the whole work

#### Worker Node Components

1. `Runtime Engine`:
    - also known as container runtime or container engine
    - is a software component that can run containers on a host OS.
    - In a containerized architecture, container runtimes are responsible for loading container images from a repository, monitoring local system resources, isolating system resources for use of a container, and managing container lifecycle.

2. `Kubelet`:
    - responsible for managing pods and their containers.
    - deals with pods specifications which are defined in YAML or JSON format.
    - it is a service responsible for conveying information to and fro to the control plane service (API Server).
    - interacts with ETCD to read the configuration details
    - conmmunicates with the master node to get commands and work efficiently.

3. `Kube Proxy`:
    - Kubernetes netork proxxy (Kube-poxy) is a daemon running on each node.
    - it basically reflects the services defined in the cluster and manages the rules to load-balance requests to a service's backend pods.

### Namespaces in Kubernetes
- tool to keep kubernetes object separated
- if you are in a wrong space you won't be able to communicate with your object

#### When to use multiple namspaces

- Namespaces are intended for use in environments with many users spread across multiple teams, or projects.
- For clusters with a few to ten users, you should not need to create or think about namespaces.
    - Let say we have a small infrastructure and we have very few services to be deployed in that particular cluster. So not recommended to create namespaces
    - If there is very big infrastructure/environment and you want your services separated from each other, then use namespaces

- Name of resources need to be unique within a namespace, but not across namespaces.


#### Benefits of using namespaces

- `Isolation`: large or growing teams can use namespaces to isolate their projects and microservices from each other.

- `Organization`: small organizations that uses single cluster for multiple environments i.e. dev, stage and production can use namespaces to work on each environment without disturbing the other

- `Permissions`: gets easy to provide RBAC (Role based access control) on each namespace instead of giving same permission on whole cluster.

- `Performance`: 
    - can help improve performance of a given cluster. 
    - if a cluster is separated into multiple namespaces for different projects, the Kubernetes API will have fewer items to search when performing operations. 
    - This can reduce latency and speed overall application performance for each application running on the cluster

- `Resource Control`: 
    - policy-driven resource limits can be set on namespaces by defining resource quotas for CPU or memory utilization. 
    - This can ensure that every project or namespace has the resources it needs to run, and that no one namespace is hogging all available resources.
    - Minimal resources to test environment but higher to Production environment


#### How Pods communicate across namespaces

- Pods are basic unit or services.
- Although namespaces are separate, they can easily communicate with each other
- Kubernetes DNS Service directory can easily locate any service by its name using the expanded form of DNS addressing:

```bash
        ..svc.cluster.local
```

- Simply adding the namespace name to the service name provides access to services in any namespace on the cluster.

#### Default namespaces in Kubernetes

3 namespaces

1. `default`: referenced by default for every Kubernetes command, and where every Kubernetes resource is located by default. Until new namespaces are created, the entire cluster resides in 'default'

2. `kube-system`: used for Kubernetes components and should be avoided

3. `kube-public`: used for public resources. Not recommended for use by users.

```bash
To Create Namespace
===================
kubectl create namespace demo-namespace

To Get Namespaces
=================
kubectl get namespace
```

## Kubernetes Concepts
### Pods

#### Docker Image
- a file to execute code in docker container
- has set of commands in it
- stored in Repository Either Docker hub, or any other cloud repository i.e. `ECR` in `AWS` which is elastic container repository

#### Kubernetes Cluster
- set of nodes that run containerized applications.
- could be single or multi-node setup

#### Pod
- single instance of application
- smallest object that we can create in Kubernetes
- have auto scaling feature
    - Pod could be auto scale as per need of application where our traffic is very high
    - could be similar in multiple nodes as well as could be different depending on the requirement of our application.



- Kubernetes cluster is setup here where we have multiple nodes stored
- Our ultimate goal is to install/run containerized application on the kubernetes cluster.
- So containerized application are not directly installed on the nodes, instead stored in a encapsulated environment/instance of application called as `pod`


### Replica Controls
### HPA
### Health Probes
### YAML