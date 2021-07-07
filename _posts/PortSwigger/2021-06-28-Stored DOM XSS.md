---
title: PortSwigger Labs - Stored DOM XSS
date: 2021-06-28 01:34:00 +0200
categories: [PortSwigger-Labs]
tags: [xss, stored-xss, javascript]     # TAG names should always be lowercase
dirImg: /assets/img/PortSwigger/Stored DOM XSS/
---


## Description

Link: https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored

![]({{ page.dirImg }}/20210628-002830.png){: .shadow}


## Writeup

A comment feature is implemented within each post in the page

![]({{ page.dirImg }}/20210628-012711.png){: .shadow}

We can test each field for XSS, starting with the simple `<scriptalert(1)</script`.

![]({{ page.dirImg }}/20210628-003548.png){: .shadow}

Some filtering is in place.

We can study the source code of the page to see whether the escaping is performed client side.  
Something interesting pops up:

![]({{ page.dirImg }}/20210628-005710.png){: .shadow}

Zooming into the javascript file we can analyze the code: Some HTML characters are escaped

![]({{ page.dirImg }}/20210628-010104.png){: .shadow}

And this is what the function `escapeHTML` does

![]({{ page.dirImg }}/20210628-010145.png){: .shadow}

It sanitizes the characters `<` and `` by using the `replace()` function. However such function will only replace the first instance of the value.

We can easily bypass the escaping by prepending  `<` to our payload.

However `< <scriptalert(1)</script` doesn't seem to work.  
At this point we can try a bunch of different payloads and see whether the XSS is triggered or not.

Payload `<<img src=q onerror=alert(1)` is successful.

![]({{ page.dirImg }}/20210628-011129.png){: .shadow}

