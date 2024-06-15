# Javascript Object and Inheritance

## What is javascript object?

Javascript object is like a class in java.

**Example Create a basic object about human**

```javascript
// create a empty object
const person = {}; 

// Add properties
person.firstName = "john";
person.lastName = "Doe";
person.age = "50";
person.eyeColor = "blue";

// Access the properties via dot notation
person.firstName
'john'

// Access the properties via bracket notation
person['firstName']
'john'
```

Or we can create object using the curly brace syntax to explicitly declare its properties and their initial values.

As well as data, properties may also contain executable functions. In this case, the function is known as a "method".

```javascript
const person =  {
    firstName: "john",
    age: 50,
    exampleMethod: function(){
        // do something
    }
}
```

## What is prototype in Javascript

Javascript will automatically assign new object its built-in prototype. For examples strings are assigned the built-it `String prototype`.  Since it inhereit the `String prototype` we can also access all the strings methods

E.g.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Same goes to other type of data

```
let myArray = [];
Object.getPrototypeOf(myArray);	    // Array.prototype

let myNumber = 1;
Object.getPrototypeOf(myNumber);    // Number.prototype
```

Objects automatically inherit all the properties of their assigned prototype, unless they already have their own property with the same key. This enables developers to create new objects that can reuse the properties and methods of existing objects

The built-in prototypes provide useful properties and methods for working with basic data types. For e.g. `String.prototype` object has a `toLowerCase()` methods. As a result, all strings automatically have a read-to-use method for converting them to lowercase.

## Prototype chain

An object's prototype is just another object, which should also have its own protype, and so on.

As virtually everything in JavaScript is an object under the hood, this chain ultimately leads back to the top-level `Object.prototype`, whose prototype is simply `null`.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption><p>Example chain</p></figcaption></figure>

Simplies means, `remainingStock` can use all the methods in `Number.prototype` and all the methods in `Object.prototype`, it inherit all the properties.

### Accessing an object's prototype using \_\_proto\_\_ <a href="#accessing-an-object-s-prototype-using-proto" id="accessing-an-object-s-prototype-using-proto"></a>

Every object has a special property that you can use to access its prototype. Although this doesn't have a formally standardized name, `__proto__` is the de facto standard used by most browsers. If you're familiar with object-oriented languages, this property serves as both a getter and setter for the object's prototype. This means you can use it to read the prototype and its properties, and even reassign them if necessary.

As with any property, you can access `__proto__` using either bracket or dot notation:

`username.__proto__ username['__proto__']`

You can even chain references to `__proto__` to work your way up the prototype chain:

```
username.__proto__                        // String.prototype
username.__proto__.__proto__              // Object.prototype
username.__proto__.__proto__.__proto__    // null
```

## Author

[ik0nw](https://github.com/ik0nw)

## Interview Question

1\) How do you create an object in JavaScript? Can you demonstrate with an example where you define an object representing a 'Car' with properties like 'make', 'model', and 'year'?

2\) What are the differences between dot notation and bracket notation when accessing or setting the properties of a JavaScript object?

3\) How do JavaScript’s prototypes compare to classes in other languages like Java or C++?

4\) How to you access object's prototype?

## Reference

{% embed url="https://portswigger.net/web-security/prototype-pollution/javascript-prototypes-and-inheritance" %}

{% embed url="https://www.w3schools.com/js/js_object_prototypes.asp" %}
