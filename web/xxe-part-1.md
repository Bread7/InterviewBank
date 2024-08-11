# XXE Part 1

XML External entity injection is a vulnerability that allows attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external ssyytems that application can access.

## What is XML?

XML is very similar to HTML

```html
<html>
    <head></head>
    <body>
        <h1>Hi this is HTML</h1>
    </body>
</html>
```

HTML (HyperText Markup Language) is a markup language used for creating the structure of web pages, with predefined tags like `<a>`, `<body>`, `<p>`, and `<img>` for common elements.&#x20;

On the other hand, XML (eXtensible Markup Language) is a flexible markup language used for storing and transporting data, where the tags are not predefined and can be customized to suit specific needs

## XML Design

The original purpose of XML is to stored datas like a mini database.

```xml
<user>
    <username>Chen xing</username>
    <age>18</age>
</user>
<user>
    <username>Ong zhengjie</username>
    <age>24</age>
</user>
```

Tags like `username` and `age` are customized, however XML also reserve some of the predefined tags like HTML.

In XML they are called `ENTITY`

Example:

```
<!ENTITY user "Chenxing">
```

This create a variable `user` and assigned the value `Chenxing`

In order to use the entity in XML, we have to specifed it in XML forms

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE body [
    <!ENTITY user "Chenxing">
]>

<body>
    <message>&user;</message>
</body>
```

## Interview Questions

1）What is the theory of XXE and what's the mitigation

