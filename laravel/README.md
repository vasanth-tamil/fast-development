## Laravel

Add forignkey

```php
$table->bigInteger("category_id")->unsigned()->index()->nullable(False);
$table->foreign("category_id")->references("id")->on("categories")->onDelete('cascade');Drop foreignkey, index and column
```

```php
$table->dropForeign('lists_user_id_foreign');
$table->dropIndex('lists_user_id_index');
$table->dropColumn('user_id');
```