## Initial Access

Threader3000 scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6008e914-bb30-4e14-9f47-c8e4d2d29c1f)

nmap scan:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f41c4f51-9cb1-4168-b9f8-b5cfa4e0ce66)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/78604f29-9e14-4c3d-b59d-2ea1f1244278)

The scan tells us that the domain is "authority.htb"

Doesn't seem like the website on port 80 has anything:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/0f1e8b44-169e-45d4-b00b-0bcdff21a369)

Kerbrute output shows some usernames:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/2b1eb5b7-77c8-4cd8-a0f9-11610e57012b)

smbclient to enumerate shares:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/b740ea9f-4f2b-473a-a19d-ea8a25af3ab3)

Seems like I only have read access to Development share:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/2fb023ce-1dfb-4b6f-90be-4645f6ec38c4)

Creds exposed in "Development/Automation/Ansible/PWM/defaults/main.yml":

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ad60d1ec-a2cf-49af-ae23-2af30bd2607c)

ansible2john to crack the password, it seems like all three hashes crack to the same password:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/4a425dad-efd4-446d-824c-f3d40ad3ea4b)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/e315a0a3-252f-4b22-86c6-64703cfc4beb)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6446da45-42bf-4f7f-b0c6-218270872665)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3b3ebd1d-b411-440a-a28d-8db2e3dfe2b5)

Now we can use this password with ansible-vault to decrypt and get the actual creds:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/b3b1b161-02aa-4e78-bbbe-8440dc10d235)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/2646a8a6-ec91-45f5-a660-1318b71a4d2c)

Going to "https://10.10.11.222:8443/pwm/private/config/manager" and logging in, we see that user "svc_ldap" exists, it is failing to connect to LDAP server "authority.authority.htb:636", and that we can download/import configuration. Maybe we can have it try to authenticate to us?
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/94c381df-8ac2-4608-8903-bc31f8ffda7c)

Modified the configuration file to downgrade to ldap and connect back to us, imported it, and then caught it with responder:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f96e1624-4185-411d-9f6a-884c9a53e33c)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/12d4d63a-5d7d-4e59-af9e-77d9b304b2bf)

Looks like we can authenticate with WINRM:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6507636a-98f8-4053-8c56-cd6880f801ba)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/c335d65d-237f-4c9c-b203-3b35e7da0184)


## Sources
https://medium.com/@depradip_8731/authority-hackthebox-47b95ebd2af
