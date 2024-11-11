# ***DOCKER***
- [***DOCKER***](#docker)
  - [Virtualisation vs. Containerisation](#virtualisation-vs-containerisation)
  - [Virtualisationsds](#virtualisationsds)
    - [Benefits](#benefits)
  - [Containerisation](#containerisation)
    - [Benefits](#benefits-1)
  - [Key Differences](#key-differences)
  - [***Differences between Virtualization and Containerization***](#differences-between-virtualization-and-containerization)
    - [***What is Included in a Container vs Virtual Machine***](#what-is-included-in-a-container-vs-virtual-machine)
    - [***Benefits of Each, Especially Virtual Machine Over Traditional Architecture***](#benefits-of-each-especially-virtual-machine-over-traditional-architecture)
  - [***Microservices***](#microservices)
  - [***Docker***](#docker-1)


## Virtualisation vs. Containerisation
Virtualisation and containerisation are both ways to create isolated environments for running applications, but they differ in their approach and structure.

## Virtualisationsds
**How It Works**
* In virtualisation, a physical machine runs a hypervisor, which is software that creates multiple virtual machines (VMs). 
* Each VM has its own operating system and resources.

**What's in a VM?**
* Each virtual machine includes a full OS, libraries, dependencies, and the application itself.

### Benefits
**Isolation**: VMs are fully isolated, which increases security and reliability.
**Resource Allocation**: You can allocate specific CPU, memory, and storage resources to each VM.
**Compatibility**: Each VM can run different OSes, making it flexible for various applications.

## Containerisation
**How It Works**
* Containers package up just the application code and dependencies but share the host OS kernel. 
* Instead of a hypervisor, containers use a container runtime like Docker to manage isolation.

**What's in a Container?**
* A container usually includes only the application, necessary libraries, and dependencies—no separate OS.

### Benefits
* **Lightweight**: Containers don’t need an entire OS, making them smaller and faster to start up.
* **Consistency**: Containers ensure that applications run the same, regardless of where they are deployed.
* **Scalability**: Containers are more efficient in terms of resource use, so you can run more containers on the same hardware compared to VMs.

## Key Differences
* **Size**: Containers are much lighter than VMs as they share the host OS kernel.
* **Startup Time**: Containers start almost instantly, whereas VMs can take several minutes.
* **Isolation**: VMs provide stronger isolation since each VM has its own OS, while containers share the host OS.

<br>


## ***Differences between Virtualization and Containerization***
| Feature               | Virtualization                                       | Containerization                                 |
|-----------------------|------------------------------------------------------|--------------------------------------------------|
| **Isolation**         | Each VM has a full OS and is isolated from others    | Containers share the host OS but have isolated environments |
| **Efficiency**        | Higher resource usage due to multiple OS instances   | More efficient, lightweight; shares OS kernel    |
| **Startup Speed**     | Slower to start, as it boots an OS                   | Starts quickly, as it runs from host OS          |
| **Portability**       | Typically less portable due to OS dependencies       | Highly portable across different environments    |
| **Use Cases**         | Useful for running different OS environments         | Ideal for lightweight applications and microservices |

---

### ***What is Included in a Container vs Virtual Machine***

| Aspect                     | Container                                      | Virtual Machine                      |
|----------------------------|------------------------------------------------|--------------------------------------|
| **OS Kernel**              | Shared with host                               | Separate OS per VM                   |
| **Application Code & Binaries** | Included                                | Included                             |
| **Libraries & Dependencies**   | Included                                   | Included                             |
| **Hypervisor**             | Not required                                   | Required                             |
| **Overhead**               | Minimal                                        | Higher                               |

---

### ***Benefits of Each, Especially Virtual Machine Over Traditional Architecture***

| Benefit                    | Virtual Machine                           | Container                                |
|----------------------------|------------------------------------------|------------------------------------------|
| **Isolation**              | Strong isolation due to separate OS       | Lightweight, less strict isolation       | 
| **Resource Allocation**    | Dedicated resources per VM               | Shares resources more efficiently        |

- **Traditional Architecture**  
  VMs offer better security, hardware emulation, and are suitable for monolithic applications on traditional infrastructure. Containers, by contrast, excel in agility, speed, and are more suited for cloud-native or microservices-based architectures.

---

## ***Microservices***

- **What Are They?**  
  Microservices are a design pattern for building an application as a collection of small, loosely coupled services, each responsible for a specific functionality.

- **How Are They Made Possible?**  
  Enabled by containerization and orchestration tools like Docker and Kubernetes, which allow isolated, independently deployable components.

- **Benefits**  
  - **Scalability:** Each service scales independently.
  - **Flexibility:** Easier to deploy, update, and maintain.
  - **Resilience:** Failure in one service doesn’t bring down the entire system.

---

## ***Docker***

- **What Is It?**  
  Docker is an open-source platform designed to simplify application deployment using containers.

- **Alternatives**  
  - Podman
  - LXC (Linux Containers)
  - rkt (Rocket)
  - OpenVZ

- **How It Works (Docker Architecture/API)**  
  Docker uses a client-server architecture, with the Docker Client communicating with the Docker Daemon to manage containers. The Daemon handles container lifecycle, resource allocation, and API requests.

- **Success Story Using Docker**  
  Companies like Netflix, Spotify, and PayPal use Docker to scale microservices, reduce deployment times, and increase application efficiency.

---
