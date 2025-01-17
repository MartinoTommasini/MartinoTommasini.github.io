---                                                                                           
title: PortSwigger Labs - Business logic - Missing validation of negative quantities                          
date: 2021-07-14 21:12:00 +0200                                                              
categories: [PortSwigger-Labs ]                                                                
tags: [business-logic ]     
dirImg: /assets/img/PortSwigger/Business-logic-negative-quantities                     
toc: false                                                                                    
---

## Description
Link: https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-high-level

>![]({{ page.dirImg }}/2021-07-14-19-42-49.png){: .shadow}

## Writeup

We move through the website while recording the HTTP traffic using Burp.

When an item is added to the cart, the request looks like this one:

>![]({{ page.dirImg }}/2021-07-14-19-54-05.png){: .shadow}

The item's **productID** and **quantity** are specified in the POST body and sent to the server (The `productId=1` represents our `Lightweight l33t leather jacket`).

We change the **quantity** to -100. Since quantity is not properly validated, our cart will have a negative value. trying to place the order for that cart, we get an error:

>![]({{ page.dirImg }}/2021-07-14-20-02-19.png){: .shadow}

The server checks for the cart's total and throws an error if the quantity is negative.  
However, since we can add negative quantities for items, we can use them to make the cart's value lower (while keeping it positive). In this way we are able to afford our dream jacket.

>![]({{ page.dirImg }}/2021-07-14-20-09-54.png){: .shadow}

Placing the order, we manage to buy a 1337 dollar jacket for 33.11 dollars (we could lower the price up to 0.01 if we really want to save money :) )

>![]({{ page.dirImg }}/2021-07-14-20-12-05.png){: .shadow}
