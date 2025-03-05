---
description: Part 2 of Prototype pollution
---

# Prototype Pollution - Part 2

Previously we went through how objects and prototype works in JavaScript and a high level overview on how an attacker can pollute the prototype of object in JavaScript.

## Brief Recap

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption><p>A typical Prototype Pollution attack type Credits: PortSwigger</p></figcaption></figure>

In order for prototype pollution vulnerability to work, the merge operation must be recursive, this operation will allow the user controlled properties to be assigned to the object's **prototype** instead of the object itself.&#x20;

As I mentioned in Part 1, to successfully exploit prototype pollution, you need the following key components

* A source - Any **user input** that allows you to **poison** the prototypes with arbitrary properties.
* A sink - A JavaScript function or DOM element that **execute** arbitrary code.
* An exploitable gadget - A property that is passed into a sink without **proper filtering**.

I will be diving more in depth into these three key components

## Prototype pollution source

As mentioned, a prototype pollution source is any user input that enables a user to add arbitrary properties to prototype objects. These are some of the most common type of sources:

* URL query via a GET request&#x20;
* JSON-based input via a POST request
* Web messages

I will be going through URL query and JSON-based input.

### Prototype pollution via URL

One can poison the prototype via URL in such a manner:

```http
https://greenhat.com/?__proto__['evilProperty']=payload
```

We might think that the URL parser may interpret `__proto__` as an arbitrary string. But as we mentioned before, if we merge these keys and values into an existing object as properties.

{% code title="What we think might happen when we do a recursive merge" overflow="wrap" %}
```json
{
    currentProperty1: 'test',
    currentProperty2: 'test2',
    __proto__: {
        evilProperty: 'payload'
    }
}
```
{% endcode %}

{% code title="What actually happen" %}
```javascript
targetObject.__proto__.evilProperty = 'payload';
```
{% endcode %}

So, how did this happen? The JavaScript engine actually treat `__proto__` as a getter for the prototype. As a result, the `evilProperty` is assigned to the prototype rather than the target object itself. Now all objects that use the same prototype of the `targetObject` will inherit the `evilProperty`.&#x20;

### Prototype pollution via JSON input

User-controllable objects are often derived from a JSON string using the `JSON.parse()` method. `JSON.parse()` also treats any key in the JSON object as an arbitrary string, including things like `__proto__`.&#x20;

{% code title="A typical malicious JSON " %}
```json
{
    "__proto__": {
        "evilProperty": "payload"
    }
}
```
{% endcode %}

Using `JSON.parse()` method, if we convert the JSON object into a JavaScript object, the resulting object will have a property with the key `__proto__`. If this object is merged into an existing object without proper key sanitization, it will lead to prototype pollution.

## Prototype pollution sinks



## Prototype pollution gadgets

