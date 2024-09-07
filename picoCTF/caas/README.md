## caas

Looking at the index.js, it seems like our input is being directly executed which should be a simple command injection using semicolon (;), allowing us to input a command after the cowsay binary:
![image](https://github.com/user-attachments/assets/bd1b0668-0029-407b-b14a-1b8c65dcdb2b)

Looking at the things in the folder that we are in, we see a file of interest:
![image](https://github.com/user-attachments/assets/63e1acac-298f-4eca-a1b7-2c0c36fae540)

Now we know the flag file and just cat it:
![image](https://github.com/user-attachments/assets/7b5e5fe4-cf6c-443b-a384-57f6cfd7e05b)
