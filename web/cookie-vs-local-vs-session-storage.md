# Cookie vs Local vs session Storage

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

## Session storage

Session storage objects can be accessed using the SessionStorage read-only property.  SessionStorage data is cleared when the page session ends.

A unique page session gets created once a document is loaded in a browser. Page sessions are vlid for only one tab at a time. Pages are only saved for the amount of time that the tab or the browser is open&#x20;

### Session storage has 4 methods

* sessionStorage.setItem(key,value)
* sessionStorage.getItem(key)
* sessionStorage.removeItem(key)
* sessionStorage.clear()

I guess no need explain, the method name is very clear.



## Local Storage

The stored data is stored across browser session. It allowed to store large data in client, and the data does not expires.  It is very similar with SessionStorage except the sessionstorage get clear after the tab is closed. Local storage persistence even after browser is closed.



## Cookie

Cookie is a small piece of data a server sends to a user's web browser. Cookies enable web application to store limited amounts of data and remeber state information.

When a new request is made, the browser usually sends previously stored cookies for the current domain back to the server within the HTTP Header.

### Updating cookies via Javascript

Example of creating and update cookies in javascript

```javascript
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
```

### Secure flag

A cookie with the `secure` attribute is only sent to the server with an encryptied request over an HTTPS protocol, its never sent with unsecured HTTP

### HttpOnly

A cookie with the HttpOnly attribute can't be modified by javascript, for example using `Document.cookie`. It can only be modified when it reaches the server.

### Cookie prefixes

The design of the cookie mechanism, a server can't confirm that a cookie was set from a secure origin or even tell where a cookie was originally set.

A vulnerable application on a subdomain can set a cookie with `Domain` attribute, which gives access to that cookie on all other subdomains.

As a defense in depth measure, use cookie prefixes to assert specific facts about the cookie:

* `__Host-`: If a cookie name has this prefix, it's accepted in a [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) header only if it's also marked with the `Secure` attribute, was sent from a secure origin, does _not_ include a `Domain` attribute, and has the `Path` attribute set to `/`. In other words, the cookie is _domain-locked_.
* `__Secure-`: If a cookie name has this prefix, it's accepted in a [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) header only if it's marked with the `Secure` attribute and was sent from a secure origin. This is weaker than the `__Host-` prefix.

The browser will reject cookies with these prefixes that don't comply with their restrictions. This ensures that subdomain-created cookies with prefixes are either confined to a subdomain or ignored completely. As the application server only checks for a specific cookie name when determining if the user is authenticated or a CSRF token is correct, this effectively acts as a defense measure against session fixation.

## Interview Question

1\) If you duplicate a tab, what happen to the SessionStorage?

2\) Cookie with secure flag is allowed sent to localhost?

3\) Give one scenario usage of session, local and cookie storage each.

4\) Where should CSRF token be stored? Any security features needed to set? Why?

5\) Is there potential danger on local and session storage? (Hint:CSP)
