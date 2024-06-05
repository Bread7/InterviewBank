# KVM

## What is KVM?

Kernel-based Virtual Machine (KVM) is a software feature installable on physical Linux machines to create virtual machines, it is a Linux operating system component that provides native support for virtual machines on Linux.

<figure><img src=".gitbook/assets/Pasted image 20240606012010.png" alt=""><figcaption><p>KVM Architecture</p></figcaption></figure>

## What is the difference between KVM and VMware?

Kernel-based Virtual Machine (KVM) and VMware ESXi both provide virtualization infrastructure to deploy type 1 hypervisors on the Linux kernel. However, KVM is an open-source feature while VMware ESXi is available via commercial licenses.

### Advantage of KVM:

* Lower total cost of ownership (KVM should be free).
* Cross-platform interoperability: KVM performs on Linux and Windows platforms so you get more out of your existing infrastructure investments.
* The simplicity of a single virtualization platform to create, start, stop, pause, migrate, and template hundreds of VMs on hundreds of other hardware or software.
* Excellent performance: Apps run faster on KVM compared with other hypervisors.
* The open source advantage: Access the source code and get the flexibility to integrate with anything.

## Which is better for sandboxing? KVM or VMware?

Even though there is much similarities in the 2, KVM is currently less known by malware developers for evasion techniques, since there are more malwares looking for VMware and VirtualBox features. So in terms of sandboxing, KVM is preferred.

Author: [`Beckham`](https://github.com/Ninjarku)🐱‍👤

## Sources:&#x20;

https://cs.brown.edu/research/pubs/theses/masters/2012/ayer.pdf https://aws.amazon.com/what-is/kvm/ https://ieeexplore.ieee.org/abstract/document/9424260
