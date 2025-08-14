# Online Bank Management System - feedback.php 'msg' Unauthorized SQL inject

#### Exploit Title: Online Bank Management System - feedback.php 'msg' Unauthorized SQL inject

#### Vendor Homepage: https://www.sourcecodester.com/php/15373/online-banking-management-system-php-free-source-code.html

#### Software Link:https://www.sourcecodester.com/download-code?nid=15373&title=Online+Bank+Management+System+in+PHP+Free+Source+Code

#### Version: Online Bank Management System 1.0

#### Tested on: phpstudy 2018

#### Description

The reason for the SQL injection vulnerability is that the website application does not verify the validity of the data submitted by the user to the server (type, length, business parameter validity, etc.), and does not effectively filter the data input by the user with special characters , so that the user's input is directly brought into the database for execution, which exceeds the expected result of the original design of the SQL statement, resulting in a SQL injection vulnerability.Online Bank Management System does not filter the content correctly at the "feedback.php msg" parameter, resulting in the generation of SQL injection.

#### Payload used:

```POST /login.php HTTP/1.1
POST /SourceCodesterBank/feedback.php HTTP/1.1
Host: 127.0.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 61

msg=test', (SELECT 1 FROM (SELECT(SLEEP(5)))a)) -- &send=Send
```

#### Proof of Concept

First, we use delay injection sleep(0) to see that the loading time of the page is 4 seconds.

<img width="1247" height="726" alt="image" src="https://github.com/user-attachments/assets/00a34a4a-92f9-4e00-9d9e-1662b4b1e584" />

Then we delay sleep(5) for another 5 seconds, and the page loading becomes 9 seconds, which successfully proves that sleep(5) has been executed successfully.

<img width="1241" height="722" alt="image" src="https://github.com/user-attachments/assets/cd793e5a-90d1-48c8-a9be-3adf788bcf21" />

