# bakabot-attribute [![Latest Stable Version](https://poser.pugx.org/bakabot/attribute/v)](//packagist.org/packages/bakabot/attribute) [![License](https://poser.pugx.org/bakabot/attribute/license)](//packagist.org/packages/bakabot/attribute) [![Build Status](https://travis-ci.com/bakabot-php/attribute.svg?branch=main)](https://travis-ci.com/bakabot-php/attribute)
Provides accessors to single-value [PHP Attributes](https://www.php.net/manual/en/language.attributes.overview.php).


## Installation
`composer require bakabot/attribute`

## Flavors
For ease of use the library provides three identical way of accessing attribute values:

```php
namespace Bakabot\Attribute;

// functions
attr(string|object $class, string $attribute, mixed $default = null): mixed;
getValue(string|object $class, string $attribute, mixed $default = null): mixed;

// public static method
AttributeValueGetter::getAttributeValue(string|object $class, string $attribute, mixed $default = null): mixed;
```

## Usage
Works on both instances and class names:

```php
use function Bakabot\Attribute\getValue;

#[Attribute]
final class SomeAttribute
{
    public function __construct(private string $value) {}
}

#[SomeAttribute('foo')]
class MyClass {}

$value = getValue(MyClass::class, 'SomeAttribute'); // "foo"
$value = getValue(new MyClass(), 'SomeAttribute'); // "foo"
```

---

Throws a `MissingAttributeException` if the attribute is not set:

```php
getValue(MyClass::class, 'UnknownAttribute');
// uncaught Bakabot\Attribute\Exception\MissingAttributeException
```

Unless you provide a default value as a third argument:

```php
getValue(MyClass::class, 'UnknownAttribute', 'bar'); // "bar"
```

---

If the attribute is repeatable, it'll return an array of that attribute's values:

```php
use function Bakabot\Attribute\getValue;

#[Attribute(Attribute::IS_REPEATABLE)]
final class RepeatableAttribute
{
    public function __construct(private string $value) {}
}

#[RepeatableAttribute('foo')]
#[RepeatableAttribute('bar')]
class MyClass {}

$value = getValue(MyClass::class, 'RepeatableAttribute'); // ["foo", "bar"]
```
