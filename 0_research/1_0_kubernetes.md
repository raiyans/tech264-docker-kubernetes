# Learning Kubernetes

## Kubernetes Reasearch

### Why is Kubernetes Needed?

Kubernetes is needed to manage containerized applications in a clustered environment. It automates the deployment, scaling, and operation of application containers across clusters of hosts. This orchestration simplifies the complexities of managing microservices and distributed systems, ensuring applications run reliably and efficiently.

### Benefits of Kubernetes

* **Rollback Changes**
* **Load-Balancing**
* **Open-Source**
* **Self-Healing**
* **Scalability**: Automatically scales applications up or down based on demand.
* **High Availability**: Provides self-healing mechanisms like auto-restarting, rescheduling, and replicating containers.
* **Portability**: Works across various environments—from on-premises to public clouds—ensuring consistent operations.
* **Resource Optimization**: Efficiently utilizes infrastructure resources by managing container placement and resource allocation.
* **Extensibility**: Offers a modular architecture with a rich ecosystem of plugins and integrations.

### Success Stories

* **Spotify**: Adopted Kubernetes to manage its microservices architecture, improving deployment speed and resource utilization.
* **The New York Times**: Migrated to Kubernetes to streamline their content delivery, resulting in faster deployment times and improved scalability.
* **CERN**: Uses Kubernetes to handle vast amounts of data processing for particle physics research, enhancing computational efficiency.
* **Airbnb**: Implemented Kubernetes to better manage scaling demands, leading to more efficient infrastructure management.

### Kubernetes Architecture

Kubernetes architecture is based on a master-worker model designed for orchestrating containerized applications across clusters of hosts. Here’s a concise breakdown:

* **Control Plane (Master Node)**: The control plane is responsible for managing the entire Kubernetes cluster, making scheduling decisions, and orchestrating workloads. It includes:

  * API Server: The main entry point to the Kubernetes cluster, handling all REST requests and exposing the Kubernetes API.
  * etcd: A key-value store that stores all cluster data, including configuration and state information.
  * Controller Manager: Monitors the cluster state and makes adjustments (e.g., scaling and replica management) to meet the desired state.
  * Scheduler: Assigns newly created pods to nodes based on resource availability, constraints, and policies.

* Worker Nodes: These nodes run the applications in pods and report to the control plane. Each worker node includes:

  * Kubelet: The main agent on each worker node that receives instructions from the API server and manages pod lifecycle.
  * Kube-proxy: Manages network rules on each node to enable communication within and outside the cluster.
  * Container Runtime: The software (e.g., Docker, containerd) responsible for running containers.
  
* Pods: The smallest deployable units in Kubernetes, representing a single instance of a running process. Pods can contain one or more tightly coupled containers.

![alt text](/images/1_kubernetes.png)

### What is a Cluster?

* Made up of at least 1 **Master Node** and 1 **Worker Node**.

A cluster in Kubernetes is a group of machines (nodes) that work together to run and manage containerized applications. It includes a control plane for orchestration and worker nodes for running workloads.

### Master vs. Worker Nodes

* **Master Nodes**: Manage the cluster, making scheduling and orchestration decisions, and maintain the desired state. They host components like the API server, controller manager, scheduler, and etcd.

* **Worker Nodes**: Run application workloads in containers within pods, managed by the control plane. Each worker has components like the kubelet, kube-proxy, and container runtime.

### Pros and Cons of Using Managed Service vs. Launching Your Own

* **Managed Service (e.g., GKE, EKS, AKS)**
  * **Pros**: Reduces operational overhead, automated updates and scaling, easy integrations, and high availability by default.
  * **Cons**: Higher costs, limited control over infrastructure, and potential vendor lock-in.

* **Launching Your Own (Self-managed)**
  * **Pros**: Full control over configuration, infrastructure choice, and customization; potentially lower costs at scale.
  * **Cons**: Requires significant operational knowledge, higher maintenance, and manual scaling and updates.

### Control Plane vs. Data Plane
* Control Plane: Manages the Kubernetes cluster, making decisions about scheduling, scaling, and maintaining the desired state.
* Data Plane: Executes the workloads (pods) on worker nodes, handling the actual application and network traffic.

### Kubernetes Objects
Kubernetes objects are persistent entities in the Kubernetes system that represent the desired state of resources and workloads in the cluster. Common ones include:

* **Pods**: The smallest deployable unit, representing one or more containers.
* **Deployments**: Manages the deployment and scaling of pods, ensuring the desired number of replicas.
* **ReplicaSets**: Maintains a specified number of identical pod replicas for availability and resilience.
* **Services**: Exposes a set of pods as a network service, allowing external or internal access.
* **ConfigMaps and Secrets**: Store configuration data and sensitive information, respectively, to be used by pods.

### Namespaces

* If an application has more alot of **Kubernetes Objects** associated. then you can group them in a namespace. if not named then all stored in default

### What Does it Mean a Pod is "Ephemeral"?
A pod is ephemeral because it is designed to be temporary and can be terminated, rescheduled, or replaced by Kubernetes at any time. Pods are not persistent and may be recreated on other nodes, leading to potential loss of state.

### How to Mitigate Security Concerns with Containers
* **Use Maintained Images**: Base images regularly updated by trusted sources reduce vulnerabilities.
* **Implement Image Scanning**: Use tools to scan images for vulnerabilities before deployment.
* **Limit Container Privileges**: Run containers with the least privileges necessary.
* **Use Network Policies**: Restrict pod-to-pod communication to only what’s required.
* **Regularly Update Dependencies**: Keep containers and dependencies updated.
* **Enable Runtime Security**: Use monitoring tools to detect unusual container behavior.

### Maintained Images

**What Are They**: Pre-built container images managed and updated by trusted organizations or maintainers (e.g., official Docker or vendor-provided images).

**Pros**:
* Regular updates and security patches.
* Pre-configured with best practices.
* Reduced maintenance burden.

**Cons**:
* Limited customization and control.
* May include unnecessary components, increasing size.
* Dependency on maintainer updates and patch timing.