---
description: An introduction to SSTI vulnerability
---

# Server-side Template Injection (SSTI)

SSTI is when an attacker can manipulate native template syntax to inject a malicious payload into a template, which causes code to be executed on the server side. Before we dive into SSTI, let's find out the use of a template engine.

<figure><img src="../.gitbook/assets/code (1).png" alt=""><figcaption><p>A basic SSTI vulnerability! </p></figcaption></figure>

### What is a template?

Template engines are designed to generate web pages by combining a fixed static template with volatile data.&#x20;

### So how does SSTI occur?

SSTI occurs when user input is concatenated directly into a template instead of being parsed as data. This allows attackers to inject template directives to manipulate the template engine to control the web server! e.g. (`{{os.system('whoami')}}`).

### Impact of SSTI

Since SSTI payloads are delivered and evaluated server-side, it makes this type of vulnerability very dangerous! While the types of attacks can vary based on the template engine in use and how the web application uses it, SSTI can potentially lead to full remote code execution but even if it can't, it allows attackers to potentially gain read access to sensitive data/arbitrary files on the server.

### How to exploit SSTI?

Consider the code `render('Hello ' + username)` Imagine that you are able to input a username using a GET request. So you would enter something like this: `http://greenhat.gitbook.io/?username=${7*7}`.

If the resulting output is `Hello 49`, then you would know that mathematical operation is being evaluated by the template engine server-side. This is a simple example, irl, you probably need to test a few syntaxes to get it right unless you know the underlying template engine running in the web application.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>A great SSTI evalution decision tree from PortSwigger!</p></figcaption></figure>

Once you detect that a potential SSTI vulnerability exists and you know the template engine, you can try to find ways to start exploiting it!

### How to prevent SSTI?

For a start, if possible, do not allow users to modify or submit new templates. But this might depend on your business case and it might not be feasible for everyone. Another alternative is to use a "logic-less" template engine like Mustache until you require logic ahahaha. Separating logic from presentation can help to reduce your attack surface from the most dangerous template-based attacks. Another way is to sandbox your template environment in a locked-down docker container to prevent attackers from pivoting :stuck\_out\_tongue\_closed\_eyes:

Ok that's all for today, I am tired.

### Interview Questions

* How does SSTI differ from other injection attacks like SQL Injection and Cross-Site Scripting (XSS)?
* What are some best practices for preventing SSTI vulnerabilities in web applications?

### Author

* Frost :snowflake:

### Reference

* [Portswigger - Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)
* [Hacktricks - SSTI (Server Side Template Injection)](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)

