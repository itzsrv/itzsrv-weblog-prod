---
title: Communication over the Web - "Web Clients and Web Servers"
cover: ./reverse-proxy.jpg
date: 2020-07-24
description: 
tags: ['post','server']
draft: false
---


The **WEB** consists of gazillions of clients and servers connected through wired and wireless networks. 

A web server takes a client request and gives a response back to the client. A web client enables user to request a resource kept over any web server. Most of the times this resource is an HTML page. Sometimes it's a picture, a PDF document or an audio file etc. 

#### Web Server
 
The web server gets the request from a client, finds the resource, formats the response attaching requested resource and returns it back to the client. 

The communication between web client and server is held using the **HTTP** protocol, which allows for simple request and response conversations. The client sends an HTTP request, and the server answers with an HTTP response, which contains the requested resource and set of instructions to present that resource.  

> If you’re a web server, you speak HTTP. 

#### Web Client

A web client (browsers like Mozilla or Safari) lets the user request something on the server, and shows the user the result of the request.

The browser is the piece of software (like Netscape and Mozilla) that knows how to communicate with the server. Browser's other big job is interpreting the HTML code and rendering the web page for the user.

When a server answers a request, the server usually sends some type of content to the browser so that the browser can display it. Servers often send the browser a set of instructions written in HTML with help of which the browser understands how to present the content to the user.

> If you're a web client, you speak HTTP and understand HTML.

To read further about HTTP specification, checkout [Brief insights about HTTP and Internet Protocol Suite (TCP/IP)](https://itzsrv.com/http-protocol/)

>
##### References: 
>
- Book: Head First Servlets and JSP






