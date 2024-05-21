---
title: API Security
author: Zheng Jie
---

# API Security

## What are APIs?

Application Programming Interfaces (APIs) are defined functions from any software that may or may not be exposed to interact with other software's APIs. In today's world of software, most APIs come from web servers for interaction, which extends even to mobile application or blockchain networks. For example in web servers, a basic API interaction is with the endpoint URL:

`https://127.0.0.1:3000/api/login` with a POST method and body of objects:

## Types of APIs Protocols

Over the years, there are many standards for APIs specification. Standards are necessary to ensure the interaction between servers and browsers remain consistent. If every server has their own custom standard, it will be programmatically challenging and inefficient to format to every single standard.

### Simple Object Access Protocols (SOAP)

An older standard that has been established and tested over the years using XML format to interact. It offers a structured layout for the data to be transmitted but may be slower and less efficient than REST standards. It uses tags similar to HTML syntax, generating more characters and words which may look more confusing than REST.

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
 <soap:Body>
  <IsValidISBN10 xmlns="http://webservices.daehosting.com/ISBN">
   <sISBN>0-19-852663-6</sISBN>
  </IsValidISBN10>
 </soap:Body>
</soap:Envelope>
```

### Representational State Transfer (REST)

Most softare organisations follow REST standard since it is easy to interact with JSON objects and it is now the backbone of the internet API standard. It uses JSON objects, XML, plaintext and many other formats to send data along with standardized HTTP methods such as GET, POST, UPDATE etc.

```sh
# Example request
POST /api/login HTTP/1.1
[other request headers details...]

# Burp suite format
# username=user&password=user
{
    username: user,
    password: user
}
```

```sh
# Example response
HTTP/1.1 200 OK
[other response headers details...]

# Burp suite format
# status=success&token=a-jwt-token
{
    status: success,
    token: a-jwt-token,
}
```

### GraphQL

This is a query language that allows immediate access to specific data, without needing to use REST backend server's defined parameters. 

```graphql
query Character{
  character (id: 123) {
    name
    gender
  }
}
```

GraphQL uses 2 operation types to interact with the server to get the data:
  
- Queries: retrieve data like GET
- Mutations: alter data like POST or UPDATE

```graphql
{
    data: {
        character: {
            name: "tom boy",
            gender: "unknown"
        }
    }
}
```

This protocol reduces network resource usage and promotes flexibility to adapt to different frontend requirements, without needing the backend to modify their endpoint processes.
  

### gRPC

This is a schema driven framework to implement Remote Procedure Calls (RPC). It also strongly supports streaming of data for HTTP/2 and Protocol Buffers (Protobuf).

Four streaming modes are supported in gRPC:

1. Unary - single request, single response
2. Server streaming - single request, a stream of server responses
3. Client streaming - a stream of client requests, a single server response
4. Bidirectional streaming - Streams from both client and server for real time communication

```sh
{
    hello: "world"
}
```

The example request above will be serialized into **binary** format and sent using one of the streaming modes above and the server will deserialize the binary data into an object for further requests.

This is similar to REST format as it is not flexible like graphQL to query data. But due to binary serializatino and deserialization, data is compressed smaller resulting in less resource usage and different runtime servers (Go <=> Python) can be communicated together without facing language specific requirements.


## Securing APIs

Depending on the protocols used, the implementation may differ but the concepts stays the same as most protocols suffer from similar vulnerabilities and attack vectors. For simplicity, most concepts here will used REST protocol as it still remains as the most used protocol.

### Broken Object Level Authorization

Lets say an endpoint URL exists only for admins: `http://localhost:3000/api/admin/deleteStuff`

and an endpoint URL for users: `http://localhost:3000/api/users/login`

Malicious attackers may try and guess that there might be admin only routes and attempt to guess the endpoints and abuse the endpoint if there are no authorization checks.

```sh
POST /api/admin/deleteStuff HTTP/1.1
doSthEvil=MUAHAHAHAHA
```

Therefore, at every endpoint, proper authorization checks for users needs to be implemented before allowing such requests from executing. An example solution could be embedding access control roles into JWT token's payload and checking the payload at the endpoint route.

### Broken Authentication

JWT tokens are commonly used in today's application, which contains a specific algorithm that was used to hash the payload. A possible misconfiguration may allow tokens to be reused by malicious attackers.

For example, the server does not validate the JWT's header algorithm an attacker may do this:

```sh
{
    alg: HS256,
    typ: JWT
} 
into ->
{
    alg: none,
    typ: JWT
}
```

Since the server does not validate the correct algorithm when it receives the token, the server will assume that the tokeen's alg is correct and now the attacker can bypass authentication checks on the server.

It is important to read documents and best practices to make sure the implementation of such technologies are done correctly. Multi factor authentications are helpful to provide further authentication checks.

### Broken Object Property Level Authorization

If an endpoint allows an object of numerous properties which different user roles have access to, authorization must be implemented on such objects:

```sh
# normal user
{
    vote: "lawrence"
}
# attacker abusing admin property
{
    vote: "lawrence",
    count: "10000"
}
```

Assuming users can only vote once and admins have the option to vote with multiple counts, attackers may exploit unchecked properties and abuse it. If a property of the request can only adhere to a specific authorization level, make sure such properties are validated. Do not assume all request objects are correct and used functions like `object_to_string()`.

Schema-based validation can help different levels of authorization checks on the request objects to validate the correct properties used. Avoid endpoint complexity if possible and decouple to different endpoint URLs if necessary.

## Interview Questions

* What are the different types of API protocols?
* How would you secure API authentication?
* How would you secure API authorization?


## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [Postman - what is graphql](https://blog.postman.com/what-is-a-graphql-query/)
2. [Postman - soap api definition](https://blog.postman.com/soap-api-definition/)
3. [Postman - what is grpc](https://blog.postman.com/what-is-grpc/)
4. [OWASP - API security](https://owasp.org/API-Security/)