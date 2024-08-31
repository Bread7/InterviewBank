# Cross-Origin Resource Sharing (CORS)

## What is CORS&#x20;

Cross-origin resource sharing (CORS) is browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin Policy (SOP).

However, it also provides potential for cross-domain attacks, if a websites CORS is poorly configured, it does not protect against CSRF

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Access-Control-Allow-Origin Header

Access-Control-Allow-Origin Header is the included in the response form one website to a request originating from another website, and ifentifies the permitted origin of the request.

A web browser **compares** the Access-Control-Allow-Origin with the requesting website's origin and **permites access to the response if they match**

### Implementing simple cross-origin resource sharing <a href="#implementing-simple-cross-origin-resource-sharing" id="implementing-simple-cross-origin-resource-sharing"></a>

The protocol headers **Access-Control-Allow-Origin** is returned by a server when a website requests a cross-domain resources, with an origin header added by the browser.

E.g.

For example, suppose a website with origin `normal-website.com` causes the following cross-domain request:

```
GET /data HTTP/1.1
Host: robust-website.com
Origin : https://normal-website.com
```

The server on `robust-website.com` returns the following response:

```
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://normal-website.com
```

The browser will allow code running on `normal-website.com` to access the response because the origins match.

### Handling cross-origin resource requests with credentials <a href="#handling-cross-origin-resource-requests-with-credentials" id="handling-cross-origin-resource-requests-with-credentials"></a>

The default behavior of cross-origin resource requests is for requests to be passed without credentials like cookies and the Authorization header. However, the cross-domain server can permit reading of the response when credentials are passed to it by setting the CORS `Access-Control-Allow-Credentials` header to true. Now if the requesting website uses JavaScript to declare that it is sending cookies with the request:

```
GET /data HTTP/1.1
Host: robust-website.com
...
Origin: https://normal-website.com
Cookie: JSESSIONID=<value>
```

And the response to the request is:

```
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://normal-website.com
Access-Control-Allow-Credentials: true
```

Then the browser will permit the requesting website to read the response, because the `Access-Control-Allow-Credentials` response header is set to `true`. Otherwise, the browser will not allow access to the response.

## Pre-flight Request <a href="#pre-flight-checks" id="pre-flight-checks"></a>

Unlike [_simple requests_](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple\_requests), for "preflighted" requests the browser first sends an HTTP request using the [`OPTIONS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS) method to the resource on the other origin, in order to determine if the actual request is safe to send. Such cross-origin requests are preflighted since they may have implications for user data.

E.g.

### Pre-flight request

```
OPTIONS /api/data HTTP/1.1
Host: example.com
Origin: http://example.org
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type, Authorization
```

One small question, what is the **Access-Control-Request-Method: POST** means?

### Response from server

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://example.org
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

In this example, the browser first sends an **OPTIONS** request to the server **BEFORE** sending the actual request, to obtain authorization information for the actual request. Upon receiving the preflight request, the server **checks the information in the request** and confirms that it **allows cross-origin requests**. Therefore, the preflight response includes CORS headers such as Access-Control-Allow-Origin and Access-Control-Allow-Methods

### Actual Request

```
POST /api/data HTTP/1.1
Host: example.com
Origin: http://example.org
Content-Type: application/json
Authorization: Bearer token
```

From the previous question, the Access-Control-Request-Method: POST means in actual CORS request, we are checking if POST methods is allowed.

### Actual Response

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://example.org
Content-Type: application/json

{"message": "Data saved successfully"}
```

### Thinking question

What if Pre-flight request failed, the server respond other than 200 status code. What will happen to browser? Will the actual Request still being sent?

The answer is no, The purpose of pre-flight request (`Option`) is to check the server whether the actual request is allowed to sent with the specified methods, headers and origin. If the server respond other than 200. The browser **WILL NOT** proceed with sending the **actual reques**t. It will **generate an error indicating that the CORS policy of the server does not allow the request**

## Interview Question

1. What is CORS and why is it important for web security?
2. What is the purpose of a preflight request in CORS? When is it triggered?
3. What is the Access-Control-Request-Method in preflight request for?
4. Will CORS protect CSRF? Why or Why not? (Research yourself)
5. What is the difference between SOP, CORS and CSP? (Recap from previous interview question)

## Author

* [Ikonw](https://github.com/Ik0nw)

## Reference

[https://portswigger.net/web-security/cors](https://portswigger.net/web-security/cors)

[https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
