## Inital Foothold

Threader3000 scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f1931764-055c-478e-b762-d453ff66a78c)

nmap scan, pretty sure the other ports are from other HTB users on the box:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3c220e8e-c0ad-446c-bd4f-7e484984af03)

ffuf output (couldn't find actuator unless i used rockyou.txt so i got a hint lmao):

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3329ad23-5e99-4207-beb1-4985a709fbac)

hostname revealed, add it to /etc/hosts:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ef498ec1-ac09-4fb6-af34-7344ff25545a)


actuator shows that there is a sessions subdirectory, which contains session cookies:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/9a01c633-1045-4c59-aa18-b5b2455a7d2b)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6b3f938c-937b-41a7-bce5-7a7a13764968)

Changing our cookie, we get access to /admin, it seems like its related to ssh so there may be an opportunity for command injection:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d2e9841e-9fa6-4f71-9209-155dfbaed2eb)

Executing the shell with command injection (NOTE: ${IFS} becomes a space in bash, which is the context for this command injection, as spaces are not allowed):

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/1238cc20-fc0b-4376-9be2-7611e4636228)

Get the shell:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/13b568f2-7910-4308-959d-e73b052331d5)

## Privesc

Host a python server on the target machine to grab the jar file:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3fd9324f-91e9-4817-8892-21ef23d333c7)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f88840d1-3afa-481d-aeb1-dd5e21150227)

Going to BOOT-INF/classes/application.properties, we see postgresql creds:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/fcc262d7-943f-4e81-82ae-b19c776e5c6c)

On our shell in the target machine, connect to postgresql and get stored credentials and crack them:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/6ed7ae7e-f756-42a5-94d2-3c776aac1d36)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ff555de4-cb4f-4e44-b021-bca97a986764)

ssh into john and see we can run ssh as root:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/58acc100-cf2f-477d-9fef-06b7e3745558)

gtfobin to privesc:
- https://gtfobins.github.io/gtfobins/ssh/
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/10801dc0-48d6-4bfc-b001-5b8530ddfd94)

## What I learned
 - ${IFS} to insert spaces in bash
 - postgresql commands
