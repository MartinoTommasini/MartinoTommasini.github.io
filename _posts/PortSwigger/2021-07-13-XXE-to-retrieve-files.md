---                                                                                           
title: PortSwigger Labs - Simple XXE to retrieve sensible files                          
date: 2021-07-13 19:15:00 +0200                                                              
categories: [PortSwigger-Labs]                                                                
tags: [xxe]     
dirImg: /assets/img/PortSwigger/XXE-to-retrieve-files                     
toc: false                                                                                    
---

## Description

Link: https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-retrieve-files

>![]({{ page.dirImg }}/2021-07-13-18-45-00.png){: .shadow}

## Writeup

The website offers a `check stock` feature that displays the number of items of a specific product in one of the shops.  

Each product has an id, as well as each shop.

We can intercept the request using Burp

>![]({{ page.dirImg }}/2021-07-13-18-53-55.png){: .shadow}

The XML is passed to the server, processed and the response is displayed in the web page.

Since no protections are in place, we can load sensitive files using the external entities. We change the XML as follows:

>![]({{ page.dirImg }}/2021-07-13-19-03-36.png){: .shadow}

It will load the content of the `/etc/passwd` file in the `xxe` entity. The server will respond with an error message (since the product ID is not valid) and will print the content of `/etc/passwd` file:

>![]({{ page.dirImg }}/2021-07-13-19-07-24.png){: .shadow}

We successfully managed to perform an XXE attack to exfiltrate sensitive information.
