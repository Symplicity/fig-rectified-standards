# Coding Style Guide Additions

This guide extends and expands on [PSR-2-R], the basic coding style guide.

These additions are totally optional. There was a void in the style guide so far
regarding these additions and as such are notices here as best practice recommendations.

[PSR-2-R]: PSR-2-coding-style-guide.md
a
Note that `[` and `]` (PHP5.4+) are used instead of `array(` and `)` for array declaration;

## Use Declarations

* Use declarations should be in alphabetical order.

## Properties and variables.

* Properties and variables should be in $camelBacked style.

```php
class Foo
{
	public $requestHandler;

	protected $isBool;

	public function foo($countVar, MyObject $myObject)
	{
		$countVar++;
		$myObject->bar();
	}

}
```

## Structure
- Use parentheses when instantiating classes regardless of the number of arguments the constructor has.
- Declare class properties before methods.
- Declare public methods first, then protected ones and finally private ones.
The exceptions (when using PHPUnit) to this rule are the class constructor and the `setUp` and `tearDown` methods of PHPUnit tests, which should always be the first methods to increase readability.

## Traits

Traits are treated as classes.

## Ternary Operator
Ternary operators are permissible when the entire ternary operation fits on one line.
Longer ternaries should be split into if else statements. Ternary operators should not ever be nested.
Optionally parentheses can be used around the condition check of the ternary for clarity:

```php
// Nested ternaries are bad.
$variable = isset($options['variable']) ? isset($options['othervar']) ? true : false : false;

// Good, simple and readable.
$variable = isset($options['variable']) ? $options['variable'] : true;
```

## Control Structures
Do not use keyword control structures. Use curly brackets instead for consistency across all files:

```php
// Bad.
if ($isAdmin):
	echo '<p>You are the admin user.</p>';
endif;

// Good.
if ($isAdmin) {
	echo '<p>You are the admin user.</p>';
}

```

## PHP Open Tags
Always use `<?php` instead of `<?=` or `<?`.

## Comparison

Always try to be as strict as possible. If a non-strict test is deliberate it might be
wise to comment it as such to avoid confusing it for a mistake.

For testing if a variable is null, it is recommended to use a strict check:
```php
// Faster and easier than is_null() call.
if ($value === null) {
      // ...
}
```

The value to check against should be placed on the right side:
```php
// Yoda style not recommended.
if (null === $this->foo()) {
    // ...
}

// Recommended for better (human) readability.
if ($this->foo() === null) {
    // ...
}
```

### Comparison methods
Consistenty is key here. The project should use one througout the code. In general stick to the short version.

`is_int()` should be used instead of `is_integer()`.
Use `is_writable()` instead of `is_writeable()`.

### Avoid conditional assigment
Especially with not using Yoda conditions one should never use single `=` inside conditions.
So avoid conditional assigments:
```php
// Conditional assigment not recommended.
if (($variable = $this->foo()) === null) {
    // ...
}

// Recommended.
$variable = $this->foo();
if ($variable === null) {
    // ...
}
```

## Whitespace

* Please use "trim-right" in your IDE settings to avoid unnecessary trailing white space.
* Use one newline at the end of the file. This avoids "missing newline" issues in several git hosting
environments.

```php
class Foo extends Bar implements FooInterface
{
	public function foo($a, $b = null) {
		// Code here.
	}

	public function bar() {
		// Method body.
	}
}

```

### Inter-Line Alignment
Don't use inter-line alignment. It is

a) Not useful in combination with tabs as indentation (and personal length adjustment of it).
b) Very bad for change diffs, as it creates a lot of noise. It is also additional work.

```php
public function bar()
{
    $foo = 'bar';
    $bazdib = 'gir';
    $zim = 'irk';

    $varname = '1234' . aVeryLongFunctionName()
        . 'foo' . otherFunction();
}
```
The aligment would have to completely change for the above three variables, if one of them
would be named longer, renaming all involved and aligned variables with it. Bad idea.

The second example would result in the same issue. So always use simple spaces for inline and
one-tab indents for multiline aligments.

### Strings and Concatenation

`'` or `"`? Both work, as long as they are used consistent throughout a file.
It is recommended to use the single `'` – as `"` is for HTML attributes and parses variables.
This also helps keeping lines shorter since you can split them on concatenations.

Don't use variables inside strings – they are better splitted like that:
```php
echo 'A string with ' . $someVariable . ' and ' . SOME_CONSTANT . '!';
echo '<a href="example.org" title="' . $title . '">Link</a>';
```

In case a string contains `'`, it is applicable to switch to `"` here to avoid
the usage of `\` escapes:
```php
$sql = "UPDATE TABLE 'foo' SET ContactName='Alfred Schmidt', City='Hamburg' WHERE ...";
```

Use a space before and after `.`:
```php
$myString = 'a string' . $variable . 'more string stuff etc';
```

## Multi-line declaration/condition/concatenation
All operators should go in the newline as first character:
```php
$foo = 'Some String'
	. ' concatenated';
