# Setuid

## What is setuid()?

Setuid is a binary capability feature that can allow users to gain elevated privileges when using them.

`setuid` (short for "set user ID") is a special file permission in Unix-like operating systems that allows users to execute a file with the permissions of the file's owner rather than the permissions of the user who is running the file. This is particularly useful for programs that require elevated privileges to perform certain tasks.

For example, if a file owned by the root user has the `setuid` bit set, executing that file will give the process root privileges, even if the user running the file is not root.

## How to find setuid binaries?

`find / -perm -u=s -type f 2>/dev/null` A command to find most setuid bits on a machine, some are more hidden, so more enumeration may be required.

## What's the harm of setuid permission?

Lets use this bash code for example

```bash
/bin/chmod +s /bin/bash
```

Now we just gave `/bin/bash` a setuid, meaning it can be ran as the root, what does this mean? It means any shell spawned with this command, can potentially give you a privilege shell!&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20240820000209 (1).png" alt=""><figcaption><p>Getting shell through a setuid /bin/bash binary</p></figcaption></figure>

Of course this is fun to attackers as its a good way to gain root access to the machine, but for defenders, this is a nightmare to have, since a feature that was meant to help them automate mundane scripted tasks, could turn out to be more harmful to them instead.

## So if I have setuid means I am (G)root?

Long story short, the answer is NO\~!

Take this challenge in Vulnhub My-CMSMS machine for example&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20240819234248 (2).png" alt=""><figcaption><p>File bianry.sh has setuid marked when searched</p></figcaption></figure>

It has the contents of:

```bash
#!/bin/bash
echo "Usage: binary.sh COMMAND" 
echo `$1`
```

What the script does is that it will echo the command that is fed to it, potentially executing it.

The binary.sh file has a setuid bit, however, it does not run with root privileges when executed:&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20240819235147.png" alt=""><figcaption><p>Execution of bianry.sh to call busybox shell back to our netcat listener</p></figcaption></figure>

When we get the reverse shell, it is just the same user's shell being called back&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20240819235121.png" alt=""><figcaption><p>Reverse shell received with no change in privilege</p></figcaption></figure>

This is all despite the file being root owned and given setuid&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20240819235342.png" alt=""><figcaption><p>Setuid permissions set with root as file owner</p></figcaption></figure>

The use of setuid() system call is required to get the privileges, so if you are on a vulnerable system just having the setuid bit there does not automatically mean that the system runs with elevated privileges, the process must trigger this binary with the setuid() system call for it to work.

Author: [`Ninjarku`](https://github.com/Ninjarku)🐱‍👤

## Interview questions

* Explain the setuid bit and its primary use in Unix-like operating systems.
* What is the difference between `setuid` and `setgid`? (You can google `setgid`)
* Describe a real-world application or system where `setuid` is crucial. What role does it play in that application? (Example: automation business)
* Think of how this application may be vulnerable to exploitation. How would you address such vulnerabilities?
* Explain how an attacker might exploit a vulnerable `setuid` program. What steps can be taken to prevent such exploits?

## References

* https://www.vulnhub.com/entry/my-cmsms-1,498/
* https://man7.org/linux/man-pages/man2/setuid.2.html
* https://www.cbtnuggets.com/blog/technology/system-admin/linux-file-permissions-understanding-setuid-setgid-and-the-sticky-bit
