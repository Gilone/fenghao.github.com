---
title: "Summaries of readings in virtualization course"
layout: post
date: 2022-03-03 09:45
image: https://media.nature.com/lw800/magazine-assets/d41586-022-01878-7/d41586-022-01878-7_23197830.jpg
headerImage: True
tag:
- Tech
category: blog
projects: false
author: Hao Feng
description:
---

### A Comparison of Software and Hardware Techniques for x86 Virtualization

With the new generation of x86 architectures released in 2005, the implementation of a trap-and-emulate VMM that executes guests directly virtualization on x86 is made possible with hardware support (Intel VT-x and AMD V), but before this, to virtualize unmodified x86 operating systems, a popular method is dynamic binary translation with a pure software implementation.  

This paper introduces the technology used by VMware Workstation’s software VMM and a new type of VMM that requires hardware support under the x86 architecture, and compares the performance of a software and a hardware VMM through experiments. However, the hardware virtualization extension rarely improves performance, and the main reason is that the hardware has no support for MMU virtualization, and it cannot coexist with existing software technologies for MMU virtualization, then the paper discusses the prospect of future techniques for addressing the MMU virtualization problem to improve the proformance of hardware-assisted virtualization.

### Performance Evaluation of Intel EPT Hardware Assist (Three Pages)

Intel's second-generation hardware support EPT provides a hardware-based MMU virtualization solution. This paper compares software-based MMV virtualization technology, the shadow paging, and the EPT, and found that the EPT can improve performance.  

The first three pages of this paper describes how the shadow paging works by maintaining a direct mapping of LPN->MPN with VMM, and EPT works by maintaining LPN->PPN and PPN->MPN mappings with hardware support.

### Memory Resource Management in VMware ESX Server (OSDI'02) (Four Sections)

This paper introduces several mechanisms and policies that ESX Server uses to manage memory.  

ESX Server uses shadow page tables for software MMU, and when Ballooning Algorithm works, ESX server uses this to force the guest operating system to use its own page replacement algorithm to reclaim memory, otherwise the required memory will be reclaimed by paging out to an VMM swap area on disk. VMM realizes memory sharing mechanisms by identifying page copies by their contents with hashing. This paper slso discussed more allocation policies and remapping optimization of I/O in subsequent chapters.

### virtio: Towards a De-Facto Standard For Virtual I/O Devices (Three sections)

Before virtio, each virtualization platform had its own network, console and other drivers, and each platform implements its own virtualized emulation device.  
In order to reduce duplication in virtual device drivers, virtio provides a set of general frameworks and standard interfaces for the interaction between guests and hosts, which greatly solves the problem of adaptation between various drivers and different virtualization solutions and increase the reuse of code across the platforms.  
The first three sections of this paper introduce the virtio, the four stages of PCI configuration and the implementation of the transport layer abstraction, the Virtqueues.

### High Performance Network Virtualization with SR-IOV (Three sections)

SR-IOV is a specification of hardware enhancements for PCIe device that allows efficient sharing of I/O devices between virtual machines. It allows virtual machines to connect to I/O devices directly and have their own PCIe configuration space, bypassing VMM's intervention for data movement, which results in low latency and near-cable speeds.  
This paper introduces SR-IOV and proposes a generic virtualization architecture for SR-IOV devices which is independent of underlying VMM and highly portable. The following sections discuss some optimization strategies to reduce the overhead and conduct experiments.

### Network Virtualization Overview

This chapter introduces network virtualization with SDN-based architectures. Unlike virtual network technologies such as VLANs, modern network virtualization systems are much more powerful, which reproduces the full feature set of a physical network. It can be implemented as an overlay on an existing network, and usually does not require any cooperation from switches in the physical network, decoupling the address space of the virtual network from that of the physical network. There is a clear separation between control and data planes, which means that users can create and manage virtual networks through APIs. Network virtualization greatly simplifies deployment and configuration, increasing scalability and portability.

