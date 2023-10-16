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

ansible2john to crack the password:





