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

## Privesc
Uploading winpeas.exe:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/44b3e65d-fcb8-4f39-bd56-a670664793fe)

Running winpeas, we see that it could be vulnerable to KrbRelayUp:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6c866724-5e6e-435b-a1fd-a0db186d3bd7)

Reading up on it, we see that we could possibly create computers in the domain:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/a3b3013b-24de-40ef-a3ff-6917e10e732b)

Couldn't get it to work, but there is a privesc tool similar in the sense that it adds computers to the domain is impacket addcomputer.py, which just requires that MachineAccountQuota > 0.

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5c9295fc-0c67-47eb-9a97-aef2bf9dfed7)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/fcf233aa-ac01-4b79-9e35-a68ee907bb35)

Requesting certificate for our new computer:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/dc4c52c1-8192-453a-a98c-681080a06b37)

This one cert has a vulnerability field allowing client authentication, we probably want this one:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/93e3c5ba-1256-49c2-bc73-77cf6551283a)

Request the cert:
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d797242c-8079-4b88-b4d1-e0194e06fac2)

Use this cert to authenticate as administrator:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/53e749b9-4e4b-4b04-a6ca-e215482e2279)

While impersonating administrator@authority.htb, create a user and add it to "Domain Admins" group. (looks like password has to be a certain complexity):

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/20a4b92f-b47e-4c92-96dd-2ac78c020f53)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d0a5447d-78ce-4bff-ade9-1fab4b1e52f0)

Finally rooted:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/94d23ebc-1379-4f2a-8a30-3c18d962bc9e)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/25654f9f-33a3-4363-8ed9-5da632595f31)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/72edd539-34ae-4ff6-9b49-cf16c29289ba)

## What I learned
Ansible Vault Cracking, configuring Ansible ldap server, Active Directory Certificate Services (AD CS) Privesc, KrbRelayUp/impacket addcomputer.py

## Sources
- https://medium.com/@depradip_8731/authority-hackthebox-47b95ebd2af
- https://github.com/ShorSec/KrbRelayUp
- https://tools.thehacker.recipes/impacket/examples/addcomputer.py
- https://github.com/ly4k/Certipy
