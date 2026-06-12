# Tables

[Back to API reference](./api-reference.md)

Helpers for resolving a database table and filtering which of its
columns are exposed on the Data Source.

## `ACA\DataSources\Facade\Table`

Resolves a database table by name. Applies the current site's table
prefix automatically.

```php
Facade\Table::from(string $table, ?string $identifier = null): Repository\Database\Table
```

The returned `Table` exposes `->filter()` for restricting which columns
are exposed. See `Filter\Name` below.

## `ACA\DataSources\Repository\Database\Table\Resolver`

The resolver that `Facade\Table` uses internally. Useful when you want
the raw `Table` object without the facade.

```php
(new Resolver())->resolve(string $table, ?string $identifier = null): Table
```

## `ACA\DataSources\Repository\Database\Table\Filter\Name`

Keeps only the listed columns on a resolved `Table`. Apply via
`Table::filter()`.

```php
new Name(array $column_names)
```

```php
Facade\Table::from('wp_posts')
    ->filter(new Name(['ID', 'post_title', 'post_date']));
```

To hide a list of columns instead (keep everything else), wrap the
filter in `Filter\Not`:

```php
use ACA\DataSources\Repository\Database\Table\Filter\Name;
use ACA\DataSources\Repository\Database\Table\Filter\Not;

Facade\Table::from('wp_posts')
    ->filter(new Not(new Name(['post_password', 'post_content_filtered'])));
```
