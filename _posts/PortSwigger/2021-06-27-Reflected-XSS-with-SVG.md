---
title: PortSwigger Labs - Reflected XSS with some SVG markup allowed
date: 2021-06-27 11:57:00 +0200
categories: [PortSwigger-Labs]
tags: [xss, reflected-xss, svg]     # TAG names should always be lowercase
dirImg: /assets/img/PortSwigger/Reflected-XSS-with-SVG
---

## Description

Link: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed

>![]({{ page.dirImg }}/20210627-231828.png){: .shadow}

## Writeup

We know the `svg` tag is going to be successful.

We try `<svg onload=alert(1)` but the `onload` event is blacklisted.

What we can do is fuzzing the event attribute to figure out which event is allowed.  

We can do it using Burp intruder.  
The request we crafted is 

>![]({{ page.dirImg }}/20210627-232934.png){: .shadow}

Our dictonary is a wordlist file with all the `events`.

Running Intruder we get to know that the `onbegin` event is whitelisted:

>![]({{ page.dirImg }}/20210627-233127.png){: .shadow}


We can exploit it and craft our XSS payload. We try a couple of different payloads, then we find the right one using the `animatetransform` tag:

```
<svg><animatetransform onbegin=alert(document.domain) attributeName=transform>
```

>![]({{ page.dirImg }}/20210627-235535.png){: .shadow}



