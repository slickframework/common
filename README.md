# Slick Common package

[![Latest Version](https://img.shields.io/github/release/slickframework/common.svg?style=flat-square)](https://github.com/slickframework/common/releases)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/slickframework/common/develop.svg?style=flat-square)](https://travis-ci.org/slickframework/common)
[![Coverage Status](https://img.shields.io/scrutinizer/coverage/g/slickframework/common.svg?style=flat-square&b=develop=develop)](https://scrutinizer-ci.com/g/slickframework/common/code-structure?branch=develop)
[![Quality Score](https://img.shields.io/scrutinizer/g/slickframework/common.svg?style=flat-square&branch=develop)](https://scrutinizer-ci.com/g/slickframework/common?branch=develop)
[![Total Downloads](https://img.shields.io/packagist/dt/slick/common.svg?style=flat-square)](https://packagist.org/packages/slick/common)

Slick common package contains a set of useful classes and traits that are used by almost every class in the entire
Slick library. They form a solid base to develop on top of and help you remove the tedious work of create getters
and setters, allowing read and/or write access to the properties and inspect classes and properties.

This package is extracted from [Slick framework](https://github.com/slickframework/slick).

This package is compliant with PSR-2 code standards and PSR-4 autoload standards. It
also applies the [semantic version 2.0.0](http://semver.org) specification.

## Install

Via Composer

``` bash
$ composer require slick/common
```

## Usage

For a simple understanding of how easy is to work with property assignment and
property value retrieving lets create a basic class:

```php
use Slick\Common\Base;

class Car extends Base
{
    /**
     * @readwrite
     * @var string The car brand
     */
    protected $brand;
    
    /**
     * @readwrite
     * @var string The car model
     */
    protected $model;
}
```

This is a really simple class that has two properties. So lets create a car using 
`Base` class constructor.

### Creating objects

The `Slick\Common\Base::__construct()` method accepts only one argument and it
will be used to set the initial values of object properties. Take a look:

```php
$mazda = new Car(['brand' => 'Mazda', 'model' => 'M6']);
```
You can create objects with any property value, in any order making the code
more readable and adding a good level of flexibility to object construction.
It is also possible to pass an object to `Slick\Common\Base::__construct()`
constructor as in the following example.

```php
$data = (object) ['model' => 'M6'];
$mazda = new Car($data);
```
### Reading/Writing property values

Now that we have our `Car` object lets change its properties.
`Slick\Common\Base` class uses the PHP magic methods to handle class properties
in a way that is a lot easier to work with. It defines an annotations for
property visibility and sets the "Getters" and "Setters" for all of those
properties.

Basically if you have a property called `model` you can access that property
by calling `getModel()` method. 

In the same way, if you call `setModel()` method with the value you want to
set as argument the `model` property value will be changed.

```php
$mazda = new Car(['brand' => 'Mazda', 'model' => 'M6']);

print $mazda->getModel(); // This will print out "M6"

$mazda->setBrand("Toyota");
print $mazda->getBrand(); // This will print out "Toyota"
```

`Slick\Common\Base` class can also handle calls to properties that have the
`@read`, `@write` or `@readwrite` annotations letting you change or retrieve
the property values as if they were defined with `public` access.

```php
$mazda = new Car(['brand' => 'Mazda', 'model' => 'M6']);

print $mazda->model; // This will print out "M6"

$mazda->brand = "Toyota";
print $mazda->brand; // This will print out "Toyota"
```

The best thing with this approach is that for any call to a `protected`
property with access annotation a "getter" or "setter" will always be
called.
This is useful to ensure that a property will always be of a given type.
Lets assume that we have coded the model to be an object of class "Model"

```php
use Slick\Common\Base;

class Car extends Base
{
    /**
     * @readwrite
     * @var string
     */
    protected $brand;
    
    /**
     * @readwrite
     * @var Model
     */
    protected $model;
    
    public function setModel(Model $model)
    {
        $this->model = $model;
        return $this;
    }
}

```

If you pay attention to the class above I have just added a new type to 
the `Car::$model` property and a setter for that property. I used the PHP type
hinting feature to prevent setting the model with a different type.

```php
// This will trigger a fatal error
$mazda->model = "M6";

// This will work just fine
$mazda->model = new Model("M6");
```

So if you try to get a property value the correspondent `get[<PropertyName>]`
will be called and if you set a property value the correspondent
`set[<PropertyName>]` will be called.

There's another method that checks the boolean value of a property.

```php

class Car extends Base
{
    ...
    
    /**
     * @readwrite
     * @var bool
     */
    protected $manualShifting 
}

$mazda = new Car(['brand' => 'Mazda', 'manualShifting' => true]);

var_dump($mazda->isManualShifting()); // This will output (Bool) True

```
<div class="alert alert-warning" role="alert">
    <h4>
        <i class="fa fa-exclamation "></i>
        Careful
    </h4>
    
    As you may notice, all properties that can be handle by
    <code>Slick\Common\Base</code> class need to be set with <code>protected</code>
    visibility because the methods in that class need to have access
    to your class (they cannot access <code>private</code> properties) and
    if you set property visibility to <code>public</code> it will not trigger
    the PHP magic methods and wont be handled.
</div>

### The right access to properties

Has I mention before, `Slick\Common\Base` manages the read/write access to
properties based on annotations. The following table has the behavior and
information about each access annotation.

| Annotation   | Description | Behavior |
|--------------|-------------|----------|
| `@read`      | You can only read the property value.   | An `Slick\Common\Exception\ReadOnlyException` exception is thrown if you try to set a value. |
| `@write`     | You can only set the property value. | An `Slick\Common\Exception\WriteOnlyException` exception is thrown if you try to read a value. |
| `@readwrite` | You can get or change the property value. | - |


#### Careful

It also important to know that if you try to read the value of an undefined
property you will get a `NULL` value, but if you try to set the value of an
undefined property an `Slick\Common\Exception\UndefinedPropertyException`
exception will be thrown.


#### Note

When working with modern IDEs you will have warnings about accessing
properties that are `protected` or methods that aren't defined.
To avoid this errors you can add dock block tags to your class like
`@property`, `@method`, `@property-read`
and `@property-write`.

Please refer to [phpDocumentor manual page](http://manual.phpdoc.org/HTMLSmartyConverter/PHP/phpDocumentor/tutorial_tags.property.pkg.html)
for more information about those tags.

### Use it in any class

Some times it is not possible to extent from `Slick\Common\Base` class because you are already
extending an existing class. In those cases if you want to use the methods defined
in `Slick\Common\Base` you can use the `Slick\Common\BaseMethods` trait that apart
from constructor, all other functionality is present (this trait is used by
`Slick\Common\Base` class already).

In addition the trait has the `Slick\Common\BaseMethods::hydrate()` method that works
like the `Slick\Common\Base::__construct()` constructor accepting an associative array
or an object to set property values.

Take a look at an example:

```php
<?php

use Slick\Common\BaseMethods;

class User extends Model
{
    /**
     * @readwrite
     * @var string
     */
    protected $name;
    
    /**
     * @readwrite
     * @var string
     */
    protected $email;
    
    use BaseMethods;
}

$adapter = new Adapter('default');
$user = new User($adapter);

$user->hydrate(['email' => 'joe@example.com', 'name' => 'joe']);

print $user->getEmail(); // Will print out "joe@example.com"

```

#### Note about performance

Although the usage became simplified by using `Slick\Common\Base`
class or the `Slick\Common\BaseMethods` trait keep in mind that
extra work and class inspection is done by those methods tho perform
this features. We think about it when developing these classes and we try
to keep it at the best performance we could. 


## Testing

``` bash
$ vendor/bin/phpunit
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email :author_email instead of using the issue tracker.

## Credits

- [Slick framework](https://github.com/slickframework)
- [All Contributors](https://github.com/slickframework/common/graphs/contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
