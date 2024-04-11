# Content Security Policy (CSP)

Content Security is a browser technology that provides a protection against attack such as cross-site scripting (XSS).&#x20;

It works by define the path or the source from which resources **can be loaded by the browser.**

So what the defination of these resource, it can be image, frame, media, plugin and javascript etc.... anything that loads by the browser.



## Mitigating Cross-site scripting

The main focus is to reduce XSS. XSS exploit the browser's trust in the content received from the server. Malicious scripts are executed by the victim's browser because **the browser trusts the source on the content.**&#x20;

**The main problem is that the BROWSER TURST WHATEVER CONTENT FROM SERVER.**



## How does CSP works?

The implementation of CSP is conducted through `response header` or **incorporating meta elements into the HTML page.**

### Specifying policy

We can use `content-security-Policy` HTTP header to specify policy like this&#x20;

```
Content-Security-Policy: policy
```

### Writing the policy

A policy is described using a series of policy directives, each of which describes the policy for a c**ertain resource type or policy area.**

We need to specify the policy for different resource type such as img, media, js. The policy is a suggestion to include a **default-src. If none of the tags are mentions, it will all fault to this \`default-sec\`**&#x20;

### Example common use cases

E.G. 1&#x20;

A web administrator wants all content come from the site's own origin (Excludes subdomains)

```
Content-Security-Policy: default-src 'self'
```

E.g. 2

A website administrator wants to allow content from a trusted domain and all its subdomains (it doesn't have to be the same domain that the CSP is set on.)

```
Content-Security-Policy: default-src 'self' example.com *.example.com
```

E.g. 3

A website administrator wants to allow users of a web application to include images from any origin in their own content, but to restrict audio or video media to trusted providers, and all scripts only to a specific server that hosts trusted code.

{% code overflow="wrap" %}
```
Content-Security-Policy: default-src 'self'; img-src *; media-src example.org example.net; script-src userscripts.example.com
```
{% endcode %}

More directive please refer to the

{% embed url="https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass#directives" %}

## Unsafe CSP rules

{% embed url="https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass#unsafe-csp-rules" %}

### Enabling reporting <a href="#enabling_reporting" id="enabling_reporting"></a>

In default, when CSP is trigger the page will show the script is refused to loaded due to CSP.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Chrome</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Firefox</p></figcaption></figure>

Default reports will not be enabled, it need to define the report-ending for headers (report-uri)

{% code overflow="wrap" %}
```
Content-Security-Policy: default-src 'self'; report-uri http://reportcollector.example.com/collector.cgi
```
{% endcode %}

For report syntax refer to&#x20;

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP#violation_report_syntax" %}

## Example interview question

Can you explain what Content Security Policy (CSP) is and why it's important for web security?

How does CSP help in mitigating cross-site scripting (XSS) attacks?

Can you explain the difference between `script-src`, `img-src`, and `default-src` directives?

How would you debug issues caused by a CSP policy blocking legitimate resources?

Can you explain the role of the `report-uri` directive in CSP and how you would use it in practice?

## Author

- [Chen Xing](https://github.com/Ik0nw)