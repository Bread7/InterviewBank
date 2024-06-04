---
title: API Security Principles
author: Zheng Jie
---

# API Security Principles

In the [API Security Overview](./security-overview.md), different types of APIs were discussed along with some samples of authentication and authorization issues. This topic is a continuation of API Security and other aspects to take note of during penetration testing or defence for the web application. 

## Sanitization and Validation of Data

SQL injection is the most common vulnerability found across applications worldwide. At times it can be due to poor sanitization of inputs in the html forms, poor sanitization of inputs at the backend APIs or it could be insecure SQL query that may fetch data. Ultimately, it has and will always be iterated again to always perform proper validation checks on the inputs to ensure malicious inputs are never executed. Some implementations may involve:
 
 1. Schema validation - Make sure user inputs are sent in the correct format such as numbers (42) should not be received as strings ("42")
 2. Regex - Custom patterns to make sure user inputs are formatted in certain manners. However, avoid creating regex unless you are sure the pattern is well designed otherwise, it can be easily bypassed
 3. Use popular open source sanitization libraries to ensure data validity - Similar to cryptography, well built libraries are much better than custom implementations due to production usages and transparency for continuous improvements

What about the other way around? If data received must be sanitized, can the data to respond with be 100% trusted? 

The answer is no. Simply put, malicious data causes server to perform unwanted requests which results in unintended responses. Moreover, these data may be sent back in the form of an error (e.g. SQL injection). Therefore, responses from the server should be validated as well especially errors.

```javascript
statusCode: 500,
message: Internal Server Error,
error: SQL error in "/": stack trace: ...errors ...some sql query data
```

Seeing an error such as the example above causes an implication that your server might have potential faults and such errors allow malicius actors to further act on it.

```javascript
statusCode: 500,
message: Internal Server Error,
error: Server has an error
```

Best practice is to log the error and force return a custom generic error message. The same solution for input sanitization can also be implemented for output sanitization to prevent returns of actual detailed errors. While it does not prevent your application from being exploited, generic error message may act as deterrence and cause doubts on their malicious attempts.

## Error Handling and Logging

Depending on the application, guidelines may need to be adhered (e.g. HTTP code for web). Regardless, errors should never be handled by its own API functions but instead, a catch-all net or central error handling service should be used to manage all the errors and how to correctly output them without disclosing too much information.

In the same central error handling service, all errors can also be logged at the same time which can be sent to a logging server for processing and analysing.

```js
// Example default api
return reply(500, error)

// Central error and logging
return replyHandler(reply, 500, error)
// // in error.js
if (error) {
    // ...do some log functions and handle error
    return reply(500, "custom error message")
} 
```

Another example would be Javascript's `JSON.parse()` function which is used to convert strings to JSON object. While it seems like a simple type casting function, it is potentially memory intensive andmay require more than 1Gb to invoke if the input value is too large.

```js
try {
    JSON.parse(value) // <- if value not sanitized as correct json format, this will produce a memory error
} catch (err) { 
    // handle error
}
```

## Use Throttling and Rate Limiting

Rate limiting controls number of requests made to the API to prevent over loading, abuse or spamming in general. Throttle stops requests if the load on the server is too high, ensuring traffic does not have huge spkies for long duration and allowing access once traffic is back to normal flow.

With reference to `JSON.parse()` example, if an attacker see this serializer function, it can be used as an attack vector if no sanitizing/chunking of input is implemented. This means the value passed in could be in large sizes like 1Kb or 1Mb in length or completely invalid format for JSON string which results in the host machine's memory to be hogged up and corrupted. 

Imagine using Burp Suite to repeatedly send in the request constantly and as a result, the application will crash within a few requests invoked.

```js
const express = require('express');
const app = express();
const PORT = 3000;
 
app.get("/", (req, reply) => {
    try {
        const result = JSON.parse("{")
    } catch (err) console.log(err)
    res.send({ result: result, msg: "Request completed." })
})

app.listen(PORT, function(err){
    if (err) console.log("Error in server setup")
    console.log("Server listening on Port", PORT);
})
```

Therefore, APIs should have rate limiting and throttling implemented to prevent repeated abuse. Especially for time-sensitive or data formatting/serializing functions, they tend to require more memory usage than normal functions. Third party libraries could be used if it is popular and widely adopted otherwise custom implementation (e.g. using redis as temporary cache storage to keep track of data)

If rate limiting and throttling is insufficient, further memory management could be done in place to ensure processes do not take up too much. Furthermore, API gateways such as [Kong](https://konghq.com/) or [tyk](https://tyk.io/) can be integrated to further monitor the API services and analyse the resource usage charts.

## Interview Questions

* Explain what is meant by ‘rate limiting’ in API testing and why it's important.
* Explain validation of data input and output.
* What are some strategies to handle logging.


## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [Stackhawk - API Security Best Practices](https://www.stackhawk.com/blog/api-security-best-practices-ultimate-guide/#authorization-and-access-control)
2. [Qualysec - Ultimate guide to API security](https://qualysec.com/the-ultimate-guide-to-api-security/)
3. [Logrocket - rate limiting](https://blog.logrocket.com/rate-limiting-node-js/)