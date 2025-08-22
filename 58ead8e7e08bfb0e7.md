# Vendor

yanyutao0402

# Product

ChanCMS

# version

 V3.3.0

# Download 

https://gitee.com/yanyutao0402/ChanCMS


# Vulnerability

SSRF

# Description

The `getPages` and `getArticle` methods in `CollectController` both get the URL from the request body and call `collect.common` to get the contents of the URL. In the `getPages` method, the `targetUrl` parameter has been verified by the `isValidTargetUrl` function. However, in the `getArticle` method, the `taskUrl` parameter is not validated by `isValidTargetUrl` before being passed to `collect.common`.

# Analysis

The `getPages` and `getArticle` methods in `CollectController` both get the URL from the request body and call `collect.common` to get the contents of the URL. In the `getPages` method, the `targetUrl` parameter has been verified by the `isValidTargetUrl` function.

<img width="675" height="291" alt="image" src="https://github.com/user-attachments/assets/866beb58-06f2-4834-93a5-4fa1822ecfa9" />

The `isValidTargetUrl` function (defined in `middleware/guard.js`) checks whether the URL points to a private IP address, effectively preventing SSRF attacks. However, in the `getArticle` method, the `taskUrl` parameter is not validated by `isValidTargetUrl` before being passed to `collect.common`.

<img width="809" height="294" alt="image" src="https://github.com/user-attachments/assets/8c849b45-28d3-42a4-b7e6-54f738e95e35" />

This is an obvious SSRF vulnerability. An attacker can provide a URL to an internal network resource (e.g. `http://127.0.0.1:80`) to scan the internal network or interact with internal services.

We reproduce this SSRF vulnerability

<img width="934" height="275" alt="image" src="https://github.com/user-attachments/assets/1ea24def-795f-4d41-9f2a-1e87904f3ad3" />


# POC
```
POST /cms/collect/getArticle HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Content-Length: 123

{
  "taskUrl": "http://127.0.0.1:80",
  "titleTag": "title",
  "articleTag": "body",
  "parseData": "return data;"
}

```
