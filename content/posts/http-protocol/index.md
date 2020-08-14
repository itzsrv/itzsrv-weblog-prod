---
title: Brief insights about HTTP and Internet Protocol Suite (TCP/IP)
cover: 
date: 2020-08-01
description: 
tags: ['post']
draft: false
---

HTTP, acronym for **HyperText Transfer Protocol**, is the protocol clients and servers use on the web to communicate. The client sends an **HTTP request**, and the server answers with an **HTTP response**. It's a network protocol that has Web-specific features, but it depends on **TCP/IP** (Internet Protocol Suite) to get complete request and response from one place to another. 

A brief insight about **TCP** is, it's responsible for making sure that a file sent from one network node to another ends up as a complete file at the destination, even though the file is split into chunks when it's sent.

**IP** is the underlying protocol that routes the chunks (packets) from one host to another on their way to destination.

Development of HTTP was initiated by Tim Berners-Lee at CERN in 1989. Development of early HTTP Requests for Comments (RFCs) was a coordinated effort by the Internet Engineering Task Force (IETF) and the World Wide Web Consortium (W3C), with work later moving to the IETF.

[Roy Fielding](https://roy.gbiv.com) maintains the IETF working group list of HTTP RFCs and Internet Drafts. For in-depth reference of HTTP1.1, refer [RFC 7230](https://tools.ietf.org/html/rfc7230) family (RFC 7230-7235).

HTTP versions and corresponding RFCs
 - HTTP v1.0    [RFC 1945](https://tools.ietf.org/html/rfc1945)
 - HTTP v1.1    [RFC 2616](https://tools.ietf.org/html/rfc2616)    
 - HTTP v1.1    [RFC 7230](https://tools.ietf.org/html/rfc7230), [RFC 7231](https://tools.ietf.org/html/rfc7231), [RFC 7232](https://tools.ietf.org/html/rfc7232),  [RFC 7233](https://tools.ietf.org/html/rfc7233), [RFC 7234](https://tools.ietf.org/html/rfc7234), [RFC 7235](https://tools.ietf.org/html/rfc7235)
 - HTTP/2   [RFC 7540](https://tools.ietf.org/html/rfc7540)
 - HTTP/3   [Proposed Internet Draft v29](https://tools.ietf.org/html/draft-ietf-quic-http-29)       

>
##### Sources: 
>
- [IETF Tools | Internet Engineering Task Force](https://tools.ietf.org)
- [https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
>
Few More Important Resources that can be checked out:
  - [IETF Upcomings](https://www.ietf.org)



