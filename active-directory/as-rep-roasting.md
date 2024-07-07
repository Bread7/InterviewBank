# AS-REP Roasting

## Recap

Again, have some recap on the AD kerberos terms:

**AS**: Authentication Service, a service that generates TGTs (Ticket Granting Tickets) for clients.

**TGS**: Ticket Granting Service, a service that **generates tickets for a specific service** for clients.

**TGT**: Ticket Granting Ticket, an admission ticket that allows the client to obtain a ticket, **serving as a temporary credential.**

**Ticket**: A credential used for mutual access between objects in a network.

**AD**: Account Database, stores the whitelist of all clients. Only clients on the whitelist can successfully apply for a TGT.

**DC**: Domain Controller.

**KRBTGT**: Every domain controller has a krbtgt account, which is the service account for the KDC (Key Distribution Center), used to create the keys for **encrypting TGS (Ticket Granting Service) tickets.**

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption><p>TGS Requesting process</p></figcaption></figure>

**1) AS-REQ** : Client sent AS-REQ to Authentication Service, the request credential is timestamp encrypted by client's hash(Derived from the NTLM hash of the user's password)

**2) AS-REP**: KDC use client hash to decrypt, if its able to decrypt to it is a timestamp. The Authentication Service will use his krbtgt hash to encrypt the TGT, this TGT contains PAC (Privilege Attribute Certificate) . PAC Contains the client SID and groups.

**3) TGS\_REQ**: Client use this TGT tickets request a specific Service to TGS.

**4) TGS\_REP**: Ticket Granting Service use krbtgt hash decrypt this ticket, if its correct, it will return the SERVICE hash encrypted TGS.

**5) AP\_REQ**: Client take the TGS to request for service

**6) AP\_REP**: The service will use its hash to decrypt the TGS (Ticket Granting Service ticket). If the decryption is correct, it will extract the PAC (Privilege Attribute Certificate) and verify it with the TGS for authorization. The domain controller will decrypt the PAC, obtain the user's SID (Security Identifier) and group information, and, according to the service's ACL (Access Control List), verify if the user has the appropriate access privileges.

## AS-REQ Roasting

For domain users, if the option "Do not require Kerberos preauthentication" is set, an AS\_REQ request sent to the domain controller's port 88 will result in the domain controller returning the TGT ticket and encrypted session key information without any verification.&#x20;

### Enumeration

To enumerate vulnerable users

```
Get-DomainUser -PreauthNotRequired -verbose #List vuln users using PowerView
```

The received AS\_REP content (the cipher under enc-part, which is encrypted with the user's hash) can be recombined into the format "Kerberos 5 AS-REP etype 23" (18200). This format can then be used with hashcat to perform an offline attack, ultimately obtaining the user's plaintext password.&#x20;

```
Rubeus.exe asreproast  > 1.txt
```

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

Append `$23` after `$krb5asrep`. And we can now use hashcat to perform a offline cracking

```
hashcat64.exe -m 18200 hash.txt pass.txt --force
```

## Author

Ik0nw

## Interview Question

1\) In AS\_REQ, why they use the timestamp? Is the timestamp protection from anything?

2\) In TGS\_REP, the Ticket Granting Server decrypt the TGS ticket, will the TGS verify the user ACL?&#x20;

3\) What is the settings in order the account is vulnerable for AS\_REQ roasting?

4\) Which step does the AS\_REQ Roasting skip to obtain the ticket?

5\) Here is some log file, are you able to identify what is the timing the attacker launch AS-REP Roasting attacks? (Challenge from hackthebox Sherlock)

6\) Which account does the attacker target?

{% file src="../.gitbook/assets/Security (1).evtx" %}
