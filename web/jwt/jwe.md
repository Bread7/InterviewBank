---
description: >-
  By the end of this read, readers should be able to explain what is JSON Web
  Encryption (JWE).
---

# JWE

### What is JWE?

In laymen term, JWE is used to securely sending (secret) information so only the intended recipient can read it. Kind of like a secret message locked in a box and only the person with the right key can open the box to read the message (Basically like every other encryption techniques).

### So what's the diff?

We have learnt so far from the previous git books I had wrote, that JWTs are able to help you in Authentication, they are not stored server side so the tokens must be well protected on the client side when sent to them and received back, so how is this done? JWE provides an additional layer of protection, we know we can easily see the payload of JWTs when they are placed into a JWT decoder either on Burpsuite or online, but what JWE does is that it does an additional encryption on the payload, this way only the intended recipient can decrypt it.

#### Example JWE

<figure><img src="../../.gitbook/assets/Pasted image 20240517155208 (1).png" alt=""><figcaption><p>Note that the payload is gibberish and you cannot understand it without decrypting it.</p></figcaption></figure>

&#x20;

For you to follow along, feel free to copy this JWE and try the steps we write about in this book

```jwe
eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZDQkMtSFM1MTIiLCJraWQiOiIxOGIxY2Y3NThjMWQ0ZWM2YmRhNjU4OTM1N2FiZGQ4NSIsInR5cCI6IkpXVCIsImN0eSI6IkpXVCJ9.gCbxP78o3DgpDTUQbuHniuGgYpATqgGkRGy7paC6hRrz7N7eIa6sAOWDO9Fhnj-c8ocMl4cF4Jb_mv5qRPCh9r57PBqx7jOhMIMPTwJGpjcyBaqtHlZlu1vupY5tQ3Y2jGz1Ti4BnywaeEHPyIPQJtN7F7hIAORzj7IY4sIKkVXtQJZgaKW8pEHq_GCqj8i5aaiM0uJnRG3GOh3livp9Npjv9doqp3gyPa1zjrg2H1RsOGn0j2QMGvtuVfkuNwF-SoPKFECyHOq0ZK1oH2sTO8-JwvHflbIZQr5xWTpS8q7MbUXEuqURtrg0Tj-2z6tdaOLT4b3UeDufK2ar3bBfRD4-nRALtoY0ekcMyGFOS7o1Mxl3hy5sIG-EySyWeuBVy68aDWDpi9qZoQuY1TbxxakjncCOGu_Gh1l1m_mK2l_IdyXCT_GCfzFq4ZTkPZ5eydNBAPZuxBLUb4BrMb5iDdZjT7AgGOlRre_wIRHmmKm8W9nDeQQRmbIXO23JuOw9.BDCarfq2r_Uk8DHNfsNwSQ.4DuQx1cfJXadHnudrVaBss45zxyd6iouuSzZUyOeM4ikF_7hDOgwmaCma-Z97_QZBJ5DzVn9SJhKUTAqpVR3BRGAxJ_HAXU5jaTjXqbvUaxsh7Z5TgZ9eck0FIoe1lkwv51xEvYqqQ_Xojr4MAEmLuME_9ArCK9mNaMADIzOj4VoQtaDP1l26ytocc-oENifBRYGu28LbJLkyQKzyQy6FuAOtWjLM0WCXV7-o_dvj6qfeYHNBD7YBSxyqdgD8dcxMBNd2sK73YsZPHEa0V1-8zz7hm3bH3tZelpwPWScqLLW_SUH586c0FVeI6ggvqzjfLZ_Y6eQibVSdXfOtJBk22QrLsuCXbRK8G1w9t23Pwu8ukUAw4v0l7HeaW_0SJyKSPQANRP83MyFbK7fmzTYaW9TYN2JrKN-PLpd2dIFSm2Ga_EfaCwNJBm4RDMzDNrf-O0AissvYyHb0WaALiCiFCogliYqLzRB6xDb-b4964M.J7WDOFLRRPJ7lLpTfN2mOiXLDg5xtaF-sLQ4mOeN5oc
```

Look closer at the token now, notice anything different of it from the original JWT?

* The answer is that it has 4 "."
* Use `ctrl f` if you don't believe me

## How do I understand this???

* First section: The JWE Protected Header
* Second section: The JWE Content Encryption Key
* Third section: The JWE Initialization Vector (IV)
* Fourth section: The JWE Payload
* Fifth section: The JWE Authentication tag

### The JWE Protected Header

We look closer at the header:

```json
{
  "alg": "RSA-OAEP",
  "enc": "A256CBC-HS512",
  "kid": "18b1cf758c1d4ec6bda6589357abdd85",
  "typ": "JWT",
  "cty": "JWT"
}
```

Notice the (typ) header and (cty) header are the same? It is actually not referring to the same thing, the (typ) header refers to this token itself, while the (cty) refers to the information hidden in the encrypted payload. So in this case, it is a nested JWT :)

The method of encryption used by JWE can vary to make it more secured, in this situation, the mode of encryption is actually called _Hybrid Encryption_. So what it essentially does is it uses symmetric encryption along with asymmetric encryption.

#### Symmetric

Based on the token example, we see the (enc) header, it states "A256CBC-HS512" which actually translate to symmetric AES-256-CBC using HMAC-SHA-256 for encrypting the content of the payload.

#### Asymmetric

As for the asymmetric encryption you would already have guessed is in the (alg) header. The example uses RSA with OAEP (Optimal Asymmetric Encryption Padding).

Of course these are not the best algorithm, and there exist better algorithms out there.

### The JWE Content Encryption Key

A Content Encryption Key (CEK) is used to encrypt the JWE payload during the symmetric encryption phase, this key must be different for each token and generated for a single use only, never to be reused.

### The JWE Initialization Vector (IV)

It is used when encrypting the plain text, this part must be a unique random value, if not used, it will be replaced with an empty section like "xxxxx.xxxxx..xxxxx.xxxxx"

### The JWE Payload

This is the payload section where the symmetric key encrypted content goes through another round of encryption using the Asymmetric key in the example, our example, the encryption would be RSA with OAEP.

### The JWE Authentication tag

This is the final piece to a JWE, it is created during authenticated encryption to allow the verifier to prove integrity of the cipher text and the header. It is created during encryption and used to validate during decryption.

If unused, like the Initialization Vector, it will also be an empty section.

### Why Use JWE?

* **Security**: It ensures that only the intended recipient can read the message.
* **Integrity**: The message cannot be altered without detection, since only the server should have the private key.
* **Confidentiality**: Sensitive information stays private.

### Questions

* What are the use cases of JWE? (Name 1)
* Should you and can you store sensitive information in JWE?
* What are some ways you would implement JWE securely in a E-commerce environment?

Author [`Beckham`](https://github.com/Ninjarku)🐱‍👤

### References:

* &#x20;https://auth0.com/docs/secure/tokens/access-tokens/json-web-encryption
* https://www.scottbrady91.com/jose/json-web-encryption https://jwt.io/
