#NFY Style Guide for Primal based PHP Code

This guide describes the current practices for NFY PHP code projects built upon the Primal framework.  If you encounter code that does not follow this guide, please take steps to update it for compliance.

##1. Overview

- All PHP files must use the `<?php ?>` tag syntax.  Short tags and ASP style tags are not allowed.
- Short-echo syntax (`<?= ?>`) may be used in code which will only be run in PHP 5.4.
- Files ending with a PHP block MUST omit the trailing closing `?>` tag.
- When making a method or function call, there MUST NOT be a space between the method or function name and the opening parenthesis.
- If using an array element to reference an array element, there must be a single space of padding after and before the outer brackets

```php
<?php
    $blah = $foo[ $bar[0] ];
```

##2. Indentation and brace placement

- Code MUST use **hard tabs** for indentation.  Tab width is left to the developer's discretion, as this can be configured at the editor level.
- Do NOT use tabs for non-indentation alignment, this should always be accomplished using spaces.
- There is no limit on line length, but it is recommended that lines be no longer than 160 characters, including indentation, for readability.
- Opening braces should always be placed on the same line as the class/method/control structure declaration. (Kernigan and Richie format).  Example:

```php
<?php
function foo () {
    //do something
}
```
    
- Visibility should be declared on all properties and methods.
- All functions should have a php_doc comment before the declaration.
- Select statements should keep all case logic on the same level as the opening statement:

```php
<?php
select ($var) {
case 1:
    break;
case 2:
    break;
}
```
    
- It is preferred that all control logic and function parameter lists be retained on a single line, but if it is necessary to split on to multiple lines, the first carriage return should be immediately following the opening parenthesis and each unit should be on a separate line.  Boolean operators and commas should tail the statement that came before:

```php
<?php
if (
    $var1 &&
    $var2 ||
    $var3
) {
    //do something
}

$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```
    
##3. Capitalization and naming

- Namespaces should always be UpperCamelCase and follow PSR-0 guidelines.
- Class names should always be UpperCamelCase, as should static class functions and static properties.
- Class member functions should always be lowerCamelCase.
- Class member properties can be either lowerCamelCase or lower under_score.
- Local variables should always be lower under_score.
- Constants and static properties that will not change during runtime should be declared in all upper case with underscore separators.
- Private properties and protected properties which may be overridden but aren't expected to be should be prefixed with an underscore.

```php
<?php
class Example {
    const PERIOD_DELAY = 10;
    
    static $UNCHANGING_LIST = array(
        'var1' => 'foo',
        'var2' => 'bar'
    );
    
    public $property;
    protected $_property;
    
    public memberFunction () {
        
    }
    
    public static StaticFunction () {
        
    }
    
}
```

##4. Static vs member methods and properties

Objects instantiation exists for the purpose of retaining and interacting with a saved state.  A class which does not define a saved state does not necessitate instantiation.  With this in mind:

- Functional class methods which only return a result and do not interact with a saved state should always be declared as static methods.
- Static class methods should only operate upon the state which is passed to them in their arguments and the data they return.  An exception is allowed for methods which alter or return a persistent value or collection of values such as a singleton.
- Singletons should always be accessed using a static function named `Class::Singleton()`.
 
##5. Chainable classes

- Any class function which does not return a result should return `$this` to allow for chaining.
- Classes which are intended to be chained should always have a static initialization function which takes the same arguments as the class constructor and returns a new class.  Example names: Start(), Create() or Init()
- When chaining functions off of a chainable class, place all chained calls on the next indentation level, but place the closing semi-colon on the original level.  This makes altering the chain easier as the semi-colon has its own row, and gives the statement vertical symmetry.
- It is recommended that any functionality triggered by parameters on the constructor/initializer function should also be available as a chainable function.

```php
<?php
ChainedClass::Start()
    ->firstFunction()
    ->secondFunction()
    ->thirdFunction()
;
```

##6. Other

- Whenever possible, database queries should be made using the Query object.
- All classes should be built for PSR-0 autoloading.  Any third party libraries which are not build for this method should have an initialization wrapper subclass.