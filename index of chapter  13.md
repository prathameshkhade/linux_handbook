# Chapter 13: Virtualization and Containers

## 13.1 Virtualization

### 13.1.1 Understanding the basics:
- `What is virtualization:`
Learn the core concept of virtualization, where a single physical machine can host multiple isolated virtual machines, each with its own operating system. 
- `KVM in Linux:`
Understand that KVM is the native virtualization technology built into the Linux kernel, allowing it to act as a hypervisor. 
- `Hypervisor types:`
Differentiate between Type 1 (bare-metal) and Type 2 (hosted) hypervisors, with KVM typically being categorized as a Type 2. 

### 13.1.2 Setting up a virtual environment:
- `Virtual Machine Monitor (VMM):`
Install and configure a virtual machine management tool like "virt-manager" on your Linux system to interact with KVM. 
- `Creating a VM`:
Use the VMM to create a new virtual machine, specifying details like operating system, RAM allocation, CPU cores, storage, and network interfaces.


### 13.1.3 Managing virtual machines:
- `Starting and stopping VMs:`
Learn how to start, stop, restart, and suspend virtual machines using commands like virsh. 
- `Guest OS installation:`
Install a guest operating system (like Ubuntu, CentOS, etc.) within your virtual machine. 
- `Network configuration:`
Set up network adapters for your VMs, assigning IP addresses and connecting them to the host network or dedicated virtual networks. 

### 13.1.4 Advanced virtualization concepts:
- `Live migration:`
Migrate a running virtual machine from one physical host to another without downtime. 
- `Snapshots`:
Create snapshots of VM states to revert to a previous configuration if needed. 
- `Storage management`:
Explore different storage options for VMs, including disk images, shared storage, and storage pools. 
- `Security considerations`:
Learn best practices for securing VMs, including network isolation, access controls, and security policies. 

### 13.1.5 Tools and commands:
- `virt-manager`: Graphical user interface for managing KVM VMs 
- `virsh`: Command-line tool for interacting with virtual machines 
- `qemu`: Emulator that can be used in conjunction with KVM for additional virtualization features 

### 13.1.6 To become a "Hero" in Linux virtualization you should be able to:
- Efficiently manage a complex environment with multiple VMs, including different operating systems.
- Troubleshoot VM issues related to networking, storage, and performance.
- Optimize VM configurations for resource utilization and performance.
- Automate VM management tasks using scripting languages. 

## 13.2 Containers with Docker & Podman
■ Docker installation, container lifecycle management

### 13.2.1 Foundational concepts:
- `Understanding Containers:`
Learn what containers are, their benefits (isolation, portability), and how they differ from virtual machines. 
- `Dockerfile Basics`:
Explore the syntax of a Dockerfile, including instructions like FROM, RUN, COPY, EXPOSE to build container images.
- `OCI Standard`:
Understand the Open Container Initiative (OCI) standard which both Docker and Podman adhere to, allowing image compatibility. 

## 13.2.2 Docker Fundamentals
- `Installing Docker:`
Set up Docker on your system (ubunut & redhat) and learn basic commands like docker pull, docker run, docker ps, docker build, docker images, etc. 
- `Building a Simple Container`:
Create a Dockerfile for a basic application (e.g., a web server) and build your first container image. 
- `Managing Containers`:
Learn how to start, stop, restart, and delete containers using Docker commands. 
- `Networking`:
Explore container networking options like bridge mode, host mode, and how to link containers together. 
- `Volumes and Data Persistence`:
Understand how to mount volumes to persist data within a container. 

### 13.2.3 Podman Basics
- `Installing Podman:` Install Podman on your system (ubuntu and redhat) and compare its command structure to Docker. 
- `Running Containers with Podman`: Use podman run to execute containers using the same Docker images. 
- `Key Differences`: Focus on the major distinction between Docker (daemon-based) and Podman (daemonless) and how it affects container management. 
- `Rootless Containers`: Explore the concept of running containers as a non-root user with Podman for enhanced security. 

### 13.2.4 Advanced Topics:
- `Docker Compose:`
Learn how to define multi-container applications with Docker Compose files for easier deployment and management. 
- `Image Optimization:`
Explore techniques to create smaller, more efficient container images. 
- `Container Registries:`
Understand how to push and pull images from public registries like Docker Hub. 
- `Kubernetes Integration:`
Explore how to deploy containerized applications on Kubernetes using Docker images and Podman.

### 13.2.5 Important Considerations:
- `Choosing Between Docker & Podman:`
While Docker might be more widely used, Podman can be beneficial for scenarios where security and a daemonless architecture are priorities. 
- `Practice & Experimentation:`
The best way to master containerization is to build various containerized applications with different levels of complexity. 


## 13.3 Kubernetes Basics
- `Pods:`
The smallest unit in Kubernetes, representing a group of containers that are tightly coupled and run on the same node. 
- `Deployments:`
A higher-level abstraction that manages the lifecycle of a set of pods, including rolling updates and rollbacks. 
- `Services:`
An abstraction that provides a stable network endpoint for accessing a group of pods, often used for load balancing. 
- `ReplicaSets:`
Ensures a specific number of pod replicas are always running, automatically replacing any that fail. 
- `Nodes:`
Physical or virtual machines within the cluster where pods are scheduled to run. 
- `Control Plane:`
The master component of Kubernetes responsible for managing the cluster state, including the API server, scheduler, and controller manager. 
- `kubectl:`
The command-line tool used to interact with a Kubernetes cluster to deploy applications, manage resources, and view logs. 
