# Operators

## Php Operators

### 1. :: 

The <u>::</u> operator is used to refer to the class scope, see following examples:

```php
<?php
    
class Parent
{
    public const VAR = 'CONSTANT VAR'; // Define class scope variable
}

class Child extends Parent 
{
    public static $var = 'static var'; // Define class scope variable
    
    public static function method()
    {
        echo parent::VAR; // Using 'parent' keyword
        echo self::$var; // Using 'self' keyword
    }
}

echo Parent::CONST_VALUE; // Using class name
```

### 2. .

The <u>.</u> operator is used to concatenate two strings, see following example:

```php
$var = 'Hello ' . 'World!';
```

### 3. ->

The <u>-></u> operator is used to access members or methods on the object scope, see following examples:

```php
$var = $object->member;
```