This chapter also introduces architecture, encapsulation with GENEVE as an example, virtual switches with OVS as an example, optimizations, the successful implementation of OVN, the new security feature and ability to create isolated networks brought by microsegmentation and the relationship between network virtualization and SDN.

### Understanding and Hardening Linux Containers (mainly Ch 2 to Ch 5)

The Linux container is a process or group of processes isolated from the rest of the system, and this article explores Linux containers and discusses their security.  

The second chapter introduces the container in the past from the perspective of the server and the client, and brings out the security problem of the container.  

The third chapter talks about namespaces. Namespaces are the foundation of container technology, which forms a foundational isolation layer by creating different userland views methods. It also introduces the implementation of 6 different types of namespaces.  

The fourth chapter introduces Cgroups, a hardware resource control mechanism that isolates and limits the resources of tasks based on demand, which could control performance or security.  

The fifth chapter talks about Capabilities, which subdivide the privileges of root (UID 0) into different groups. Capabilites exist as attributes of threads, and each functional group can be enabled and disabled independently, which could classify system calls with different privileges.

### Learn Kubernetes Basics

Kubernetes is an open source system for automatically deploying, scaling, and managing containerized applications.

A Kubernetes cluster consists of a set of nodes, the nodes host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster.

We can use Minikube to quickly deploy lightweight Kubernetes clusters, and use the kubectl command line interface to create a Deployment to deploy containers, all containers are running in Pods. All containers in the same Pod share the same IP address, IPC, hostname, and other resources. Pods abstract networking and storage from the underlying container, making it more portable. Kubernetes uses Services to define methods for accessing pods, thus enables a loose coupling between dependent Pods. With Scaling, we can change the number of replicas in a deployment. With Rolling updates, Deployments' update could take place with zero downtime.

### Architecture Guide of gVisor

gVisor is an unprivileged application kernel that provides an additional layer of isolation between running applications and the host operating system by intercepting application system calls within user space and acting as a guest kernel. The kernel is written in Go language which is more secure. It uses an abstraction called Platform for system call interception, basic context switching and memory mapping. This architecture provides flexible resource management while also reducing the fixed costs of virtualization, but its system call cost is higher, which limits its performance.

### Cloud Programming Simplified: A Berkeley View on Serverless Computing (Pages 3 to 8)

Pages 3 to 8 of this article introduce the foundation and the history of serverless computing.

Traditional cloud services worked less well for stateful services, and it takes a lot of effort to set up the environment in servers. However, serverless computing could scale automatically with no need for explicit provisioning, and be billed based on usage, which relieves programmers from managing server resources. Today’s serverless computing is better autoscaling, strong isolation, platform flexibility, and service ecosystem support.

The author also compared Kubernetes and serverless, and found that Kubernetes can provide short-lived computing environments, like serverless computing, and has far fewer limitations.

The rest of the article mainly explores the limitations and future of serverless.

### Serverless in the Wild: Characterizing and Optimizing the Serverless Workload at a Large Cloud Provider (ATC'20) (Briefly)

This paper mainly discusses the characteristics of the FaaS workload.

The author first characterizes the entire production FaaS workload of Azure Functions, and proposes a practical resource management policy, the simulation results show that the policy significantly reduces the number of function cold starts, while spending fewer resources than the fixed keep-alive policy.

### Pocket: Elastic Ephemeral Storage for Serverless Analytics (OSDI'18)

Direct communication between serverless tasks is very challenging. Due to elasticity, performance and cost factors, existing storage services are not a good fit for sharing short-lived intermediate data in serverless applications.

This article presents Pocket, an elastic, distributed data store that automatically scales, using a strict separation of responsibilities for control, metadata, and data management, to provide the performance that applications demand with low cost.

### Unikernels: Library Operating Systems for the Cloud (ASPLOS'13)

In this paper, the author present unikernels, and the implementation of their prototype system, OCaml-based Mirage.

