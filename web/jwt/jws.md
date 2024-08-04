# JWS



<figure><img src="../../.gitbook/assets/Pasted image 20240524011018.png" alt=""><figcaption><p>An image of the JWT Family tree Source: [https://codecurated.com/blog/introduction-to-jwt-jws-jwe-jwa-jwk/]</p></figcaption></figure>

## What is JWS?

Of course we know it is called JSON Web Signature but what is it really?&#x20;

It is an implementation of JWT, representing content that is secured using **digital signatures** or **Message Authentication Codes** (Sounds familiar?) all using JSON-based data structure.

The most common algorithms used by JWS out there are the following:

* HMAC using SHA-256 or SHA-512 hash algorithms (HS256, HS512)
* RSA using SHA-256 or SHA-512 hash algorithms (RS256, RS512)

If you have decoded a JWT or JWS in `https://jwt.io/` before, you would note these do look rather familiar and in the other pages of this JWT gitbook, we have touched on what they mean.



## How are JWS used?

They are often used in claims. Claims are small pieces of information that are asserted regarding a subject in question.&#x20;

For example, this is a JWS

<figure><img src="../../.gitbook/assets/Pasted image 20240524015315 (1).png" alt=""><figcaption><p>JWS example highlighting the Payload portion</p></figcaption></figure>

Decoding the Token reveals the payload section (the second section of the JWS)

```json
{
      "sub": "696969",
      "name": "Jak Jak",
      "admin": false
}
```

The claim refers to the `sub`, the `name` and `admin` in this.

So what happens is that these claims get signed with a signature (often a private key), depending on which algorithm is used. The claims can then be verified by being decrypted using a secret signing key (public key). This ensure Integrity of the claims.

In essence, when you sign a JWT, you are using JWS to create it. A JWT is often a JWS that adheres to the JWT standard.

But note why are they slightly different is due to the fact that there are Unsecured JWTs where the 3rd section of the JWT is an empty space like such:

```json
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
```

## Who can create JWS?

A common misconception is that JWS can only be created by the server for authentication only, however it is actually valid both ways.

1. **Server-Created JWS (JWT Example):**
   * **Client** sends login credentials to the **server**.
   * **Server** validates credentials and generates a JWT containing user information and permissions.
   * **Server** signs the JWT with its private key.
   * **Server** sends the signed JWT (JWS) back to the **client**.
   * **Client** includes the JWT in the Authorization header of subsequent requests.
   * **Server** verifies the JWT signature to authenticate the client and authorize access to resources.
2. **Client-Created JWS (API Request Example):**
   * **Client** creates a JWS containing the API request details.
   * **Client** signs the JWS with its private key.
   * **Client** sends the signed JWS to the **server**.
   * **Server** verifies the signature using the client's public key.
   * **Server** processes the request if the signature is valid.

The Server created is mostly straight forward, server does the creation of token and is client just need to use this token for their authentication access.

But for client created wise... it is just little more complex if you want it secured.

Best would be for client to be authenticated firstly by any secure means, so the server knows that the request is valid, then the client needs to generate a private key and save it, using the private key to sign the JWS and sending it to the Server. The complicated portion lies in the storing of private keys, and the additional steps of authenticating first before this can happen.

## Want to get a practical challenge on JWS?

Go try out the [JWT Challenge](https://greenhat.gitbook.io/interview-bank/web/jwt/jwt-challenge)

## Questions

* What is the purpose of JWS?
* What makes JWS _secure_?
* Can you or should you put confidential information in JWS?
* Who can create JWS? Client or Server?

Author: [`Ninjarku`](https://github.com/Ninjarku)🐱‍👤

## Source:

* https://codecurated.com/blog/introduction-to-jwt-jws-jwe-jwa-jwk/
* https://datatracker.ietf.org/doc/html/rfc7515?ref=codecurated.com
* https://www.loginradius.com/blog/engineering/guest-post/what-are-jwt-jws-jwe-jwk-jwa/
* https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-token-claimsobsidian://open?vault=OSCP\&file=Pasted%20image%2020240524011018.png
