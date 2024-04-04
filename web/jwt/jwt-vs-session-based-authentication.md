---
description: >-
  What is the difference between JWT and session based authentication? What is
  the advantage and what's the disadvantage in the comparison of JWT to session?
---

# JWT vs Session Based Authentication

## JWT vs. Session Based Authentication (SBA)

### JWT

* **Stateless** - server don't need to keep record of the token

### SBA

* **Stateful** - Sessions are stored server-side for **PROPER** SBAs

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>JWT authentication Process</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>SBA process </p></figcaption></figure>

## Advantages of JWT

### Scalability:

* Due to their stateless nature, JWTs are ideal for distributed systems.&#x20;

### Flexibility:

* They can be used across different domains and applications.&#x20;

### Security:

* When properly implemented, they provide a secure way to handle user authentication.
* Implement short-lived JWTs and use refresh tokens for renewing access without re-authentications

## Disadvantage of JWT

### Transmission Security:

* MUST transmit JWTs over HTTPS
* Else it can be sniffed/stolen and abused&#x20;

### Storage:

* Storage of JWTs are entirely on client side, thus it is important to store them securely to prevent XSS attacks and other vulnerabilities&#x20;

### Unpredictability:

* It does not have revocation efficiency
* If a valid JWT token stolen during its validity period, it can be used for authentication without the user's knowledge even if the user has logged out
* To prevent this, additional processing logic at the backend has to be used, this leads to more overhead and partially defeats the purpose of using JWT, when the goal was to be stateless to begin with

## Advantages of SBA

### Simplicity and Reliability:

* The server's session record acts as a centralized truth source, making it straightforward to manage user sessions.&#x20;

### Revocation Efficiency:

* Access can be quickly revoked by deleting or invalidating the session record, ensuring up-to-date session validity.

## Disadvantages of SBA

### Performance Issues (High Latency) at Scale in Dynamic Environments:

* The dependency on database interactions for every session validation can introduce latency, especially for high-traffic applications.
* In applications with dynamic clients, this latency can impact user experience, making session-based authentication less ideal in such scenarios.

## **Why is JWT good?**

* Cross Site Request Forgery (CSRF) uses cookies to direct forged attacks towards a target, if the user has a cookie that is stored on the browser, this attack vector will be able to abuse the cookie to accomplish their malicious actions

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Example of a CSRF URL embedded in an &#x3C;a> tag</p></figcaption></figure>

* As JWT does not use cookies, it negates CSRF attacks



## What to do in the process of choosing the type for a server setting?

Python choose if else to determine your needs XD

```
requirement = input("What are your needs?") 

if ("stateless" in requirement.lower()):
	print("JWT")
elif ("scalable" in requirement.lower()):
	print("JWT")
elif ("secure" in requirement.lower()):
	print("JWT")
elif ("immediate control" in requirement.lower()):
	print("SBA")
else:
	print("Rethink what you want :)")
```

## Long story(code) short:

If you need **stateless**, scalable, and/or more security then choose JWT

If you want  efficient immediate control, choose SBA



## **Want MORE security?**

* Meet JSON Web Encryption (JWE)
* It is the encrypted form of JWT, using 5 header sections instead of 3

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>JWE Using 5 headers instead of 3</p></figcaption></figure>

* It is significantly considered harder to decrypt compared JWT, providing more security for end users (Disclaimer: This still DOES NOT mean you should store sensitive information in the JWE)
* More about JWE&#x20;
  * https://www.forgebox.io/view/JWTSignEncrypt
  * https://www.scottbrady91.com/jose/json-web-encryption



Sources:

* [https://juejin.cn/post/7110044736848658445  ](https://juejin.cn/post/7110044736848658445https:/dev.to/codeparrot/jwt-vs-session-authentication-1mol)
* [https://dev.to/codeparrot/jwt-vs-session-authentication-1mol#:\~:text=Choosing%20between%20JWT%20and%20session,authentication%20holds%20the%20upper%20hand.](https://juejin.cn/post/7110044736848658445https:/dev.to/codeparrot/jwt-vs-session-authentication-1mol)
* [https://iq.opengenus.org/user-authentication-techniques-types/](https://iq.opengenus.org/user-authentication-techniques-types/)
* [https://www.forgebox.io/view/JWTSignEncrypt](https://www.forgebox.io/view/JWTSignEncrypt)
* [https://www.scottbrady91.com/jose/json-web-encryption](https://www.scottbrady91.com/jose/json-web-encryption)
