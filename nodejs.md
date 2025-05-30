# Node JS

[node js tutorial](https://www.youtube.com/playlist?list=PL78RhpUUKSwfeSOOwfE9x6l5jTjn5LbY3)

---

## Table of Contents

- [Chapter 1: Introduction to Node js](#chapter-1--introduction-to-node-js)
- [Chapter 2: Setting Up the Environment](#chapter-2-setting-up-the-environment)
- [Chapter 3: How network request works ](#chapter-3-how-network-request-works)
- [chapter 4: request/response](#chapter-4-requestresponse) 
- [chapter 5: parsing request](#parsing-request)


## Chapter 1 : Itroduction to Node js

Node.js is an open-source and cross-platform JavaScript runtime environment. Node.js runs the V8 JavaScript engine, the core of Google Chrome, outside of the browser.

- windows browser objects linke BOM , DOM , cache, cookies does not work with Node.js as it is outside of browser, so browser properties does not work with it.

[read more](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)


---

## Chapter 2: Setting Up the Environment

- Installing Node.js & npm
- Checking versions (`node -v`, `npm -v`)
- To change node version use `nvm install 20.12.0` then `nvm use 20.12.0`

[🔝 Back to Top](#node-js)

---

## Chapter 3: How network request works

_How a Web Request Works: From DNS Lookup to Server Response_

### 1. DNS Lookup (Domain Name Resolution)

- **What is DNS?**  
  The Domain Name System (DNS) translates human-readable domain names (like `example.com`) into machine-readable IP addresses (like `93.184.216.34`).

- **How does DNS lookup work?**

  1. **Browser cache check:**  
     The browser first checks if it has recently resolved the domain and stored the IP (DNS cache). If yes, it uses this cached IP address.

  2. **Operating system cache:**  
     If not in browser cache, the OS cache is checked.

  3. **Router cache:**  
     The local network router may have cached the IP.

  4. **Recursive DNS Resolver:**  
     If caches miss, the request goes to a DNS resolver provided by your ISP or a public DNS (like Google DNS `8.8.8.8`).

  5. **Root DNS servers:**  
     The resolver asks the root DNS servers for the Top-Level Domain (TLD) server (like `.com`).

  6. **TLD DNS servers:**  
     The resolver asks the TLD server for the authoritative DNS server for the domain.

  7. **Authoritative DNS server:**  
     Finally, the authoritative server returns the IP address for the domain.

- **Caching:**  
  Each step caches the result for a Time-To-Live (TTL) period to speed up future requests.

### 2. Establishing a Connection

- **TCP Handshake (3-way handshake):**

  To communicate with the server, your browser establishes a TCP connection:

  1. **SYN:** Client sends a SYN packet to server to start connection.
  2. **SYN-ACK:** Server replies with SYN-ACK to acknowledge.
  3. **ACK:** Client sends ACK to confirm the connection is established.

- **TLS/SSL Handshake (if HTTPS):**  
  If the site uses HTTPS, a secure encrypted connection is established after TCP handshake using TLS/SSL protocols.

### 3. Sending the HTTP Request

- Browser sends an HTTP request to the server with:
  - **Request method:** GET, POST, etc.
  - **URL path:** `/index.html`, `/api/data`
  - **Headers:** Browser info, cookies, accepted data types, etc.
  - **Body:** For POST/PUT, data sent to server.

### 4. Server Processing the Request

- Server receives the request and processes it by:
  - Running backend code (Node.js, PHP, etc.)
  - Querying databases if needed
  - Generating HTML, JSON, or other responses

### 5. Server Sends HTTP Response

- Server sends back a response containing:
  - **Status code:** 200 OK, 404 Not Found, 500 Server Error, etc.
  - **Response headers:** Content-Type, Cache-Control, Cookies, etc.
  - **Body:** The requested resource (HTML, JSON, images, etc.)

### 6. Browser Processes Response

- The browser receives the response and:
  - Parses HTML, CSS, JavaScript
  - Renders the webpage
  - Executes scripts
  - Makes additional requests for images, CSS files, JS files, etc.

### 7. Caching in Browser & CDN

- Browsers cache responses based on headers (e.g., Cache-Control).
- Content Delivery Networks (CDNs) cache static files geographically closer to users to speed up delivery.

### Summary Flow

| Step                       | Description                                     |
| -------------------------- | ----------------------------------------------- |
| 1. DNS Lookup              | Domain → IP resolution with caching             |
| 2. TCP (and TLS) Handshake | Establish connection between client and server  |
| 3. HTTP Request            | Client sends request with method, headers, body |
| 4. Server Processing       | Server runs logic, fetches data                 |
| 5. HTTP Response           | Server sends status, headers, body              |
| 6. Browser Render          | Browser parses response and renders page        |
| 7. Caching                 | Response caching to optimize future requests    |

![DNS Lookup image](https://i.ibb.co/DDRf30Pq/DNS-lookup.webp)

## ![after DNS http handshak](https://i.ibb.co/qMQ3p8zc/afterdns.jpg)

[🔝 Back to Top](#node-js)

---


## chapter 4: request/response

 Takes user input and submit the data to payload  and redirects to '/' . also create a submittedinfo.txt file.
```js
//

import { createServer } from "node:http";
import { writeFileSync } from "node:fs";

const hostname = "127.0.0.1";
const port = 3000;

const server = createServer((req, res) => {
  if (req.url === "/") {
    res.setHeader("Content-Type", "text/html");
    res.write("<html>");
    res.write("<head><title>Form Submission</title></head>");
    res.write("<body>");
    res.write("<h1>Enter Your Details</h1>");
    res.write('<form method="POST" action="/submit">');
    res.write('<label for="name">Name:</label>');
    res.write('<input type="text" id="name" name="username ">');
    res.write(' <br><label for="male">  Male </label>');
    res.write('<input type="radio" id="male" name="gender" value = "male">');
    res.write('<br> <label for="female"> Female </label>');
    res.write('<input type="radio" id="female" name="gender" value="female">');
    res.write('<br>  <input type = submit value = "Submit">');
    res.write("</form>");
    res.write("</body>");
    res.write("</html>");
    return res.end();
  } else if (req.url.toLowerCase() === "/submit" && req.method == "POST") {
    writeFileSync(
      "submittedinfo.txt",
      "file is created after submit   request, check data on network payload"
    );
    res.statusCode = 302;
    res.setHeader("Location", "/");
    return res.end();
  }
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

```
[🔝 Back to Top](#node-js)

---

## chapter 5: parsing request

#### How Data Comes in Streams 

In Node.js, when a client sends data (like in a POST request), it doesn't arrive all at once. Instead, it's streamed in small **chunks** of binary data.

These chunks are temporarily stored in a **buffer** — a special memory area for raw binary data. Node.js listens for these chunks using:

```js
req.on('data', chunk => {
  // each chunk is a part of the full data
});
```

 Once all chunks are received, the end event is triggered:
```js
req.on('end', () => {
  // all data is now available, combine chunks here
});

```



[🔝 Back to Top](#node-js)