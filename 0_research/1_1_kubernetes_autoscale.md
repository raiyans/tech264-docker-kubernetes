# Research Types of Autoscaling with K8s

- [Research Types of Autoscaling with K8s](#research-types-of-autoscaling-with-k8s)
  - [What is Autoscaling?](#what-is-autoscaling)
  - [Types of Autoscaling in Kubernetes](#types-of-autoscaling-in-kubernetes)
    - [1. Horizontal Pod Autoscaling (HPA)](#1-horizontal-pod-autoscaling-hpa)
    - [2. Vertical Pod Autoscaling (VPA)](#2-vertical-pod-autoscaling-vpa)
    - [3. Cluster Autoscaling](#3-cluster-autoscaling)
  - [Difference Between Vertical and Horizontal](#difference-between-vertical-and-horizontal)
  - [Benefits of Autoscaling](#benefits-of-autoscaling)
- [The key components of Kubernetes autoscaling](#the-key-components-of-kubernetes-autoscaling)
  - [1. Resource Request](#1-resource-request)
    - [Configuring the resource request](#configuring-the-resource-request)
    - [Pod Disruption Budget](#pod-disruption-budget)
  - [2. Pod Disruption Budget](#2-pod-disruption-budget)
  - [3. Horizontal Pod Autoscaler](#3-horizontal-pod-autoscaler)
  - [4. Cluster Autoscaler](#4-cluster-autoscaler)
    - [Adding a node](#adding-a-node)
    - [Removing a node](#removing-a-node)
- [Use Horizontal Pod Autoscaler (HPA) to scale the app](#use-horizontal-pod-autoscaler-hpa-to-scale-the-app)
  - [Steps to Implement HPA for Node.js Application](#steps-to-implement-hpa-for-nodejs-application)
  - [Install Apache](#install-apache)
  - [1. Define HPA for Node.js App](#1-define-hpa-for-nodejs-app)
  - [2. Create HPA](#2-create-hpa)
  - [3. Verify HPA Setup](#3-verify-hpa-setup)
  - [4. Load Test with Apache Bench (ab)](#4-load-test-with-apache-bench-ab)
  - [Install Metrics Server](#install-metrics-server)
  - [Steps to Fix the Metrics Server](#steps-to-fix-the-metrics-server)
- [Delete \& Create](#delete--create)
  - [Delete all at once](#delete-all-at-once)
  - [Delete MongoDB Pod/Deployment and Re-create:](#delete-mongodb-poddeployment-and-re-create)
    - [Deletion Commands](#deletion-commands)
    - [Creation Commands](#creation-commands)
    - [Check They're There](#check-theyre-there)


## What is Autoscaling?
* Autoscaling in Kubernetes is a way to **automatically adjust** the **number of running pods** or the **resources allocated to them** based on the **current demand**. 
* This helps ensure that your application can handle varying loads efficiently without manual intervention.

<br>

## Types of Autoscaling in Kubernetes

### 1. Horizontal Pod Autoscaling (HPA)
* **What it does**: HPA automatically adjusts the number of pod replicas in a deployment, replica set, or stateful set based on observed CPU utilisation, memory usage, or custom metrics.
* **How it works**: The HPA controller periodically checks the metrics and scales the number of pods up or down to match the desired target utilisation.
* **Use case**: Ideal for applications with fluctuating workloads, such as web servers or APIs that experience varying traffic.

### 2. Vertical Pod Autoscaling (VPA)
* **What it does**: VPA automatically adjusts the CPU and memory requests and limits for containers in a pod.
* **How it works**: VPA monitors the resource usage of pods and updates their resource requests and limits to better match the actual usage.
* **Use case**: Useful for applications where the resource requirements change over time, such as batch processing jobs or data analytics workloads.

### 3. Cluster Autoscaling
* **What it does**: Cluster autoscaling adjusts the number of nodes in a Kubernetes cluster based on the resource requirements of the pods.
* **How it works**: When there are not enough resources to schedule new pods, the cluster autoscaler adds more nodes. 
  * Conversely, it removes nodes when they are underutilised.
* **Use case**: Helps manage the overall capacity of the cluster, ensuring that there are enough resources to run all scheduled pods efficiently.

![alt text](/images/auto-diagram.png)

<br>

## Difference Between Vertical and Horizontal

| Feature                     | Horizontal Pod Autoscaling (HPA)                                      | Vertical Pod Autoscaling (VPA)                                      |
|-----------------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------|
| **Scaling Direction**       | Scales the number of pods horizontally (increases or decreases the number of pod replicas) | Scales the resources allocated to each pod vertically (adjusts CPU and memory requests and limits) |
| **Resource Management**     | Focuses on distributing the load across multiple pods                | Focuses on optimizing the resource allocation for individual pods   |
| **Implementation**          | Requires setting up metrics to monitor and define target utilization thresholds | Requires setting up policies to monitor resource usage and adjust resource requests and limits |
| **Use Case**                | Ideal for applications with fluctuating workloads, such as web servers or APIs that experience varying traffic | Useful for applications where the resource requirements change over time, such as batch processing jobs or data analytics workloads |
| **Example**                 | Automatically adds more pod replicas to handle increased traffic for a web application | Increases memory limits for a data processing job to ensure it completes successfully |

<br>

## Benefits of Autoscaling
* **Efficiency**: Automatically adjusts resources to match demand, reducing costs and improving performance.
* **Reliability**: Ensures that applications remain responsive under varying loads.
* **Scalability**: Easily handles growth in application usage without manual intervention.

<br>

# The key components of Kubernetes autoscaling
* With this feature, you always have enough resources for the workload, and when a node becomes unhealthy it gets replaced without affecting the workload. 
* You need to provide the scheduler with information about your Pods, so it can make the right decisions when scheduling them.

The following components are needed to truly benefit from the autoscaling feature of Kubernetes;

1. Resource Request
2. Pod Disruption Budget
3. Horizontal Pod Autoscaler
4. Cluster Autoscaler

![alt text](/images/autoscaling.png)

Source: https://timdepater.com/articles/kubernetes-autoscaling-components/

## 1. Resource Request
* When you configure a Pod, you specify how much of each resource it needs. 
* The most common resources to specify are CPU and memory, but there are others.

Per Pod you can specify;
* The amount of CPU & memory you expect this Pod needs; the request.
* The amount of CPU & memory you’re allowing the Pod to use; the limit.
* The scheduler takes the resource request into account when determining which node has the resources available to run this Pod. 
* When there is not a node available that would fit the Pod’s resource request, the Pod goes to the Pending state.
* The Cluster Autoscaler will notice a Pod is pending because of a lack of resources and acts upon it by adding a new node.

<br>

### Configuring the resource request
* The resource request is configured per Pod like this;

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "128Mi"
```

To come up with sane values for CPU & memory you can take the following into account;
* `kubectl top pods -A` shows the actual CPU & memory usage of all Pods. 
  * Be aware that this is a snapshot of that moment, it’s better to gather this information from a monitoring system that can show the trend over a longer period.
* **CPU** is a resource that can overbooked, if the actual usage is higher than defined in the resource request then this could result in performance issues but the Pod wouldn’t get evicted because of it.
* **Memory** can’t be overbooked and is reserved based on the resources request. 
  * When a Pod is using more memory than configured and the node runs out of memory the scheduler could evict this Pod.

### Pod Disruption Budget


<br>

## 2. Pod Disruption Budget
* Pod disruption budgets allow you to configure the number of Pods that can be down simultaneously from voluntary disruptions. 
* Voluntary disruptions are mostly triggered by the application owner or cluster administrator. 
* This happens for example when a deployment is changed or a node is drained. 
* The scheduler makes sure that when it’s evicting Pods, it keeps enough Pods running from the same deployment, statefulset or other controllers to don’t exceed the Pod disruption budget.

> The cluster autoscaler is performing cluster administrator actions like draining a node to scale the cluster down. That’s why it’s important to configure these correctly when you want the cluster to autoscale and auto-heal.

Example of a Pod Disruption Budget that allows for 1 Pod to be unavailable at the same time.
```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
  name: myapp
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      app: myapp
```

<br>

## 3. Horizontal Pod Autoscaler
* With a Horizontal Pod Autoscaler, you specify which metrics decide if the number of replicas should scale up or down. 
* You can use per-Pod resource metrics like CPU and memory or custom metrics like the number of requests/second, the Pod is receiving.

Resource metrics can be defined as utilisation value, e.g.;

```yaml
metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 90
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 90
```

* When you define the metric as utilisation value it will be calculated as the percentage of the configured resource request.

<br>

## 4. Cluster Autoscaler
* The Cluster Autoscaler is the component that adjusts the size of the node pool so that all Pods have a place to run and there are no unneeded nodes.
* On most public cloud providers it’s part of the control plane which is managed by the provider. 
  * For AWS that’s not the case, you need to deploy it yourself.

### Adding a node
* The Cluster Autoscaler will monitor the Pods and decide to add a node when a Pod needs to be scheduled and there aren’t sufficient resources for the resource request of that Pod.

This works as follows;

1. A new Pod is created
2. The scheduler reads the resource request of the Pod and decides if there are enough resources on one of the nodes.
3. If there are, the Pod is assigned to the node.
4. If there aren’t, the Pod is set to the Pending state and can’t start.
5. The Cluster Autoscaler will detect a Pod is not able to schedule due to a lack of resources.
6. The Cluster Autoscaler will determine if the Pod could be scheduled when a new node is added (it could be due to (anti-) affinity rules that the Pod still can’t schedule on the newly created node).
7. If so, the Cluster Autoscaler will add a new node to the cluster.
8. The scheduler will detect the new node and schedule the Pod on the new node.

> Note: the scheduler is not capable of moving Pods to different nodes to make room for the new Pod. This can sometimes lead to inefficient use of resources.

<br>

### Removing a node
* The Cluster autoscaler will decide to remove a node when it has low utilisation and all of its important Pods can be moved to other nodes. 
* There are a few reasons which prevent a Pod from being moved to a different node. 
* To move a Pod it needs to be evicted and a new one needs to be started on a different node.

Reasons why a Pod can’t be moved;
* The Pod has a restrictive Pod Disruption Budget.
* The Pod is part of the kube-system namespace and doesn’t have a Pod Disruption Budget, or it’s too restrictive.
* The Pod isn’t backed by a controller object (so not created by deployment, replica set, job, statefulset, etc.).
* The Pod has local storage and doesn’t have the safe-to-evict annotation.
* The Pod can’t be moved due to various constraints (lack of resources, non-matching node selectors or (anti-) affinity, safe-to-evict annotation set to false, etc.)

> The logs of the Cluster Autoscaler can tell you the actual reason, but when the Cluster Autoscaler is managed by the cloud provider you don’t always have access to that log.

<br>

# Use Horizontal Pod Autoscaler (HPA) to scale the app
Task:
* Scale only the app (2 minimum, 10 maximum replicas).
* Test your scaler works by load testing.
* You could use Apache Bench (ab) for load testing.

<br>

## Steps to Implement HPA for Node.js Application
* Set up a Horizontal Pod Autoscaler (HPA) for scaling the Node.js app based on CPU utilisation.
* The HPA will automatically adjust the number of Pod replicas according to the load on the application.

## Install Apache
Source: https://www.apachelounge.com/download/

![alt text](./kube-images/17.png)

* create an alias for ab (alias ab=~/.apache/bin/ab.exe)
* `nano .bashrc`
* Insert: alias ab='~/.apache/bin/ab.exe'
* Ctrl+S, Ctrl+X


<br> 

## 1. Define HPA for Node.js App
* Ensure that your [nodejs-deploy.yml](../k8s-yaml-definitions/local-nginx-deploy/nodejs-deploy.yml) YAML file requests CPU resources. 
* This allows Kubernetes to monitor CPU usage and autoscale based on the defined limits.

```yaml
        resources:
          requests:
            cpu: "100m"     # Request 100 millicores (0.1 CPU)
          limits:
            cpu: "500m"     # Limit the app to 500 millicores (0.5 CPU)
```

* Apply this updated Deployment
  * `kubectl apply -f nodejs-deploy.yml`

<br> 

## 2. Create HPA
* Create an HPA that will scale the number of replicas between 2 and 10 based on CPU utilisation (for example, scaling up when CPU usage goes above 50%).

```yaml
# hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nodejs-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nodejs-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50  # Target 50% CPU utilization
```

* Apply the HPA configuration:
  * `kubectl apply -f hpa.yml`

<br>

## 3. Verify HPA Setup
* Check the status of the HPA to confirm it’s properly set up and monitoring the Node.js deployment.
* You should see output that lists the current and target CPU utilization, along with the desired replica count based on current load.
  * `kubectl get hpa`

![alt text](./kube-images/14.png)

<br>

## 4. Load Test with Apache Bench (ab)
* To simulate load on your application and test if the HPA scales the Pods, use Apache Bench (ab).
  * `ab -n 10000 -c 100 http://<Node.js-Service-External-IP>:<port>/`

* `-n 10000`: Total number of requests to send.
* `-c 100`: Number of concurrent requests.
* Replace `<Node.js-Service-External-IP>` and `<port>` with the external IP and port of your Node.js Service (you can get this by running kubectl get svc).

You will need:
* [nodejs-service.yml](../k8s-yaml-definitions/local-nginx-deploy/nodejs-service.yml) external IP. 
* Port number with the external IP. 

How to get the external IP:
* nano into the [](../k8s-yaml-definitions/local-nginx-deploy/nodejs-service.yml) yaml file and change "NodePort" to "LoadBalancer".

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: sparta-app-svc
  namespace: default
spec:
  ports:
  - nodePort: 30003   # The port on each node where the service is exposed
    port: 80    # The port that the service listens on.
    targetPort: 3000    # The port on the pod that the service forwards traffic to.
  selector:
    app: sparta-app # Label to match service to deployment
  type: LoadBalancer # used to be NodePort, changed for external IP for hpa
```

* Apply and update these changes.
  * `kubectl apply -f nodejs-service.yml`
* Check this has refreshed.
  * `kubectl get services`

![alt text](./kube-images/15.png)

<br>

* To simulate load on your application and test if the HPA scales the Pods, use Apache Bench (ab).
  * `ab -n 20000 -c 200 http://localhost:30003/`

* To view if pods have been made:
  * `kubectl get pods`

![alt text](./kube-images/16.png)

<br>

## Install Metrics Server
1. Install Metrics Server
* This command installs the Metrics Server in your Kubernetes cluster. 
* The Metrics Server is responsible for collecting resource usage data (CPU and memory) from the nodes and pods in your cluster.

```yaml
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

2. Check Metrics Server Logs.
* This command retrieves the logs from the Metrics Server deployment in the kube-system namespace. 
* It helps you diagnose any issues with the Metrics Server by showing its log output.

```yaml
kubectl logs -n kube-system deployment/metrics-server
```

3. List Metrics Server Pods.
* This command lists all the pods in the kube-system namespace that have the label k8s-app=metrics-server. 
* It helps you verify that the Metrics Server pods are running.

```yaml
kubectl get pods -n kube-system -l k8s-app=metrics-server
```

<br>

## Steps to Fix the Metrics Server
1. Check Metrics Server Logs
   * Review the logs for the Metrics Server to identify any errors:
```yaml
kubectl -n kube-system logs deployment/metrics-server
```

2. Patch the Metrics Server Deployment
   * Since the logs previously indicated a TLS certificate validation error, let's ensure the Metrics Server is configured to bypass this validation. 
   * You can patch the deployment to add the --kubelet-insecure-tls argument:

```yaml
kubectl patch deployment metrics-server -n kube-system --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/containers/0/args/-",
    "value": "--kubelet-insecure-tls"
  }
]'
```

3. Verify the Metrics Server Deployment
   * After patching, check the status of the Metrics Server deployment again:
```yaml
kubectl get deployment -n kube-system metrics-server
```

4. Check Metrics Availability
   * Once the Metrics Server is running, verify that metrics are available:
```yaml
kubectl top nodes
kubectl top pods
```

5. Monitor HPA
   * After ensuring the Metrics Server is running and metrics are available, monitor the HPA to see if it starts reporting CPU metrics correctly:
```yaml
kubectl get hpa
```

<br>

# Delete & Create

## Delete all at once
* `kubectl delete /local-nginx-deploy`
* `kubectl apply /local-nginx-deploy`

## Delete MongoDB Pod/Deployment and Re-create:
* `kubectl delete deployment mongo`
* `kubectl apply -f mongodb-deploy.yml`

### Deletion Commands
* `kubectl delete service mongodb-svc`
* `kubectl delete service sparta-app-svc` 
* `kubectl delete deployment mongodb-deployment`
* `kubectl delete deployment sparta-app-deployment`
* `kubectl delete pvc mongodb-pvc`
* `kubectl delete pv mongodb-pv`

### Creation Commands
* `kubectl create -f mongodb-deploy.yml`
* `kubectl create -f mongodb-service.yml`
* `kubectl create -f nodejs-deploy.yml`
* `kubectl create -f nodejs-service.yml`
* `kubectl apply -f hpa.yml`

### Check They're There
* `kubectl get all`
* `kubectl get pv`
* `kubectl get pvc`