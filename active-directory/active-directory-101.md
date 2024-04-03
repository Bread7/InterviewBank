---
description: Part 1 of a hopefully fruitful and long series
---

# Active Directory 101

**What is Active Directory?**

Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks. Its structure facilitates centralized management of an organization's resources which may include users, computers, groups, network devices, file shares, group policies and trusts.

Active Directory handles a variety of tasks, including authentication and authorization of users and computers in a Windows domain.&#x20;

**So, why is Active Directory important for cybersecurity?**

Active Directory can be considered "used by many, but secured by few." It presents a massive attack surface for many companies so it is important to know because when an AD environment is compromised, it typically results in complete control over the network! As security professionals, no matter if we're on the red or blue side, we will come across AD environments of all sizes throughout our careers. For this reason, we need to gain a firm grasp of the structure and function of AD and how to enumerate, attack, and remediate all types of flaws and misconfigurations.&#x20;

**How does Active Directory work?**

AD has a distributed and hierarchical structure that provides centralized management of an organization's resources.  As mentioned, these resources may include:&#x20;

* Users
* Groups
* Computers
* Network devices
* File Shares
* Group policies
* Trusts

These resources (also known as data) are stored as objects. Objects are normally defined as either resource e.g. (computers/printers) or security principals e.g. (users/groups)

A group of objects that share the same AD database is called a **domain**. One or more domains with a common schema and configuration constitute what is known as a **tree**. The top tier of Active Directory's logical structure is a **forest**, which is made up of a group of **trees**. The forest typically serves as the security boundary for an enterprise network.&#x20;

Objects within a domain can be grouped into organizational units (OUs) to simplify administration and policy management. OUs can be created according to functional, geographical or business structures. Group policies can then be applied to the OUs for simple administration.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Source: AD Structure - Source Microsoft.com Docs</p></figcaption></figure>

**Benefits of using AD**

* Security - AD helps businesses improve security by controlling access to network resources
* Extensibility - AD can be easily organized to align with companies' organizational structure and business needs
* Simplicity - Central management of user identities and access privileges across the enterprise.
* Resiliency - AD supports redundant components and data replication to enable high availability and business continuity.&#x20;

Ok that's all I need to study Crypto

**Interview Question**

What is the difference between an AD Domain and a DNS Domain?

What is AD relationship to Azure Active Directory?

What kind of directory services that AD offers?



