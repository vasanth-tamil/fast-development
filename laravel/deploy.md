### laravel deployment

**step 1 ( copy all file in server path )**

```text
cp project/* server/laravel/
```

**step 2 ( export database sql )**

```text
create Database and import sql file
```

**step 3 ( edit .env file )**

```text
APP_DEBUG=false
APP_URL={{ DOMAIN_NAME }}

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE={{ DATABASE_NAME }}
DB_USERNAME={{ DATABASE_USERNAME }}
DB_PASSWORD={{ DATABASE_PASSWORD }}
```

**step 4**

```text
cp project/public/index.php server/laravel/
cp project/public/.htaccess server/laravel/
```

**step 5**

* `.htaccess` file link 
  
  
  
  https://stackoverflow.com/questions/71115900/hostinger-laravel-shared-hosting
  
  ```text
  <IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteRule ^(.*)$ public/$1 [L]
  </IfModule>
  ```
