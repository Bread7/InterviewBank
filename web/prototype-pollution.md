---
description: Part 1
---

# Prototype Pollution

## What is Prototype Pollution?

Prototype Pollution is a JavaScript vulnerability that allows attackers to add arbitrary properties to global object prototypes which may then be inherited by user-defined objects.

Just to note, Prototype Pollution **cannot** be **exploited** as a **standalone** vulnerability. In fact, when it was discovered, many security researchers thought that the way that it can be exploited is very theoretical and it won't happen in reality but history has proven them wrong :cry:. So why do researchers feel that it is theoretical? It is because Prototype Pollution allows attacker to control properties of objects that would otherwise be inaccessible. **However**, if the application handles an attacker-controlled property in an **unsafe** way, Prototype Pollution could be chained with other vulnerabilities and it can lead to deadly consequences! If it is client-slide, it will usually lead to a DOM XSS vulnerability. If it is server-side, it can lead to **remote code execution (RCE)**.

## So how does Prototype Pollution works?

To understand how it works, we need to be familiar with how prototype and inheritance work in JavaScript. So Imma give a crash course.

### JavaScript prototypes and inheritance

Unlike Java, JavaScript uses a prototype inheritance model which works differently from the class-based model which we are used to in languages like C#, Java or Golang. Both are different styles of Object-Oriented Programming (OOP). Below is a table that shows the differences:

<table><thead><tr><th width="154">Feature</th><th>Prototype Inheritance Model</th><th>Class-Based Model</th></tr></thead><tbody><tr><td>Definition</td><td>Objects inherit directly from other objects aka prototypes.</td><td>Objects are instances of classes that define a blueprint.</td></tr><tr><td>Programming Languages</td><td>JavaScript.</td><td>Java, C#, Golang and many others.</td></tr><tr><td>Inheritance Mechanism</td><td>Use object prototypes where objects inherit properties directly from other objects.</td><td>Uses classes and instances, where objects are instantiated from a class.</td></tr><tr><td>Object Creation</td><td>Objects are created by cloning other objects.</td><td>Objects are instantiated from class templates.</td></tr><tr><td>Method Sharing</td><td>Methods are shared via the prototype chain.</td><td>Methods belong to the class and are inherited by instances.</td></tr><tr><td>Encapsulation</td><td>Less enforced; any object can modify its prototype chain.</td><td>More structured, with class-based encapsulation mechanisms.</td></tr></tbody></table>









