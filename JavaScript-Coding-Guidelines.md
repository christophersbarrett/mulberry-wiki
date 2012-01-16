# JavaScript Style Guide

*This document, especially its structure, owes heavily to the [jQuery Core
Style Guide](http://docs.jquery.com/JQuery_Core_Style_Guidelines).*

These style guidelines apply to all Mulberry JavaScript code, and to any
JavaScript code published as part of an "official" Mulberry demo.

## JSHint

With the exception of some vendor files, the Mulberry code passes JSHint with
the following settings:

    jsl = JSLint.new(
      :undef => false,
      :strict => false,
      :nomen => false,
      :onevar => false,
      :newcap => false,
      :regexp => false,
      :plusplus => false,
      :linter => 'jshint'
    )

For more on JSHint, see [the options documentation](http://www.jshint.com/options/).

## Variable Declaration

Variables should be declared at the top of a function, even if the variable is
not defined until later in the function. Variables that are assigned a value
when they are declared should be on their own line; variables that are declared
but that are not assigned a value may appear on the same line, but should
appear at the end of the declaration statement. There should be a space before
and after the `=`, and the variable names should be left-aligned with each
other:

    var foo = 1,
        bar = 2,
        bim, baz;

Variables should be named using `camelCase`. Use your judgment in creating
variable names; for example, arrays should generally have a plural name, and
it may make sense to give booleans a name that begins with `is`.

## Semicolons

Semicolons are never optional. Nothing should appear after a semicolon on the
same line as the semicolon.

## Indentation and Spacing

The Mulberry code uses two spaces for indentation. Whether you like this or
not, it is the way it is, and should be followed strictly.

Mulberry code strips trailing whitespace from all lines.

If you use vim, you may want to put something like the following in your
`.vimrc` file.

    function! StripWhitespace ()
        exec ':%s/ \+$//gc'
    endfunction
    noremap <leader>ss :call StripWhitespace ()<CR>

Other text editors should provide options for enabling this by default as well.

We have a *fairly* liberal policy toward the use of whitespace inside Mulberry
code. For example:

    if (foo === 'bar') {
      baz = { bim : 'bop' }
      bop = [ 1, 2, 3 ];
    }

Commas should always be follwed by a space.

Colons should always be preceded and followed by a space.

## Strings

Mulberry code uses single quotes around strings.

## Blocks

`if`, `else`, `for`, `while`, and `try` blocks *always* have braces and the
body of the block *always* begins on a new line, even if the body is only
a single line.

    if (foo) {
      bar();
    }

    if (foo) {
      foo = 1;
      return false;
    }

## Ternaries

The ternary operator can be very useful for variable assignment. However,
nexted ternaries are not allowed. That is, the following construct should
*never* be used:

    var a = b ? c : d ? e : f;

## Comments

Long comments should use `/* ... */`.

Single line comments should always be on their own line and be above the line
they reference. Additionally there should be an extra endline above it. For
example:

    var some = "stuff";

    // We're going to loop here
    for (var i = 0; i < 10; i++ ) {
      doIt();
    }

Inline comments are allowed as an exception when used to annotate special
arguments in formal parameter lists:

    function foo(types, selector, data, fn, /*INTERNAL*/ one ) {
      // do stuff.
    }

## Testing Equality

The strict equality operators (`===` and `!==`) should always be used. If
necessary, you should perform any variable coercion prior to the equality
check.

## Calling Functions

Function calls should include spaces between arguments, but should not include
spaces after the opening parenthesis or before the closing one:

    foo('bar', 1);

## Arrays and Objects

Empty objects and arrays don't need filler spaces.

    []
    {}

Arrays may be declared on one or more lines, depending on readability.
A populated array should have a space after the opening `[` and before the
closing `]`.

    var animals = [ 'cat', 'dog', 'honey badger' ];

Objects should almost always be declared on multiple lines, and should be
spaced for readability. At the very least, there should be a space after the
opening `{` and before the closing `}`, and before and after each colon:

    var dog = {
      color : 'black',
      name : 'Ellie'
    };

## Type Checks

Type checking should always be achieved via the built-in Dojo methods:
`dojo.isArray`, `dojo.isString`, `dojo.isFunction`, and `dojo.isObject`.

## Event Binding and Subscriptions

When binding to an event or subscribing to a topic, it's important to do so in
a way that will be appropriately torn down. If the binding or subscription is
happening inside a component (or any other class that inherits from
`toura._View`), then it should be done with the component's `this.connect` or
`this.subscribe` method, which will automatically ensure proper teardown. If
you perform the binding or subscription via the standard `dojo.connect` or
`dojo.subscribe`, you are responsible for ensuring proper teardown. For
example:

    var s = dojo.subscribe('/app/ready', function() {
      // do stuff
      dojo.unsubscribe(s);
    });
