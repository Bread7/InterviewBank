# LLMRN

LLMNR (Link-Local Multicast Name Resolution) is a DNS-based protocol used for name resolution on the same local link. Hosts on the same local network can use this protocol to resolve the names of other hosts.

In windows when resolving hostnames the order are as follows

* Local hosts file
* DNS
* LLMNR
* NETBIOS

Believe everyone is familiar about the first 2. In Linux the hosts file is located at the `/etc/hosts` files.

It requires root permission to modify the files, same goes to windows it is located at the `%SystemRoot%\System32\drivers\etc\hosts`

Beaware it needed administrator privilege to edit (Best to open administrator privilege notepad then select file)

## LLMNR Resolving process

1\) Check Local NetBios cache

2\) If none, send broadcast message to intranet 224.0.0.252(ff02::fb) broadcasting LLMNR packets

3\) The correct hostname workstation will reply with the packet to the requestor.

Below is the example of my 2 VM in local network trying to ping using **HOSTNAME ONLY**

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

## Why we need LLMNR when have DNS?

DNS is a high efficent in resolving names, but it requires a DNS Server to work, same goes to NetBios. It requires a WINS server and NetBios do not support IPv6.&#x20;

So what are the advantage of LLMNR compare to DNS

* Local network resolution without DNS configuration: LLMNR allows devices to resolves names on the local network without requiring a centralized DNS server. This is useful when the network do not have a DNS setup.

## LLMNR Poisoning Principle

If a DNS server fails to resolve a query, the system requesting the resolution will use LLMNR (UDP 5355) or NBNS (UDP 137) to broadcast the query on the network segment in a Windows environment.&#x20;

**The attacker, responding first**, can prompt the requesting system to provide Net-NTLM hashes or plaintext credentials based on the services used during the broadcast (e.g., FTP).

## Practical with responder

So the theory is we can setup a responder that capture all the broadcast for LLMNR, and we be the first responder to tell the host that "Hey im the host you're looking for, send your data to here this is my IP address"

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption><p>Responder</p></figcaption></figure>

### So how to trigger it?

Remeber the theory of the order to resolving the address?

It will first check for local hosts file, if None, will go for DNS, if None again....

**It will broadcast using LLMNR**

We act like normal user that enter the wrong network shares.

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption><p>Access unexist network share</p></figcaption></figure>

Due to the network resources like SMB relay, the host will send it's NTLM-v2 hash for authentication, even though it's not needed(Ignore the Xing, It should be www but i didnt screenshot)

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption><p>Captured NTLMv2</p></figcaption></figure>

and we can proceed to crack it using JTH

## Challenge from SHERLOCK!

Question: Its suspected by the security team that there was a rogue device in Forela's internal network running responder tool to perform an LLMNR Poisoning attack. Please find the malicious IP Address of the machine.

Normally Domain controller will be the one giving the response, we can first find out what's the IP of domain controller

So how we find the IP for domain controller?

Yes, we can filter by kerberos traffic. (This is the most obvious when requesting tickets)

```
tcp.port == 88
```

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

Recall what we have learn in previous AD exploitation interviews,

The IP 172.17.79.136 is making a TGS-REQ to 172.17.79.4, and the dst replied with a TGS-REP.

So we can confirm the IP 172.17.79.4 is the domain controller. Next we can proceed with filter LLMNR (port 5535) and filter off the domain controller IP from both src and dst

```
udp.port == 5355 && !(ip.addr == 172.17.79.4)
```

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

Inspect the logs looking for the response packets, The outstanding ip address `172.17.79.135` is frequently reply the response. So its the attacker trying to be funny

## Author

Ik0nw

## Interview question:

1\)  What is the order when windows resolve Hostname?

2\) What is the advantage of LLMNR over DNS?

3\) How to prevent LLMNR poisioning?