Unikernels are sealed, fixed-purpose and single address space appliance VMs builds on past work in library OSs. The entire software stack of system libraries, language runtime, and applications is compiled into a single bootable VM image that runs directly on a standard hypervisor or hardware. Unikernels significant reduction in image sizes, improved efficiency and security, and should reduce operational costs.

### Firecracker: Lightweight Virtualization for Serverless Applications (NSDI'20)

This paper introduces Firecracker, a new open source Virtual Machine Monitor, written in Rust, which uses a Linux Kernel-based Virtual Machine (KVM) to create and manage microVMs, achieved strong security, minimal overhead and feature of multi-tenant.

Firecracker provides a minimal required device model for the guest operating system while excluding unnecessary devices and guest-facing functionality, specialized for serverless workloads and also been generally useful for other compute workloads within a reasonable set of constraints.

This paper also discusses how specializing for serverless informed the design of Firecracker and the deployment of AWS serverless services on top of it.

### Xen and the Art of Virtualization (SOSP'03)

This paper presents Xen, a hypervisor for the X86 architecture that allows multiple operating systems to share hardware resources in a secure and high efficiency way without sacrificing performance and functionality.

Compared with other full-virtualization technologies, Xen adopts the paravirtualization method, which requires modification of the guest OS, and no modifications are required to the application binary interface (ABI) and guest applications. Xen uses Domain0 as the initial control interface, hosting the application-level management software.

The paper introduces paravirtualized x86 interface from Memory Management, CPU and Device I/O. The goal of Xen is to support over 100 virtual machines simultaneously on one modern server, and authors found that Xen has a better performance than other virtualization systems at that period through experiments.

### KVM: the Linux Virtual Machine Monitor

Kernel-based Virtual Machine is a part of Linux, which can turn Linux into a VMM, enabling multiple isolated virtual machines to run on top of Linux. Linux natively has the required components for VMM, so each virtual machine on top of KVM behaves like a normal Linux process and integrates seamlessly with the rest of the system.

KVM provides device abstraction but does not emulate processors. It opens the /dev/kvm interface for use by the user-mode host: set the address space of the guest virtual machine, simulate I/O for the client and map the client's video display back to the system host.

This paper introduces the KVM architecture, MMU, I/O Virtualization and discusses other features of KVM.

### QEMU, a Fast and Portable Dynamic Translator (It's OK to not fully understand Section 2)

This paper presents the internals of QEMU, a machine emulator using an original portable dynamic translator. It emulates several CPUs (x86, PowerPC, ARM and Sparc) on several hosts operating systems (x86, PowerPC, ARM, Sparc, Alpha and MIPS), and it can run an unmodified target operating system and all its applications in a virtual machine.  

Besides full system emulation, QEMU also supports Linux user mode emulation where a Linux process compiled for one target CPU can be run on another CPU. QEMU is also uesd for simulate specific embedded devices and debugging because the virtual machine can be easily stopped, and its state can be inspected, saved and restored.

This article also examines the implementation of the dynamic translator used by QEMU.

### When Virtual is Harder than Real: Security Challenges in Virtual Machine Based Computing Environments (HotOS'05)

In this paper, authors elaborate on the capabilities that virtual machines provide and how this can adversely impact security in current systems.

The virtualization technologies can undermine today’s relatively static security architectures rely of many organizations which often assume predictable and controlled change in number of hosts, host configuration, host location, etc. Further, some of the useful mechanisms that virtual machines provide (e.g. roll-back) can have unpredictable and harmful interactions with existing security mechanisms.

Authors also discuss potential directions for changing security architectures to adapt to these demands.

### Hey, You, Get Off of My Cloud: Exploring Information Leakage in Third-Party Compute Clouds (CCS'09)

In this paper, authors discuss security vulnerabilities introduced by third-party cloud computing services.

