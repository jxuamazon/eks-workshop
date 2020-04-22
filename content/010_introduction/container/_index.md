---
title: "Containers"
weight: 10
draft: false
tags:
  - beginner
  - docker
---

### Evolution of Virtualization 

#### Virtual Machines (VM)  
- Hypervisor controls how resources on a physical machine is shared
- Type 1 - Bare metal hypervisor
- Type 2 - Hosted hypervisor - on top of eixsting OS

#### Containers ( takes a week to be really good at ) 
Virtualization of runtime environments on top of existing OS

##### Docker container
- Most polular container platform
- Use a "Docker file" to build a "Docker Image" and run it on a "Docker Engine"
- A "Docker Image" contains all the software needed for the application
- Use shared docker images from a "hub"

##### Sigularity container for scientific computing
- More control on the system, not need to ask cluster admins to install anything
- Works great on clusters (take advantage of network fabrics)





