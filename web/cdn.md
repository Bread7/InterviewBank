# CDN

## What is CDN?

**Web Application Firewall (WAF)**: Many CDNs offer built-in WAF capabilities that can identify and block malicious network traffic and attack attempts, such as SQL injection and Cross-Site Scripting (XSS) attacks.

**TLS/SSL Encryption**: CDNs can provide data encryption services to ensure data security during transmission, protecting against eavesdropping and tampering.

**Content Optimization and Caching**: By caching and optimizing the delivery of content, CDNs can reduce the load on websites, thereby lowering the server load caused by sudden traffic surges or attacks.

**Rate Limiting**: CDNs can impose limits on request frequency to prevent malicious users or bots from making excessive requests to the server, thus conserving server resources.

**Intelligent Routing**: CDNs use their globally distributed server network to intelligently select the optimal data transmission path, avoiding impacts from local network attacks or failures.

**Threat Intelligence**: Some CDN providers also possess the capability to collect and analyze threat intelligence, identifying emerging threats and attack patterns, and timely updating their security mechanisms to counter these threats.

**Edge Computing**: CDNs can execute security policies and application logic at the network edge, further enhancing security and performance.

Generated from chatgpt, but above bascially give rough idea of how CDN protectes the original site.

## Possible ways to discover the real server IP

1\) **Dns History**: there are sites that stores the DNS history record that may potential leak the original server IP address before binding to CDN.

E.G. [**https://completedns.com/dns-history/**](https://completedns.com/dns-history/)

[**https://securitytrails.com/**](https://securitytrails.com/)

2\)  **SSL Certficate**: SSL/TLS certificates can sometimes reveal information about the server, including its IP address. When a certificate is configured, especially **before a CDN is deployed**, details within the certificate or the process of obtaining the certificate might leak the server's real IP. For example, if the **SSL certificate was generated for the original IP** or domain **before the CDN was implemented**, and if these details are not updated or hidden post CDN implementation, they can be used to **trace back to the original server's IP address.**

3\) **Secondary Domain**: Not all Company domains are all registered with the CDN. For example, facebook.com domain is using for CDN, but xxxx.facebook.com might not be using CDN. **Perform a /24 subnet scan with the ip adress it may possible discover the IP address of the main domain server IP**.

However, in some condition if the server **only white listed the CDN domains**, the IP scan technique will not be working.

4\) **RSS mail from Server**: If these emails are sent directly **from the original server** rather than through a mail delivery service that masks the server's IP, then analyzing the **email headers could potentially reveal the server's IP address.**

5\) **Passive Acquisition:** Find a way to make the target web server initiate a request to our own server or collaborator.

1. If the target system has a mailing function, usually found in user registration/password recovery sections,
2. The next step is to extract header information from the target emails. For example, we can subscribe to the target service, create an account, use the "forgot password" function, or order some products... In short, we need to find a way to get the target to send us an email (in such scenarios, we can use Burp Collaborator).
3. After receiving the email, we can view its source code, especially the email headers, and note down all the IP addresses, including subdomains. These details are likely related to the hosting service.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
