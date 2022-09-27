# Methods

## Useful Native Php Methods

### 1. spl_autoload_register()

To automatically include classes, see following example:

```php
spl_autoload_register(
	function ($class_name)
    {
        require_once($class_name . '.php');
    }
)
```

### 2. file_exists()

To check if file exists in specified directory, see following example:

```php
if (file_exists(path))
{
    // Do something...
}
```

### 3. explode()

To split string using specified separator, see following example:

```php
$var = explode('controller/method/param', '/'); // Returns ['controller', 'method', 'param']
```

### 4. filter_var()

To filter string using specified filter, see following example:

```php
filter_var($var, FILTER_SANITIZE_URL);
```

### 5. rtrim()

To trip trailing characters off of string, see following example:

```php
$url = rtrim('controller/method/param/', '/');
```

