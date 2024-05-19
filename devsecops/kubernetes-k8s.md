# Kubernetes (k8s)

## What is Kubernetes?

It is on open-source tool for providing cluster deployment and management for containerized application, developed by google.

The name Kubernetes comes from the Greek word meaning "helmsman" or "pilot". The abbreviation k8s is derived from the fact that there are eight characters betweens the letter 'k' and 's'.

### Main features:

* High availability, no downtime, automatic disaster recovery
* Rolling updates without affecting normal business operations
* One-click Rollback to previous version
* Convenient scaling and expansion, providing load balancing
* A complete ecosystem

## Different deployment solution

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Traditional deployment:

Applications run directly on the OS hardware layer, which can lead to inefficient resource allocation. When a bug occurs, a single application can consume most of the resources, causing other applications to malfunction. Additionally, it's important to be able to segregate applications to prevent such issues.

#### Virtualized Deployment:

In s single hardware running multiple virtual machines, each virtual machines is a standalone Operation system, causing waste in resources

#### Container Deployment:

Containers share the host system, functioning as lightweight virtual machines. They offer high resource efficiency, and resources can be allocated as needed.

## When we need K8s?

When your application is running on a single machine, Docker and Docker Compose are sufficient. For applications running on three or four machines, you can manually configure each instance and manage load balancing. However, as the number of instances grows to 10, 100, or even thousands, managing updates, software upgrades, and version rollbacks becomes problematic.

Kubernetes (k8s) provides centralized control of clusters, allowing you to manage applications with ease. You can add instances, update versions, and roll back versions with a single command, simplifying the management of large-scale deployments.

## Kubernetes Cluster Architecture

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Master

The master node, as the control platform, does not require high performance and does not run tasks. Usually, one is sufficient, but multiple master nodes can also be used to increase cluster availability.

### Worker

Worker nodes can be virtual machines or physical computers where tasks are run, requiring good performance; there can be multiple nodes that can be continuously added to expand the cluster; each worker node is managed by a master node.

### POD

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

In k8s, pod is the smallest scheduling unit. A Pod can contain one or more closely related containers that share networking and storage resources and run on the same host. Pods provide an abstraction layer that allows applications to be independent of underlying physical or virtual machine.

Above confuse rite, we can have some example. In example for Node 1 ( or we called host machine 1)

We can have 2 dockers in POD1 listening on POD\_A:80 and POD\_B:8080.

In another POD within the same Node, we can also have POD\_B:80 listening on the same port 80. Each pod have its own IP address.

That means, docker in different ports are able to communicate with other pods through the IP address. E.g. POD\_A:80 can communicate to POD\_B:80.

Now another question rise, yes we can communicate within the NODE happily. But what if we want expose the port to public?

K8S will be using NAT to map the ports to host ports like docker. For example  POD\_A:80 we can map to HOST:80, but POD\_B:80 we cant map to HOST 80 also, we have to map to other ports.

#### QUESTIONS!

Why we have to so troublesome, create multiple Pod in single node?&#x20;

Are we allowed to create a single POD, and we put all the containers inside?

#### Answer:

In K8s, the design concept of Pods is to support the deployment and management of containerized applications, even many Pods can run on a single Node, each pods should contains the container that share the same lifecycle, networking and storage. This design have following advantage:

1. **Resource isolation and management:** Each container within a Pod shares the same resources, allowing **Kubernetes to better manage resource usage, such as CPU and memory**.
2. **Communication between containers:** Containers within the same **Pod can communicate directly using localhost without going through the network stack**, improving communication efficiency.
3. **Flexibility:** Placing related containers in the same Pod makes it easier to manage their deployment, scaling, and adjustment as needed.
4. **Lifecycle management:** Pods are the basic unit of management and scheduling in Kubernetes, so grouping related containers in the same Pod **allows for better management of their lifecycle, such as creation, deletion, and restart.**

## Interview question

1. **Explain the difference between traditional deployment, virtualized deployment, and container deployment.**
2. **When should you use Kubernetes instead of Docker or Docker Compose?**
3. **What are the roles of the master and worker nodes in a Kubernetes cluster?**
4. **Why might you need to create multiple Pods within a single node instead of a single Pod with all containers?**
5. **What is the significance of having multiple master nodes in a Kubernetes cluster? (ownself research)**

## Author

[Ikonw](https://github.com/Ik0nw)
