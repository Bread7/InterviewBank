# KVM

## What is KVM?

Kernel-based Virtual Machine (KVM) is a software feature installable on physical Linux machines to create virtual machines, it is a Linux operating system component that provides native support for virtual machines on Linux.

<figure><img src="../.gitbook/assets/Pasted image 20240606012010.png" alt=""><figcaption><p>KVM Architecture</p></figcaption></figure>

## What is the difference between KVM and VMware?

Kernel-based Virtual Machine (KVM) and VMware ESXi both provide virtualization infrastructure to deploy type 1 hypervisors on the Linux kernel. However, KVM is an open-source feature while VMware ESXi is available via commercial licenses.

### Advantages of KVM:

* Lower total cost of ownership (KVM should be free).
* Cross-platform interoperability: KVM performs on Linux and Windows platforms so you get more out of your existing infrastructure investments.
* The simplicity of a single virtualization platform to create, start, stop, pause, migrate, and template hundreds of VMs on hundreds of other hardware or software.
* Excellent performance: Apps run faster on KVM compared with other hypervisors.
* The open source advantage: Access the source code and get the flexibility to integrate with anything.

## Which is better for sandboxing? KVM or VMware?

Even though there is much similarities in the 2, KVM is currently less known by malware developers for evasion techniques, since there are more malwares looking for VMware and VirtualBox features. So in terms of sandboxing, KVM is preferred.



## Relevant topic about Virtualization:

### Full Virtualization

Virtualization is often approached as full virtualization.&#x20;

The hypervisor provides abstraction

* Hiding of hardware details from VMs to allow multiple VMs to run concurrently on the same physical machine
* Through a software-based virtualization layer.

This is typically referred to as Type 1 virtualization and one such example would be VMware ESXi.&#x20;

Full Virtualization doesn't require OS assistance to virtualize a computer or create VMs.

#### Advantages of Full Virtualization

* Doesn't require OS assistance to virtualize a computer or create VMs.
* Enables applications to be ran on completely isolated guest OSes.

#### Disadvantages of Full Virtualization

* However, the downside to this is that the hypervisor itself still adds a layer of additional complexity to the technology stack that an organization must procure, license, implement and manage.
* Applications that require direct access to the computer's hardware won't function properly in a VM.
* In bare-metal environments (Regular no VM), a server fault will affect one workload. In a virtualized environment, a physical server fault or failure can affect **every** VM running on the system.

### Paravirtualization

Known as partial virtualization, is a solution that sought to bolster early virtualization performance by enabling an OS to recognize the presence of a hypervisor.

Commands sent from the OS to the hypervisor are dubbed as _hypercalls_.

At the same time, the OS can still talk to and manage the underlying hardware layer -- that's the _para_ or _partial_ angle.

#### Advantages of Paravirtualization

* Relies on direct communication between the guest OS kernel and the hypervisor in a system, this provides improved performance levels and system utilization compared to full hypervisors without the benefit of hardware-assisted processors.
* It provides easier backups, faster migrations, improved server consolidation and reduced power consumption compared to hypervisors on legacy hardware.\


#### Disadvantages of Paravirtualization

* Because admins must modify the OS to use Paravirtualization, it limits the use of paravirtualization to OS versions, since most of such OSes are Open sourced, that are properly modified and validated for such use, thus the number of OS options available for an enterprise is limited.
* Major proprietary OSes, such as Windows Server, simply don't support paravirtualization.
* Direct communication creates a tight dependency between the OS and hypervisor, potentially resulting in version compatibility problems where a hypervisor or OS update might break the virtualization.
* The direct communication also could pose security issues for the environment as well.

As you can see, the disadvantages of Paravirtualization greatly outweighs the advantages.

## Question

* What is KVM and how does it work?
* What are some operating systems that support KVM?&#x20;
* Explain the difference between full virtualization and paravirtualization. Where does KVM fit in?

Author: [`Ninjarku`](https://github.com/Ninjarku)🐱‍👤

## Sources:&#x20;

* https://cs.brown.edu/research/pubs/theses/masters/2012/ayer.pdf
* https://aws.amazon.com/what-is/kvm/
* https://ieeexplore.ieee.org/abstract/document/9424260
* [https://www.techtarget.com/searchitoperations/tip/Full-virtualization-vs-paravirtualization-Key-differences#:\~:text=Full%20virtualization%20is%20a%20complete,to%20communicate%20with%20the%20hypervisor.](https://www.techtarget.com/searchitoperations/tip/Full-virtualization-vs-paravirtualization-Key-differences)
