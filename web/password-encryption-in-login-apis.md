# Password Encryption in Login APIs

## Is password encryption necessary when authenticating through login APIs that use HTTPS?

HTTPS is the protocol we trust for secure web connections, using encrypted TLS connections under the hood to protect the confidentiality of our data. The question here is whether password encryption on the front-end is necessary during authentication to the login APIs when users login to a webpage that uses HTTPS.

Perhaps, this is a common question that we ask ourselves when performing penetration tests on login pages. In my experience, I often notice that web applications login APIs leave credentials in clear text without any encryption on the front end, and penetration testers may document this as a security risk in their report. However is that really considered a vulnerability?... Or not. The answer really depends.

You might also think that popular web applications perform encryption of passwords on the front-end, but the truth **might surprise you**. Let’s dive in to see the differences!

### Example 1: No Front-End Encryption (github.com)

In this below example when logging into GitHub, passwords in the POST request to the login API are unencrypted. This way, you’re solely relying on HTTPS to secure your credentials.

If an attacker successfully performs a Man-In-The-Middle (MITM) attack , e.g by stealing your TLS secrets and decrypting HTTPS traffic, your cleartext password will be fully compromised.

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p>GitHub Login Request with Cleartext Password Intercepted on Burp</p></figcaption></figure>

### Example 2: With Front-End Encryption (facebook.com)

In this other scenario where the password is encrypted **on the front-end** for facebook.com, the POST request contains the encrypted password. This way, even if the previously mentioned MITM attack succeeds, the attacker cannot obtain the victim’s cleartext password in the request. This adds an additional security layer to protect the users credentials from getting stolen.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Facebook Login Request with Encrypted Password Intercepted on Burp</p></figcaption></figure>

### **So… Is it necessary?**

To answer the question, I think that it is not necessary. From the applications perspective, encryption only protects the fact that the **attacker cannot obtain your cleartext password.** If the attacker manages to compromise your HTTPS connection, your confidentiality is already broken since he can see all requests and responses of all web activities in cleartext. Therefore, **a successful MITM attack for both examples allows the attacker to easily replay the requests containing the credentials for the login API to gain unauthorised access to the target application.**

Since HTTPS is securely designed and direct attacks against the protocol is uncommon, successful compromise of the HTTPS connection is likely due to a malware infection or an adversary exploiting a SSL/TLS CVE and **is not due to the weakness of the web application itself.** This could be the reason why companies like GitHub, Coinbase and Microsoft still leave passwords unencrypted in the POST request.

On the contrary, it’s also better to secure logins for sensitive applications, which is why most high confidentiality websites such as internet banking web applications encrypt passwords in the POST request during login. Do you think it is necessary? I'll leave it up to you to decide :smile:.

### **Interview Question**

Is password encryption necessary when authenticating through login APIs that use HTTPS?

### Author

* [Koh Rui Heng ](https://github.com/n0mi1k)
