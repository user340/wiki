# httpsへリダイレクトさせる

`httpd.conf` に次の設定を追加する。

```
<IfModule mod_rewrite.c>
  RewriteEngine  on
  RewriteCond %{HTTPS} off
  RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```
