---
layout: post
title: Upgrade Azure Web Role to HTTP2
---

if you upgrade you Azure Web role from OS Family 4 to OS Family 5 and running with Azure SDK 2.9, you may have problem to open your website with Chrome(but works fine on IE). you will get 'SPDY Protocol Error' or 'ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY' in Chrome.

It is dued to HTTP2.  IIS 10 supports HTTP 2 and Windows 2016 has more strciter policy on cypher suits.

How to check the cipher suites being used on my web site?
1, you can use check online with ssllabs.com
https://www.ssllabs.com/ssltest/analyze.html?d=youwebsite-url.com
2, you can use IIS cyyhto tool
https://www.nartac.com/Products/IISCrypto
3, you can also find it in your regiestry.
"HKLM:\SOFTWARE\Policies\Microsoft\Cryptography\Configuration\SSL\00010002"

How to fix it? 
based on this document, you need to put those at the bottom of the list.
TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384

https://support.microsoft.com/en-us/help/4032720/how-to-deploy-custom-cipher-suite-ordering-in-windows-server-2016

you can do it via powershell script here.
https://github.com/NWebsec/NWebsec.AzureStartupTasks/blob/master/NWebsec.AzureStartupTasks/content/NWebsec.AzureStartupTasks/scripts/TLS_hardening.ps1
