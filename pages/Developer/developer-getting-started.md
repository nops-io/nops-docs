---
title: Getting started with the nOps developer API
keywords: setup, onboarding, sharesave, api, developer
tags: [developer]
sidebar: mydoc_sidebar
permalink: developer-getting-started.html
folder: Developer
series: [API]
weight: 2.0
---

# How to create an API key and how to sign your API requests #

This topic describes the steps to create a signed request. This means that you must generate a key pair that is used to verify your signature.

**Summary of steps**

To create a signed request, you must complete the following steps.

1.  **Configure an API key in nOps** through the nOps app.
2.  **Create a public/private key pair.** Configure nOps with the public key to validate the signature.
3.  **Compose your request.** You can get some help with our [Public Swagger](https://app.nops.io/public_swagger/)

## **Create a public/private key pair**. ##

### **Create a Signature Verification Key pair** ###

The following instructions are for Unix-based OS machines. For Windows platforms/OS we suggest using either [OpenSSL](https://github.com/openssl/openssl/blob/master/NOTES-WINDOWS.md) or [PuTTYgen](https://www.ssh.com/academy/ssh/putty/windows/puttygen).

Begin by opening a command window.

1.  **Generate a PEM Certificate** using the following command:  
    `$ openssl genpkey -out rsakey.pem -algorithm RSA -pkeyopt rsa_keygen_bits:1024`  

2.  **Extract a public key** from the PEM certificate by using the following command.  
    `$ openssl rsa -in rsakey.pem -pubout > key.pub`  
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/solutions/sol-rsa-key.png)  

3.  Copy this information into your clipboard.

4.  Log into the nOps console and from the **Organization Settings** pane click the **API Key** option
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-org-api-key-menu.png)
5.  Click on **Let’s Generate Your API Key**
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/solutions/sol-api-verified-key.png) 
 
6.  Paste your RSA key into the nOps API Key **Signature Verification** field.
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/solutions/sol-api-with-key-pasted.png)
7.  Click **Save**.  You will see a confirmation dialog that contains your newly generated key.  
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-api-key-generated.png)

    **You now have a fully configured API Client and can use this key when you send a request to nOps.

### **Compose your request** ###

Once you have the key, you can compose a signature string and sign it and send your request.

### **Signing your Request** ###

The format for the signature is:

`{client_id}.{date_str}.{url}?api_key={key}`

The information for the signature request contains the following:

* client\_id: The Client ID is contained in the first part of API\_Key that is returned.  
    In this example key: api_key=**123**.aaaa4432454ccccb5a2280e755fdzzzz  
    the api_key is 123
* date_str: Use the yyyy-MM-dd format, such as 2022-11-07
* url: Use a request url that _does not_ include the domain name. Ensure that the url contains a trailing slash (/) or the signature will not be verified, and your request will fail. The signature must match exactly before it can be verified.
* API key: Use the format ?api_key=xxx

For example:

123.2022-01-10./nops\_api/v1/billingGetTotal/?api\_key=123.aaaa4432454ccccb5a2280e755fdzzzz

The example signature shown above can only be used:

on the date: 2022-01-10  
for the API URL of: /nops_api/v1/billingGetTotal/  
for the client (123) indicated within the api_key request:  
?api_key=123.aaaa4432454ccccb5a2280e755fdzzzz

The signature must match exactly for the signature to be verified.

To sign and encode your signature string using your private key.

Following is an example of how to create a key pair using Python instead of the prior instructions to create a key pair.  However, in order to use the Python script you may first need to install _pycryptodomex_ by using this command:

`pip install pycryptodomex`  


See: [https://pycryptodome.readthedocs.io/en/latest/src/installation.html](https://pycryptodome.readthedocs.io/en/latest/src/installation.html)

```
import binascii 
from Cryptodome.Hash import SHA256 
from Cryptodome.PublicKey import RSA 
from Cryptodome.Signature import pkcs1_15 

key = RSA.generate(1024) 
encrypt_key = key.export_key(pkcs=8) 
public_key = key.publickey().export_key().decode() 

message = "123.2022-01-10./nops_api/v1/billingGetTotal/?api_key=123.aaaa4432454ccccb5a2280e755fdzzzz" 
encoded_string = message.encode() 
byte_array= bytearray(encoded_string) 
sha_bytes = SHA256.new(byte_array) 
signature = pkcs1_15.new(encrypt_key).sign(sha_bytes) 
signature = binascii.b2a_base64(signature)[:-1].decode("utf-8")

```

## **To send an API request** ##

Include the encoded signature in the `x-nops-signature` header.

API requests should use the following format.

    https://app.nops.io <enter_REST-URI>?api_key=<Client_key>

`<enter_REST-URI>` is the location of the endpoint for example: /c/admin/projectaws/

`<Client_key>` is the API key you generated within nOps in the **Create a public/private key pair** procedure earlier in this document.

{% include custom/series_related.html %}
