# Hard-coded Credentials in Tenda F1202 Device (V1.2.0.14/V1.2.0.20/V1.2.0.9)
## Overview
A hard-coded credentials vulnerability was identified in the Tenda AP W12 device running firmware version V1.2.0.14/V1.2.0.20/V1.2.0.9. The root user account uses a hard-coded password (cracked as "Fireitup" using the John tool). This password is stored in the file /etc_ro/shadow using MD5-crypt hashing, which can be easily decrypted by tools like John and exploited. For instance, it allows unauthorized root access to the device through network-accessible services or the administrative interface.

## Vulnerability Details
+ **Vendor**: Tenda
+ **Vulnerability Type**: Hard-coded Credentials(CWE-798)
+ **Affected Product**: Tenda F1202
+ **Affected Version**:
V1.2.0.14(408)
V1.2.0.20(408)
V1.2.0.9(8155)

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
https://www.tenda.com.cn/material/show/1809
https://www.tenda.com.cn/material/show/2204
https://www.tenda.com.cn/material/show/2671

## Discovery
The vulnerability was discovered by analyzing the firmware (US_F1202V1.0BR_V1.2.0.14(408)_CN_TD.bin/US_F1202V1.0BR_V1.2.0.20(408)_CN_TD.bin/US_F1202V1.0BR_V1.2.0.9(8155)_CN_TD.bin). 

The file was extracted from the squashfs-root directory, and the MD5-crypt hash of the root user's password was cracked using John, resulting in the password "Fireitup". The cracked password allows attackers to log in to the router's system with root privileges.

## Steps to Reproduce
1. Extract the firmware image (US_F1202V1.0BR_V1.2.0.14(408)_CN_TD.bin/US_F1202V1.0BR_V1.2.0.20(408)_CN_TD.bin/US_F1202V1.0BR_V1.2.0.9(8155)_CN_TD.bin).
2. Locate the file in the extracted squashfs-root directory: squashfs-root/etc_ro/shadow.

<img width="2016" height="121" alt="image" src="https://github.com/user-attachments/assets/bd336ef2-b517-42ca-8b06-a8fbafcb59dd" />


3. Use a password-cracking tool (e.g., John) to crack the MD5-crypt hash of this user:
    - root:Fireitup:14319::::::

<img width="759" height="119" alt="image" src="https://github.com/user-attachments/assets/5eaf2557-3476-433e-b247-75e4f0f66a41" />

4. Attempt to log in to the device's administrative interface or other network-accessible services using the cracked password.

## Impact
Attackers with network access to the device can:
+ Gain full administrative control by logging in with the root account (password: "Fireitup").
+ Access sensitive configuration data, potentially exposing network details, modify device settings, or execute arbitrary code, leading to further network breaches.
