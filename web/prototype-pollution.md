---
description: Part 1 of exploring the Prototype Pollution vulnerability.
---

# Prototype Pollution

## What is Prototype Pollution?

Prototype Pollution is a JavaScript vulnerability that allows attackers to add arbitrary properties to global object prototypes which may then be inherited by user-defined objects.

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption><p>A typical example of Prototype Pollution, Credits: PortSwigger</p></figcaption></figure>

Just to note, Prototype Pollution **cannot** be **exploited** as a **standalone** vulnerability. In fact, when it was discovered, many security researchers thought that the way that it can be exploited is very theoretical and it won't happen in reality but history has proven them wrong :cry:. So why do researchers feel that it is theoretical? It is because Prototype Pollution allows attacker to control properties of objects that would otherwise be inaccessible. **However**, if the application handles an attacker-controlled property in an **unsafe** way, Prototype Pollution could be chained with other vulnerabilities and it can lead to deadly consequences! If it is client-slide, it will usually lead to a DOM XSS vulnerability. If it is server-side, it can lead to **remote code execution (RCE)**.

## So how does Prototype Pollution works?

To understand how it works, we need to be familiar with how prototype and inheritance work in JavaScript. So Imma give a crash course.

### JavaScript prototypes and inheritance

Unlike Java, JavaScript uses a prototype inheritance model which works differently from the class-based model which we are used to in languages like C#, Java or Golang. Both are different styles of Object-Oriented Programming (OOP). Below is a table that shows the differences:

<table><thead><tr><th width="154">Feature</th><th>Prototype Inheritance Model</th><th>Class-Based Model</th></tr></thead><tbody><tr><td>Definition</td><td>Objects inherit directly from other objects aka prototypes.</td><td>Objects are instances of classes that define a blueprint.</td></tr><tr><td>Programming Languages</td><td>JavaScript.</td><td>Java, C#, Golang and many others.</td></tr><tr><td>Inheritance Mechanism</td><td>Use object prototypes where objects inherit properties directly from other objects.</td><td>Uses classes and instances, where objects are instantiated from a class.</td></tr><tr><td>Object Creation</td><td>Objects are created by cloning other objects.</td><td>Objects are instantiated from class templates.</td></tr><tr><td>Method Sharing</td><td>Methods are shared via the prototype chain.</td><td>Methods belong to the class and are inherited by instances.</td></tr><tr><td>Encapsulation</td><td>Less enforced; any object can modify its prototype chain.</td><td>More structured, with class-based encapsulation mechanisms.</td></tr></tbody></table>

### Object in JavaScript

A JavaScript object is essentially just a collection of `key:value` pairs known as "properties". It is similar to JSON.  Properties can include data and as well as executable functions. This function can also be known as a method.

```javascript
const user = {
    username: "frost",
    userId: 1,
    isAdmin: false,
    exampleMethod: doFunction() {
        // something 
    }
}
```

To access these properties we can use the following:

```javascript
// dot notation 
user.username

// bracket notation
user["username"]
```

### How does object inheritance work in JavaScript?

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption><p>Credits: PortSwigger </p></figcaption></figure>

When you reference a property of an object, the JavaScript engine will first try to find it in the object. If it is not there, it will look for it in the object's prototype. In this example from the picture above, I can reference `myObject.propertyA` .

### Prototype in JavaScript

Every object in JavaScript is linked to another object of some kind, this is known as its prototype. Since JavaScript is a prototype inheritance model, every  new object will be automatically assigned to one of JavaScript's built-in prototypes. For example, strings are automatically assigned to the built-in `String.prototype`.&#x20;

```javascript
let myObject = {};
Object.getPrototypeOf(myObject); // Returns Object.prototype

let myString = "";
Object.getPrototypeOf(myString); // Returns String.prototype

let myNumber = 1;
Object.getPrototypeOf(myNumber); // Returns Number.prototype
```

As we know, these objects will automatically inherit all of the properties of their assigned prototype, unless they already have their own property with the same key. This helps developers to create new objects that can be reuse the properties and methods of existing objects.

### How can we access an object's prototype?

Each object has a special property that you can use to access its prototype. We can use \_\_proto\_\_! \_\_proto\_\_ serves as both the getter and setter for the object's prototype. This means that you can use it to read the prototype and even change it!

```javascript
username.__proto__
username['__proto__']
```

We can even chain the prototype of an object!

```javascript
username.__proto__                            // String.prototype
username.__proto__.__proto__                  // Object.prototype
username.__proto__.__proto__.__proto__        // null
```

### Modifying an object's prototype

Since&#x20;

