---
description: Part 1 of exploring the Prototype Pollution vulnerability.
---

# Prototype Pollution - Part 1

## What is Prototype Pollution?

Prototype Pollution is a JavaScript vulnerability that allows attackers to add arbitrary properties to global object prototypes which may then be inherited by user-defined objects.

Just to note, Prototype Pollution **cannot** be **exploited** as a **standalone** vulnerability. In fact, when it was discovered, many security researchers thought that the way that it can be exploited is very theoretical and it won't happen in reality but history has proven them wrong :cry:. So why do researchers feel that it is theoretical? It is because Prototype Pollution allows attacker to control properties of objects that would otherwise be inaccessible. **However**, if the application handles an attacker-controlled property in an **unsafe** way, Prototype Pollution could be chained with other vulnerabilities and it can lead to deadly consequences! If it is client-slide, it will usually lead to a DOM XSS vulnerability. If it is server-side, it can lead to **remote code execution (RCE)**.

## Huh??? What is a prototype? What does it do? Why is there such a thing? :thinking:

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

#### Prototype Chain

As everything in JavaScript is an object, this chain leads back to the top level `Object.prototype`, whose prototype is a `null`.&#x20;

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption><p>Credits: PortSwigger</p></figcaption></figure>

Another thing we should take note of is that objects don't just inherit properties from their immediate prototype, they also inherit properties from all the objects above them in the prototype chain.&#x20;

#### How can we access an object's prototype?

Each object has a special property that you can use to access its prototype. We can use `__proto__`! `__proto__` serves as both the getter and setter for the object's prototype. This means that you can use it to read the prototype and even change it!

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

Since \_\_proto\_\_ is used as a getter and setter, it is possible to modify JavaScript's built-in prototypes. This also means that developers can customize or override the behaviour of built-in methods and even add new methods to perform useful operations.

## So how does Prototype Pollution works?

Prototype pollution vulnerabilities arise when a JavaScript function **recursively** merges an object that contains **user-controllable** properties into an existing object without **sanitizing** the keys! This allows attackers to inject a property with a key like `__proto__` along with arbitrary nested properties.

Because `__proto__` has a special meaning, the merge operation may assign the nested properties to the object's **prototype** instead of the target object itself! This allows the attacker to pollute the prototype with properties that contain values which can be used by the application in a dangerous way.&#x20;

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption><p>A typical Prototype pollution attack flow Credits: PortSwigger</p></figcaption></figure>

To exploit this successfully, we need a few key components:

* A prototype pollution source - This is any **input** that enables you to poison the prototype objects with arbitrary properties.
* A sink - a JavaScript function or DOM element that **enables** arbitrary code execution.
* An exploitable gadget - Basically any properties that is passed into a sink without **proper sanitization**.&#x20;

I will continue on how we can exploit Prototype Pollution in another Part 2 article.

## Interview Questions

* What is Prototype pollution?
* How does prototype-inheritance model contribute to the Prototype pollution vulnerability?
* How does the prototype chain relate to prototype pollution?

## Author

* Frost :snowflake:

## References

* [https://portswigger.net/web-security/prototype-pollution](https://portswigger.net/web-security/prototype-pollution)
* Offsec - Advanced Web Attacks and Exploitation&#x20;
