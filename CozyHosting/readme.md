![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/bb1f4cec-1c13-4f3c-8892-86d12985f7cb)## Inital Foothold

Threader3000 scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f1931764-055c-478e-b762-d453ff66a78c)

nmap scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3c220e8e-c0ad-446c-bd4f-7e484984af03)

ffuf output:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3329ad23-5e99-4207-beb1-4985a709fbac)

hostname revealed, add it to /etc/hosts:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ef498ec1-ac09-4fb6-af34-7344ff25545a)

looks like all ports other than 80 have this:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/c88afb8b-b1bc-48be-a4b1-b0b3987fea46)

actuator shows that there is a sessions subdirectory, which contains session cookies:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/9a01c633-1045-4c59-aa18-b5b2455a7d2b)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6b3f938c-937b-41a7-bce5-7a7a13764968)

Changing our cookie, we get access to /admin:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/0f56e41f-6591-440d-97a4-f85d427f8f5c)