```

Multi-line control structures should have the parentheses in their own lines (similar to classes and methods):
```php
if (
	($a === $b)
	&& ($b === $c)
	|| (Foo::CONST === $d)
) {
	$a = $d;
}
```

### Careful with deep arrays
One mistake that often gets made:
```php
$foo = [[
	0,
	1, 2
], 3, 4];
```
This would effectively change all lines (and their indentation), if the array structure got normalized.
Arrays also need to minimize effects on the resulting diff, and as such indentation must always be the right one:
```php
$foo = [[
		0,
		1, 2
	], 3, 4];
```
for example, max. normalized as:
```php
$foo = [
	[
		0,
		1,
		2
	],
	3,
	4
];
```
As you can see, entries like `0` would not need any change on reorganizing,
thus reducing overhead in work and making diffs easier to read as they only show actual changes made.

## Typehinting
Arguments that expect objects, arrays or callbacks (callable) can be typehinted:
```php
public function foo(Model $model, array $array, callable $callback, $boolean)
{
}
```
Here $model must be an instance of Model, $array must be an array and $callback must be of type callable (a valid callback).

Note that if you want to allow $array to be also an instance of ArrayObject you should not typehint as array accepts only the primitive type:
```php
public function foo($array)
{
}
```

## Method Chaining
Method chaining should have multiple methods spread across separate lines, and indented with four spaces:
```php
$email->from('foo@example.com')
    ->to('bar@example.com')
    ->subject('A great message')
    ->send();
```

## Casting
For casting use:

* `(bool)` - Cast to boolean.
* `(int)` - Cast to integer.
* `(float)` - Cast to float.
* `(string)` - Cast to string.
* `(array)` - Cast to array.
* `(object)` - Cast to object.

Please use `(int)$var` instead of `intval($var)` and `(float)$var` instead of `floatval($var)` when applicable.
Use `(bool)$var` instead of `!!$var`.

## Commenting Code
All comments should be written in English, and should in a clear way
describe the commented block of code.

Inline code should use `//` and a single space afterwards.
Use sentences with a capital first letter and a full stop if possible:
```php
// This is a nice inline comment.
$this->doSomething();
```

Comment blocks, with the exception of the first block in a file, should always be preceded by a newline.

### Tags
Recommended tags for each function/method are:

* `@throws` if applicable
* `@triggers` if applicable

Additionally these may be useful:

* `@see` or `@link`
* `@deprecated` if applicable - using the `@version <vector> <description>` format, where version and description are mandatory.

For PHPUnit:

* `@covers` if applicable

For additional tags see [phpDocumentator](http://phpdoc.org/).

The `@package` and `@subpackage` annotations are not used.

## Naming Conventions
- Use namespaces for all classes.
- Suffix interfaces with `Interface`.
- Suffix traits with `Trait`.
- Suffix exceptions with `Exception`.

## Writing better code

* Try to accept and return as few types as possible (mixing integer, boolean, string, array, object etc is usually not too good)
* Try to return early in methods/functions to avoid unnecessary depths

### Example for return early
```php
// Bad.
public function foo($input, $anotherInput = null)
{
	if ($input) {
		if ($anotherInput) {
			if ($anotherInput === $input) {
				// Code
				return true;
			}
		}
	}
	return false;
}

// Good.
public function foo($input, $anotherInput = null)
{
	if (!$input || !$anotherInput || $anotherInput !== $input) {
		return false;
	}
	// Code.
	return true;
}

```

### Use `private` when possible
Use `private` until you are forced to use `protected`.
Use `public` only if you really have to, but only for methods. Class variables should never be `public`.

### Do not use underscore as a prefix for anything
Never start any variable, method, or class name with an underscore.

## Example Addresses
For all example URL and mail addresses use `example.com`, `example.org` and `example.net`, for example:

* Email: `someone@example.com`
* WWW: `http://www.example.com`
* FTP: `ftp://ftp.example.com`

The `example.com` domain name has been reserved for this
(see [RFC 2606](http://tools.ietf.org/html/rfc2606.html))
and is recommended for use in documentation or as examples.

## Line Length (Relaxed addition to original recommendation)
It is recommended to keep lines under 100 characters long for better code readability.
Lines must not be longer than 120 characters.

## Other

### Files
File names which do not contain classes should be lowercased and underscored, for example:
```
long_file_name.php
```

### .editorconfig
The following is recommended to be put in your root dir (where composer.json is, as well) as `.editorconfig` file:

```
# This file is for unifying the coding style for different editors and IDEs
# editorconfig.org
root = true

[*]
end_of_line = lf
charset = utf-8
indent_style = space
indent_size = 4
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false

[*.yml]
indent_style = space
indent_size = 2
```
YML files unfortunately are only valid with a 2 space indentation.

## Further considerations (and still up for discussion)
While so far the main focus was on the developer (readability), there are some additional optional guidelines that can help to further reduce diff size on code modification (maintainability).
Those can and should be automated, though, as there is no point in forcing the developer to take care of those manually.

The main idea is to keep each line independant from the others so removing or adding lines has minimal impact.

### Multi-line arrays
Arrays that span across multiple lines can have a trailing comma to make sure that adding new rows does not change the previous row, as well.
```php
$array = [
	'first',
	'second', // Note the trailing comma
];
```

### Multi-line logic
For longer logic (method calls, operations) it can be helpful to put the trailing semicolon at the next line. Especially for fluid programming this will not show the previous row as modified.

```php
$Object
	->doFirst()
	->doSecond()
;
```
This would also be consistent to the symmetric bracket placing in general.

