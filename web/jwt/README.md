---
description: >-
  What are JSON Web Tokens (JWT)? How they are used in web applications for
  authentication and authorization? Discuss some advantages and potential
  security concerns associated with JWT? Mitigations?
---

# JWT

Source:\
[https://jwt.io/introduction](https://jwt.io/introduction)&#x20;

{% embed url="https://jwt.io/introduction" %}

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>JWT example format</p></figcaption></figure>

## Format of JWT:

### Header

The header _typically_ consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

{ "alg": "HS256", "typ": "JWT" }

After which, it is **Base64Url**  encoded to form the first of the 3 parts

### Payload

* Claims are the second part of the token
* They are statements of the entity(Typically user) + additional data
* There are **3** types of claims: _registered_, _public_, and _private_ claims.

#### Registered claims:

* These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims.&#x20;
* Some of them are: **iss** (issuer), **exp** (expiration time), **sub** (subject), **aud** (audience), and [others](https://tools.ietf.org/html/rfc7519#section-4.1).

#### Public claims:

* These can be defined at will by those using JWTs.&#x20;
* To avoid collisions they should be defined in the [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) or be defined as a URI that contains a collision resistant namespace.

Private claims:&#x20;

* These are the custom claims created to share information between parties that agree on using them and are neither registered or public claims.

#### Format:

{ "sub": "1234567890", "name": "John Doe", "admin": true }

After which, it is **Base64Url**  encoded to form the second of the 3 parts

### Signature

To create the signature, the following are used to create a signature&#x20;

* encoded **header**,&#x20;
* encoded **payload**,&#x20;
* a **secret**
* **algorithm specified** in the header



#### Example signing:

`HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

## Usage

### Single Sign On is a feature that widely uses JWT&#x20;

Authentication

When the user successfully logs in using their credentials, JSON Web Token will be returned.

## Why should we use JWT?

* JWT is smaller when compared to **Simple Web Tokens (SWT)** and **Security Assertion Markup Language Tokens (SAML)**.
* Signing process for JWT is more secured than signing XML format
*   Easier mapping

    * XML doesn't have a natural document-to-object mapping
    * JSON has :)



## Problems?

Source:\
[https://portswigger.net/web-security/jwt](https://portswigger.net/web-security/jwt)

{% embed url="https://portswigger.net/web-security/jwt" %}

* Man-In-The-Middle attacks
* JWT header parameter injections
* Brute-forcing secret keys
* JWT algorithm confusion

## Mitigations

* Use an up-to-date library for handling JWTs and make sure your developers fully understand how it works, along with any security implications. Modern libraries make it more difficult for you to inadvertently implement them insecurely, but this isn't foolproof due to the inherent flexibility of the related specifications.
* Make sure that you perform robust signature verification on any JWTs that you receive, and account for edge-cases such as JWTs signed using unexpected algorithms.
* Enforce a strict whitelist of permitted hosts for the `jku` header.
* Make sure that you're not vulnerable to path traversal or SQL injection via the `kid` header parameter.

## **Best Practices:**

* Always set an expiration date for any tokens that you issue.
* Avoid sending tokens in URL parameters where possible.
* Include the `aud` (audience) claim (or similar) to specify the intended recipient of the token. This prevents it from being used on different websites.
* Enable the issuing server to revoke tokens (on logout, for example).

#### Author: [`Beckham`](https://github.com/Ninjarku)🐱‍👤

## Tool to Play around with JWT

{% embed url="https://jwt.io/#debugger-io" %}
