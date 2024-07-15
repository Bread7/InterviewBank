# Windows Prefetching



## Introduction

Prefetching is a Windows memory management process in which the operating system pre-loads resources from disk into memory as a means of speeding up the loading time for applications. As part of its process, a .pf file is created in the **C:\Windows\Prefetch** directory and updated each subsequent time the application is executed.&#x20;

The .pf file contains a list of resources, including files and directories that the executable referenced during execution, which is used to pre-load those resources the next time the application is executed.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Prefetch Directory Listing</p></figcaption></figure>

### **What is the use of Prefetch Files?**

1. These files are used to study the behavior of the Application means which application executes automatically or not etc
2. Prefetch files can be used for **forensic analysis of the particular Application.**
3. Analysis of the viruses can be studied with the **help of prefetch files.**

## Prefetch file properties

Prefetch files are **enabled by default on all Windows operating systems.**

Prefetch, however, is **disabled by default on Windows server operating systems** and can be enabled through the registry.

In every windows OS there's a limitation of prefetch files



| Version    | Maximum File |
| ---------- | ------------ |
| Windows XP | 128          |
| Windows 7  | 128          |
| Windows 8  | 1024         |
| Windows 10 | 1024         |

### **Prefetch configuration from the Registry**

Prefetch can be configured from the registry. The path of the prefetch configuration in the registry is:

“_HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters_”

<figure><img src="https://lh6.googleusercontent.com/uVRpX-GaI2bUXq_-GNI9U7WqF3C4Aqpt0zKJ6xBuMn3F8n1bVv9NXoq7_jNFSBumwX-McBijQ34ypkCx_P3FHQ4w42HFdgEh16dCYlq04qIgXw-f5Y4bDN3zbyBfkdX2BXV3VUFAUFLEcwwlhAm2tKVzv0TE0JJA3BPw8530d_cO7193dX3GcQJENA" alt=""><figcaption></figcaption></figure>

Prefetcher is enabled and the data value is 3. You can change the configuration of prefetcher by changing the data value.

There are 4 values to enable prefetcher:

|   |                                                    |
| - | -------------------------------------------------- |
| 3 | Enable Prefetcher for application startup and Boot |
| 2 | Enables Boot prefetching                           |
| 1 | Enables Prefetcher for application startup         |
| 0 | Disables Prefetcher                                |

## How Investigators Use Prefetch File Contents

The prefetch file, while not intended for analysis, can provide a wealth of information for an investigator. When opened, a prefetch file can show:

* Creation date – timestamped with the local time of the machine
* Date/time of last execution time – timestamped with the local time of the machine
* Run count – the number of times the executable has been launched
* Other run times – limited to the previous eight (8) executions
* Directories and files referenced – includes other executables
* Volumes and file paths – the location from which files were accessed

## **Example walkthrough**

### **This is one of the question from sherlock (campfire-1)**

_What is the full path of the tool used to perform the actual kerberoasting attack?_

Given the prefetch folder we are going to find the tool is used for the attacking, and the full path of the files

{% embed url="https://f001.backblazeb2.com/file/EricZimmermanTools/net6/PECmd.zip" %}

We are using this PECmd.exe to perform the tasks.

```
PECmd.exe -d prefetch --csv . --csvf prefetch.csv
```

pointing the directory to prefetch folders and process them output it in csv format

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Scrolling abit we found the tool to be **Rubeus.exe** .&#x20;

To individually analyse the file we can directly parse the file to Pecmd.exe

{% code overflow="wrap" %}
```
PECmd.exe -f prefetch\RUBEUS.EXE-5873E24B.pf
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### _When was the tool executed to dump credentials?_

Since we know the attacker uses Rubeus.exe&#x20;

Pecmd also provides the timestamp of the application is executed

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Author

Ik0nw

## Interview Question

1\) What is prefetch files?

2\) Does Windows Server enable prefetching by default?

3\) What is the maximum prefetch file for windows 11?
