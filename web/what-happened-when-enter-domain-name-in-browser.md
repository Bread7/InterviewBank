# What happened when enter domain name in browser

This is a class interview question that tests a comprehensive understanding of how network and computer systems work. Here's the process from typing a facebook.com to browser finishing loading the page:

### User key in address

When user key in **facebook.com** in browser, browser will first check if the content is a complete URL or a search query （wont explain how browser know its query or URL)

Since facebook.com is a domain, it will first look out for the IP

### Domain Name resolution

Browser will for local, OS or even router cache to find facebook.com IP

If all these cache does not have IP, browser will send query to the DNS for resolution&#x20;

Here is the order:

1\) Check browser cache: Some browser might cache frequently visited sites IP

2\) Check Operating system cache: OS have a local DNS cache, for previous result

3\) Check local DNS: if None of the above works, the OS will send a query to DNS (Normally is provided by ISP)

4\) If the Local DNS does not have result, the DNS will query to the root DNS,  the root DNS will query to the TLD (Top level Domain) server, TDL will then query to the **facebook.com** authorized DNS server.

#### CDN

If gmail uses CDN, DNS might not resolve the IP address pointed to the actually Server, but to one of the EDGE server's IP address. CDN will allocate user's geolocation and chose the nearest edge server to provide the service, reduce the latency and boost the loading speed

### TCP Handshake

After Browser got the IP address, it will proceed with a TCP 3 way handshake communication:

1\) SYN: Browser sends a SYN (syncronice) packet, requesting for a connection

2\) SYN-ACK: Server receive the request, replied with a SYN-ACK(Syncronice acknowledge) packet, agreed with the connection

3\) ACK: Browser sent a ACK packet, start the connection

### HTTP/HTTPS request

After connection, browser will sent a GET request to `gmail.com`, but since its a HTTPS by default.

Here is the detailed TLS/SSL handshake process:

#### 1) Client HELLO

Browser will sent a Client Hello packet, the packet have the following information:

* Supported TLS version (e.g. TLS 1.2 or TLS 1.3)
* Supported cipher suites
* Generate a random keys&#x20;
* Client supported extenstions like SNI

#### 2) Server HELLO

After server receive Client Hello, respond with a Server Hello packet:

* Server TLS version
* Server cipher suit
* Server's random keys
* Server supported exenstions

#### 3) Server sends server certificate

Server will send client it's digital certificate, the certificate include server's public key and certified authority (CA)'s signature, then the server will verify this digital certificate validity based on following:

* If the DC is from authorized CA
* is the DC within valid period
* is the DC's domain name same as the one we requesting (facebook.com in this case)

#### 4) Exchange keys

ok i dont like crypto, but bascially they uses their own private keys and opponment public keys to generate a symetric keys(ECDHE or DHE) for secure communication&#x20;

#### 5) Client Finished

Client will send a Finished message telling server the handshake for client side is done, this message wil be sent using the encrypted keys from previous step.

And client will also sent an Change cipher spec packet, tell server the future communicates will be encrypted

#### 6) Server finished

Same as client side, sent a finish and change cipher spec

Now here mark the end of TLS/SSL handshake we will be back to the topic

### Server response

When server receive the browser request, it will response to the coding logic of the main page and return the resources needed.

### **Browser Rendering Process**

After the browser receives the HTML content, it begins to parse and render the page. The rendering process includes the following steps:

1. **Parsing HTML**: The browser converts the HTML into a DOM tree (Document Object Model).
2. **Parsing CSS**: The browser converts the CSS into style rules and applies them to the DOM tree, forming a render tree.
3. **Executing JavaScript**: If the page contains JavaScript, the browser will execute these scripts, which may dynamically modify the DOM or CSS.
4. **Layout and Painting**: The browser calculates the position and size of each element based on the structure of the render tree and then paints the page onto the screen.
