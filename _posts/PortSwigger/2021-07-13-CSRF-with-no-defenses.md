---                                                                                           
title: PortSwigger Labs - CSRF with no defenses                          
date: 2021-07-13 16:18:00 +0200                                                              
categories: [PortSwigger-Labs]                                                                
tags: [csrf]     
dirImg: /assets/img/PortSwigger/CSRF-with-no-defenses                     
toc: false                                                                                    
---

## Description

Link: https://portswigger.net/web-security/csrf/lab-no-defenses

>![]({{ page.dirImg }}/2021-07-13-13-02-06.png){: .shadow}


## Writeup

We can login to our account with the credentials we have `wiener:peter`.

Once logged it we are redirected to our account page

>![]({{ page.dirImg }}/2021-07-13-15-34-46.png){: .shadow}

Here we can change our email by providing a new email.

We fire up Burp in order to record the `update email` request and analyze it

>![]({{ page.dirImg }}/2021-07-13-15-50-31.png){: .shadow}

The new email is specified in the POST body and url encoded. 

Since there aren't any CSRF protections we can simply make the victim issue a `change email` operation and hardcode the new email in the post body. 

The response that we will serve back to the victims when they visit the `/exploit` page is the following:

>![]({{ page.dirImg }}/2021-07-13-16-03-53.png){: .shadow}

The form will be submitted without any user interaction and they email will be changed to **pwned@gmail.com**.

The CSRF attack succedeed.
