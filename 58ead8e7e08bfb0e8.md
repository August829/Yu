# Hard-coded Credentials in Tenda AP W12 Device (V1/V2/V3)
## Overview
A hard-coded credentials vulnerability was identified in the Tenda AP W12 device running firmware version V1/V2/V3. The root user account uses a hard-coded password (cracked as "Fireitup" using the John tool). This password is stored in the file /etc_ro/shadow using MD5-crypt hashing, which can be easily decrypted by tools like John and exploited. For instance, it allows unauthorized root access to the device through network-accessible services or the administrative interface.

## Vulnerability Details
+ **Vendor**: Tenda
+ **Vulnerability Type**: Hard-coded Credentials(CWE-798)
+ **Affected Product**: Tenda AP W12
+ **Affected Version**: (V1/V2/V3)
V3.0.0.6(3948)
V2.0.0.6(10720)
V3.0.0.5(3644)
V2.0.0.3(8326)
V1.0.0.5(9419)
V1.0.0.1(5411)

+ **Attack Type**: Remote
+ **Attack Vector**: Unauthorized login using the default password (root:Fireitup) via network-accessible services or the administrative interface
+ **Impact**:
    - Privilege Escalation
    - Information Disclosure
    - Potential Code Execution
+ **Affected Component**: File, user authentication mechanism (/etc_ro/shadow)
+ **CVE ID**: Pending (CVE application in progress)
+ **Discovered by**: Yu_Bao
+ **Firmware**:
https://www.tenda.com.cn/material/show/673226879721541
https://www.tenda.com.cn/material/show/670449286803525
https://www.tenda.com.cn/material/show/658105462243397
https://www.tenda.com.cn/material/show/3651
https://www.tenda.com.cn/material/show/3650
https://www.tenda.com.cn/material/show/3085

## Discovery
The vulnerability was discovered by analyzing the firmware (ac9_kf_V15.03.05.19(6318_)_cn.bin). The file was extracted from the squashfs-root directory, and the MD5-crypt hash of the root user's password was cracked using John, resulting in the password "Fireitup". The cracked password allows attackers to log in to the router's system with root privileges.

## Steps to Reproduce
1. Extract the firmware image US_AC10V4.0si_V16.03.10.13_cn_TDC01.bin.
2. Locate the file in the extracted squashfs-root directory: squashfs-root/etc_ro/shadow.

<img width="1509" height="135" alt="image" src="https://github.com/user-attachments/assets/97974b3b-114b-4e4b-96df-032a0acc99e7" />

3. Use a password-cracking tool (e.g., John) to crack the MD5-crypt hash of this user:
    - root:Fireitup:14319::::::

<img width="759" height="119" alt="image" src="https://github.com/user-attachments/assets/5eaf2557-3476-433e-b247-75e4f0f66a41" />

4. Attempt to log in to the device's administrative interface or other network-accessible services using the cracked password.

## Impact
Attackers with network access to the device can:
+ Gain full administrative control by logging in with the root account (password: "Fireitup").
+ Access sensitive configuration data, potentially exposing network details, modify device settings, or execute arbitrary code, leading to further network breaches.
