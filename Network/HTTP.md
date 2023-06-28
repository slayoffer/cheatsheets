## What is HTTP?

HTTP (Hypertext Transfer Protocol) is the way in which data is communicated on the world-wide web (not to be confused with the Internet, [there is a difference](https://www.bbc.co.uk/newsround/av/47523993)). There are 3 major versions, and we'll run through each of them here.

Keeping it simple, the protocol defines how a browser speaks to a server to pass data back and forth. In all versions of the protocol the browser will ask for something (a "request") and the server will send back data (the "response"). It's a simple concept, but things have got more complex over time, and as web developers it's important we understand how the later versions of the protocol differ.

## HTTP/1.x

Released circa 1996, the first version was HTTP/1.0. HTTP/1.1 was released in 1999 and quickly got adopted.

HTTP/1.1 is a pretty simple concept. Let's look at how a request would work:

1.  You navigate to a website, let's say [accreditly.io](https://accreditly.io/) in this example (for a shameless plug). Your browser sends a `GET` request to the server:

```
GET accreditly.io
```

The server receives the request and response with the document:

```
200 OK

<html>
  <head>
    <title>Accreditly - Web Development Certifications</title>
    [...]
```

In that document are likely various assets. Things like images, stylesheets (CSS files) and JavaScript assets (.js files). Your browser reads the document and repeats steps 1 and 2 for each asset in a queue.

It's a simple flow that's pretty easy to understand, but there are a few problems here:

a. Head-of-line blocking: Each HTTP/1.x connection could handle only one request at a time. This limitation often led to inefficient use of network resources, as subsequent requests had to wait for the previous request to complete.

b. Lack of prioritization: HTTP/1.x did not offer a way to prioritize requests, which could lead to less critical resources blocking more important ones.

c. There are other problems, such as plain text headers being sent that are large, especially when cookies are in use.

All of these issues have a large performance impact, especially on the modern web.

## HTTP/2

In comes HTTP/2, released in 2015. The goal of HTTP/2 was to address the issues raised above. The web has changed over the 16 years that HTTP/1.1 was king, people have mobile devices on less stable 4G connections, landline connections have huge throughput capabilities compared to those in the 90s (dial-up, anyone?), but websites are also huge in comparison to what they used to be.

The average size of a web page is now huge in comparison to what it was. Additionally, web pages are now loading assets at a rate that is growing far faster than the speed of our connections are improving. According to HTTP Archive the average size of an image on a web page grew over 8,000%, and so did the number of images, and that's before we even get onto JavaScript and other assets. It's a big problem.

So how does HTTP/2 help?

There are a number of features introduced in HTTP/2, with the main benefit that people focus on being **multiplexing**.

### Multiplexing

Look at our example above in HTTP/1.1, where your browser requests a document with multiple assets within it. In HTTP/1.1 those asset requests are queued and requested one at a time.

In HTTP/2 that works a little differently. Using multiplexing, the browser effectively requests the assets together, and then receives them in the same way, all on the same connection.

Take a look at the following diagram:

### Other benefits

In addition to multiplexing HTTP/2 also implements some other features, focussing on performance:

1.  Header compression: HTTP/2 uses the HPACK algorithm to compress request and response headers, significantly reducing the amount of data transmitted.
2.  Server push: With HTTP/2, servers can proactively push resources to the client's cache before they are requested, reducing latency and improving the overall user experience.
3.  Stream prioritization: HTTP/2 enables clients to prioritize requests, allowing more critical resources to be fetched and rendered first.
4.  Binary framing: HTTP/2 uses a binary framing layer to encapsulate messages, which makes the protocol more efficient and less error-prone compared to the plain-text approach of HTTP/1.x.

HTTP/2 relies on the same underlying protocol in order to operate: TCP. This is both a positive and a negative. Because TCP is used by HTTP/1.x already it means adoption is much easier; browsers don't need to implement a new underlying protocol, and servers can continue operating as they are now with a few tweaks to implement the HTTP/2 features. The downside is that there are issues with TCP, especially in high-latency and lossy networks.

## Introducing HTTP/3

HTTP/3 does away with TCP, and instead utilises a flavour of UDP called **Quick UDP Internet Connections**, or 'QUIC' (see what they did there?).

QUIC has a few benefits:

1.  Built-in encryption: QUIC incorporates Transport Layer Security (TLS) 1.3 by default, ensuring a secure connection without the need for a separate TLS handshake. This reduces latency and improves connection establishment time.
2.  Reduced head-of-line blocking: Unlike TCP, QUIC handles packet loss at the individual stream level. This means that the loss of a single packet does not block the entire connection, further reducing head-of-line blocking issues.
3.  Connection migration: QUIC is designed to better support connection migration, allowing clients to change IP addresses without losing connectivity or incurring additional latency. Something that users on mobile/celluar connections will benefit from in particular.
4.  0-RTT connection establishment: QUIC enables 0-RTT (zero round trip time) connection establishment in certain situations, which can significantly reduce latency when connecting to a previously visited server.
5.  Improved congestion control: QUIC offers more advanced congestion control mechanisms, allowing it to better adapt to varying network conditions and improve overall performance.

HTTP/3 and QUIC sounds great, right?

Well, yes, in theory. The problem is that there's a big task for browsers and providers to implement these features, not to mention compatibility issues with network infrastructure.

Many large networks don't support UDP at all, so piping traffic around on a new protocol that sits on top of UDP is potentially a way off.

That said, big players like [Cloudflare](https://www.cloudflare.com/en-gb/learning/performance/what-is-http3/) are making big leaps forward in offering HTTP/3 support for their customers, and browsers such as Chrome, Edge and Firefox also have support. You may notice Safari isn't in that list; Apple are usually late to the party in offering support for new features and protocols, and often implement their own version of things. According to [CanIUse](https://caniuse.com/http3), Safari has partial support for HTTP/3, and it's only available for some users. Bear in mind that HTTP/3 isn't officially released yet though, so things could change.