Many cloud providers allow “multi-tenancy” — multiplexing the virtual machines of disjoint customers upon the same physical hardware. This in turn, engenders a new threat — that the adversary might penetrate the isolation between VMs and violate customer confidentiality. This paper explores the practicality of mounting such cross-VM attacks in existing third-party compute clouds.

Authors used the Amazon EC2 services to show the practicality of side-channel attacks in cloud-computing environments, explain these findings in more detail and then discuss means to mitigate the problem.

### GPU Virtualization on VMware’s Hosted I/O Architecture

GPU virtualization is a nascent field of research. This paper introduces a taxonomy of GPU virtualization strategies—both emulated and passthrough-based and describes in detail the specific GPU virtualization architecture using a virtual device model with a high level rendering protocol developed for VMware’s hosted products.

Their primary approach paravirtualizes: it delivers an idealized software-only GPU and their own custom graphics driver for interfacing with the guest operating system.

Authors analyze the performance of the GPU virtualization with a combination of applications and microbenchmarks. They find that taking advantage of hardware acceleration significantly closes the gap between pure emulation and native, but that different implementations and host graphics stacks show distinct variation.

### Do OS abstractions make sense on FPGAs? (OSDI'20)

FPGAs can deliver tremendous improvements in performance and energy efficiency for a range or workloads. In this paper, authors built and evaluated Coyote, an open source, portable, configurable "shell"' for FPGAs which provides a full suite of OS abstractions, working with the host OS. It allows us to reflect on whether importing traditional  OS abstractions wholesale to FPGAs is the best way forward.

Their evaluation shows that the price of this complete OS functionality is not acceptable in throughput, space efficiency, scheduling overhead, and memory bandwidth. Coyote shows that a coherent and reasonably complete set of OS abstractions, suitably modified, can map well onto an FPGA, deliver both immediate and longer-term benefits, and impose only a modest overhead on today’s hardware.

### Amazon Nitro (esp. the video talk on that page)

The AWS Nitro System consists of specialized Nitro hypervisors and custom ASICs that are used as the underlying platform for EC2 instances.

Nitro hypervisor is lightweight, which is based on the KVM core kernel module. And Nitro uses SR-IOV for hardware virtualization of network and storage I/O, and it also has hardware virtualization support of interrupts.

With the Nitro System, we are able to break apart the protection of the physical hardware and bios, virtualization of CPU, storage, networking and management capabilities brought by hypervisors, offload them to dedicated hardware and software, and reduce costs by allowing EC2 instances to have access to all their underlying resources – no CPU needs to be reserved for storage or network I/O, so their virtualization performance is close to bare metal.

### Hints for Computer System Design

In this paper, the author summarizes his experience in developing computer system and says that probably there is no "best" way to build the system, much more important is to avoid choosing a terrible way, and to have clear division of responsibilities among the parts. And the author gives some hits from aspect of functionality, speed and fault-tolerance of a computer system with detailed examples. There are some key points in this article that are very interesting.

In terms of functionality, he suggests:

- Do one thing at a time, and do it well. An interface should capture the minimum essentials of an abstraction. Generalizations are generally wrong, and abstraction can be a source of severe difficulties.

- Make it fast, rather than general or powerful.

- The purpose of abstractions is to conceal undesirable properties; desirable ones should not be hidden.

- Use procedure arguments to provide flexibility in an interface.

- Leave it to the client to combine these functions.

- Keep a place to stand if you do have to change interfaces.

- Do not expose implementation details and ideas, and keep the responsibilities between modules clear.

- Design common cases separately from special cases.

In terms of speed, he suggests:

- Split resources in a fixed way if in doubt, rather than sharing them. Prioritize Use static analysis and Dynamic translation.

- Use caches and hints.

- When in doubt about the design, use the brute force.

- Prioritize background computing and batch computing.

- Ensure system security first, not performance.

- Shed load to control demand, rather than allowing the system to become overloaded.

In terms of fault-tolerance, he suggests

- Use end-to-end design.

- Use a log to record state of an object.

- Make actions atomic or restartable.
