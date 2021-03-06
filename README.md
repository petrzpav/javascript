# CTU JavaScript Guide

This guide aims to provide the ground rules for an application’s JavaScript code, such that it’s highly readable and consistent across different developers on a team. The focus is put on quality and coherence across the different pieces of your application.

The guide goes a bit beyond code style guidelines; it offers recommendations for technology stack, tooling and programming approach. It is purposefully opinionated – you don’t need to agree with everything, but you should at least think why you disagree.

This guide is aimed at needs of ICT development teams at [Faculty of Information Technology](http://fit.cvut.cz/) of [Czech Technical University in Prague](http://www.cvut.cz/), but you are more than welcome to adapt it for your organisation.

## tl;dr

- Stick with [Standard](https://github.com/feross/standard): [2 spaces, no semicolons, single quotes…](#code-style)
- Organise your code into [modules](#modules).
- Use [ESLint](http://eslint.org/) with [our rules](linters/).
- [Use ES6](#es6) with [Babel](https://babeljs.io/) unless the build process would add too much overhead to your project.
- Use `const` and `let` instead of `var`.
- Learn about [functional programming](#functional-programming). It will make you a better developer.
- [Avoid classes](#classes), [prototypes](#prototypes) and `this`. Stick with objects and pure functions.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [ES6](#es6)
- [Modules](#modules)
- [Strict Mode](#strict-mode)
- [Linting](#linting)
- [Code Style](#code-style)
  - [Indentation](#indentation)
  - [Spacing](#spacing)
  - [Line Length](#line-length)
  - [Brace Style](#brace-style)
  - [Semicolons`;`](#semicolons)
  - [Quotes](#quotes)
  - [Commas](#commas)
  - [Naming](#naming)
- [Variables](#variables)
- [Strings](#strings)
- [Equality](#equality)
  - [“Truthy” and “Falsy” Values](#truthy-and-falsy-values)
- [Ternary Operators](#ternary-operators)
- [Functions](#functions)
  - [Anonymous Functions](#anonymous-functions)
  - [Function Parameters](#function-parameters)
  - [Conditional Return](#conditional-return)
- [Destructuring](#destructuring)
- [Prototypes & Classes](#prototypes--classes)
  - [Extending Native Prototypes](#extending-native-prototypes)
  - [Prototypal Inheritance](#prototypal-inheritance)
  - [Class Based Inheritance](#class-based-inheritance)
  - [Recommended Reading](#recommended-reading)
- [Arrays](#arrays)
  - [Loops](#loops)
  - [Manipulation](#manipulation)
  - [Copy](#copy)
  - [Array-like Objects](#array-like-objects)
- [Objects](#objects)
  - [ES6 Shorthands](#es6-shorthands)
- [`console` statements](#console-statements)
- [Comments](#comments)
- [Polyfills](#polyfills)
- [Asynchronous Behaviour](#asynchronous-behaviour)
  - [Callbacks](#callbacks)
  - [Promises](#promises)
  - [Recommended Reading](#recommended-reading-1)
- [The Harmful Parts](#the-harmful-parts)
  - [`this` Keyword](#this-keyword)
  - [`new` Operator](#new-operator)
    - [But How About Performance?](#but-how-about-performance)
  - [Recommended Reading](#recommended-reading-2)
- [Functional Programming](#functional-programming)
  - [Separate Data and Behaviour](#separate-data-and-behaviour)
  - [Immutability](#immutability)
  - [Pure Functions](#pure-functions)
  - [Currying](#currying)
    - [Parameters Order](#parameters-order)
  - [Functional Composition](#functional-composition)
- [Acknowledgements](#acknowledgements)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## ES6

[Learn about ECMAScript 6](http://git.io/es6features), its new syntax and features, and use it in your project today.

For browser-oriented projects transpile your project with [Babel](https://babeljs.io/).

For server oriented projects, you can use most features of ES6 natively in Node.js. Alternatively you can use [babel-node](https://babeljs.io/docs/usage/cli/#babel-node) from the Babel project. Consult [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/) and [node.green](http://node.green/).

You may want to avoid Babel in smaller projects where the code compilation brings unjustified overhead.

See [tooling](tooling/) for tips how to manage your build process.

## Modules

Organise your code into [modules](http://jsmodules.io/). Use [ES6 modules](http://jsmodules.io/) (with Babel) or [CommonJS](http://wiki.commonjs.org/wiki/CommonJS) (in other cases). Each module is scoped with explicit exports and does not pollute global namespace. Additionally you can easily share modules between server and browser projects.

For browser projects, you can compose modules into a single _bundle_ with tools like [Browserify](http://browserify.org/) or [Webpack](https://webpack.github.io/). See [tooling](tooling/) for more information.

Module systems also provide us with dependency injection patterns, which are crucial when it comes to testing individual components in isolation.

## Strict Mode

Put [`'use strict'`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode) at the top of your modules. Strict mode allows you to catch nonsensical behaviour, discourages poor practices, and _is faster_ because it allows compilers to make certain assumptions about your code.

[Linter](#linting) can warn you about this. Babel [inserts `'use strict'`](https://babeljs.io/docs/advanced/transformers/other/strict/) by default, so this rule does not apply.

## Linting

Use [ESLint](http://eslint.org/) for linting your code with our [provided rules](linters/).

## Code Style

We build upon the [Standard](https://github.com/feross/standard) code style. You may not like some features of it, but your team members may like them – and vice versa. The point is: somebody else made these decisions for you, so you can carry on with your life and [don’t discuss the color of the bikeshed](https://www.freebsd.org/doc/en_US.ISO8859-1/books/faq/misc.html#idp60682704).

### Indentation

Spacing must be consistent across every file in the project. Put [`.editorconfig`](http://editorconfig.org) configuration file into your project. These are recommended defaults:

<a name="editorconfig"></a>
```ini
# editorconfig.org
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

**Use 2 spaces for indentation.** The `.editorconfig` file can take care of that for us and everyone will be able to create the correct spacing by pressing the tab key.

### Spacing

Spacing doesn’t just entail indentation, but also the spaces before, after, and in between arguments of a function declaration.

Put space before parenthesis and after comma.

```js
function (a, b) {
  // your stuff
}

if (true) {
  // other stuff
} else {
  // more stuff
}
```

Set off operators with spaces.

```js
// bad
const x=y+5

// good
const x = y + 5
```

End files with a single newline character ([`.editorconfig`](#editorconfig) will take care of it for you).

```js
(function(global) {
  // ...stuff...
})(this)↵
```

When making long method chains, use indentation. Use a leading dot, which emphasises that the line is a method call, not a new statement.

```js
// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount()

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount()
```

[Linter](#linting) will remind you about this style and [automatic formatter](https://github.com/maxogden/standard-format) can fix the code for you.

### Line Length

Where possible, improve readability by **keeping lines below the 100-character** mark. **Never go over 120 characters per line.**

### Brace Style

Per Standard, use the “[one true brace style](http://eslint.org/docs/rules/brace-style.html#1tbs).”

Braces are [always required for conditionals](#conditionals).

```js
// bad
function foo ()
{
  return true
}

// good
function foo () {
  return true
}
```

```js
// bad
if (foo) bar()

// good
if (foo) { bar() }
```

```js
// bad
if (bar)
{
  for (let i; i < 10; i++)
    console.log('Nan')
}

// good
if (bar) {
  for (let i; i < 10; i++) {
    console.log('Nan')
  }
}
```

```js
// bad
try
{
  somethingRisky()
} catch (e)
{
  handleError()
}

// good
try {
  somethingRisky()
} catch (e) {
  handleError()
}
```

```js
// bad
if (foo) {
  bar()
}
else {
  baz()
}

// good
if (foo) {
  bar()
} else {
  baz()
}
```

### Semicolons`;`

**We don’t use semicolons;** we rely on Automatic Semicolon Insertion _(ASI)_ instead. While [there are some caveats](https://github.com/feross/standard/blob/master/RULES.md#automatic-semicolon-insertion-asi), you shouldn’t run into them if you are not doing anything crazy in your code (and thus breaking this guide). The most notable caveat is this one:

> End of line is not treated as semicolon if the next line starts with `[`, `(`, `+` etc.

The following code won’t work:

```js
// bad
let a = b + c
(d + e).print()
```

Linter will warn you about these exceptions. You can solve this by putting semicolon _at the beginning_ of the line:

```js
// good
let a = b + c
;(d + e).print()
```

However, you may want to keep using semicolons in JavaScript if your project includes languages with mandatory semicolons, like Java. [Semistandard](https://github.com/Flet/semistandard) is then for you.

Regardless of your choice, a [linter](#linting) should be used to catch unnecessary, unintentional or missing semicolons.

### Quotes

**Use single quote `'`** for quoting strings consistently throughout your codebase.

```js
// bad
const message = "o hai!"

// good
const message = 'o hai!'
```

### Commas

**Do not use leading commas**, it’s just plain ugly.

```js
// bad
const story = [
    once
  , upon
  , aTime
]

// good
const story = [
  once,
  upon,
  aTime,
]
```

**Use trailing commas**, it makes code diffs cleaner and modifications simpler.

```js
// bad
const hero = {
  firstName: 'Ramona',
  lastName: 'Flowers'
}

// good
const hero = {
  firstName: 'Ramona',
  lastName: 'Flowers',
}
```

```js
// bad
const heroes = [
  'Batman',
  'Iron Man'
]

// good
const heroes = [
  'Batman',
  'Iron Man',
]
```

### Naming

Variables and functions must have meaningful names so that you don’t have to resort to commenting what a piece of functionality does. Instead, try to be expressive while succinct, and use meaningful variable names.

```js
// bad
function a (x, y, z) {
  return z * y / x
}
a(4, 2, 6) // => 3

// good
function ruleOfThree (had, got, have) {
  return have * got / had
}
ruleOfThree(4, 2, 6) // => 3
```

Use **camelCase** when naming objects, functions, and instances.

```js
// bad
const this_is_my_object = {}
function MYFUNCTION () {}

// good
const thisIsMyObject = {}
function myFunction () {}
```

Use **PascalCase** when naming constructors (object factories) or classes.

```js
// bad
function user (options) {
  return {
    name: options.name
  }
}
const myUser = user({name: 'Ada'})

// good
function User (options) {
  return {
    name: options.name
  }
}
const myUser = User({name: 'Ada'})
```

<!-- TODO: SCREAMING_SNAKE_CASE for constants? -->

## Variables

Declare variables where you need them, but place them in a reasonable place (e.g. not inside conditions).

Keeping variable declarations to _one per line is encouraged_. Always use one `const`, `let` or `var` statement for each assignment.

```js
// bad
const foo = 1,
      bar = 2

// good
const foo = 1
const bar = 2
```

```js
// bad
let a
  , b

// good
let a
let b
```

```js
// bad
const foo = 1

if (foo > 1) {
  let bar = 2
}

// good
const foo = 1
let bar

if (foo > 1) {
  bar = 2
}
```

Variable declarations that aren’t immediately assigned a value are acceptable to share the same line of code.

**Avoid using `var`.** Use `const` for all of your references. While this won’t prevent mutation of structures (i.e. objects or arrays), it will prevent a reassignment of the variable.

If you must mutate references, use `let` instead of `var`. `const` and `let` are scoped to block (i.e. within braces `{}`), while `var` is scoped within a function. See [JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html) for more details.

```js
// bad
var a = 1
var b = 1

// good
const a = 1
let b = 1

if (true) {
  b += 1
}
```

Group declarations by their type: `const` first, `let` second.

```js
// bad
const a = 1
let b, c
const d = 2

// good
const a = 1
const d = 2

let b, c

// acceptable
let foo, bar
```

## Strings

Use ES6 [template strings](https://developer.mozilla.org/docs/Web/JavaScript/Reference/template_strings) for strings formatting.

```js
// bad
const message = 'oh hai ' + name + '!'

// good
const message = `oh hai ${name}!`
```

## Equality

Avoid using `==` and `!=` operators, always favor `===` and `!==`. These operators are called the “strict equality operators,” while [their counterparts will attempt to cast the operands](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators) into the same value type.

```js
// bad
function isEmptyString (text) {
  return text == ''
}
isEmptyString(0) // => true

// good
function isEmptyString (text) {
  return text === ''
}
isEmptyString(0) // => false
```

There is one exception: use `==` and `!=` to check for `null` _or_ `undefined` values in one statement.

```js
// bad
if (text === null || text === undefined) {
  console.log('text is null or undefined')
}

// good
if (text == null) {
  console.log('text is null or undefined')
}
```

### “Truthy” and “Falsy” Values

Keep comparisons simple, but make sure you know what you are doing.

```js
// bad
if (name !== '') {
  // ...stuff...
}

// good
if (name) {
  // ...stuff...
}
```

```js
// bad
if (collection.length > 0) {
  // ...stuff...
}

// good
if (collection.length) {
  // ...stuff...
}
```

Conditional statements such as the `if` statement evaluate their expression using and convert value to boolean using these rules:

- Objects evaluate to `true`
- `undefined` evaluates to `false`
- `null` evaluates to `false`
- Booleans evaluate to the value of the boolean
- Numbers evaluate to false if `+0`, `-0`, or `NaN`, otherwise `true`
- Strings evaluate to false if an empty string `''`, otherwise `true`

```js
if ([0]) {
  console.log('this is true')
  // An array is an object, objects evaluate to true
}
```

## Ternary Operators

**Avoid nested ternary operators.** Ternary operators are fine for clear-cut conditionals, but unacceptable for confusing choices.

jQuery is a prime example of a codebase that’s [**filled with nasty ternary operators**](https://github.com/jquery/jquery/blob/c869a1ef8a031342e817a2c063179a787ff57239/src/ajax.js#L117).

```js
// bad
function calculate (a, b) {
  return a && b ? 11 : a ? 10 : b ? 1 : 0
}

// good
function getName (mobile) {
  return mobile ? mobile.name : 'Generic Player'
}
```

In cases that may prove confusing just use `if` and `else` statements instead.

## Functions

When declaring a function, always use the [function declaration form](http://stackoverflow.com/q/336859/389745) instead of [function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function). Because [hoisting](https://github.com/buildfirst/buildfirst/tree/master/ch05/04_hoisting).

```js
// bad
const sum = function (x, y) {
  return x + y
}

// good
function sum (x, y) {
  return x + y
}
```

That being said, there’s nothing wrong with function expressions that are just [currying another function](http://ejohn.org/blog/partial-functions-in-javascript/).

```js
// good
const plusThree = sum.bind(null, 3)
```

Keep in mind that [function declarations will be hoisted](https://github.com/buildfirst/buildfirst/tree/master/ch05/04_hoisting) to the top of the scope so it doesn’t matter the order they are declared in. That being said, you should always keep functions at the top level in a scope, and avoid placing them inside conditional statements.

```js
// bad
if (Math.random() > 0.5) {
  sum(1, 3)

  function sum (x, y) {
    return x + y
  }
}

// good
if (Math.random() > 0.5) {
  sum(1, 3)
}

function sum (x, y) {
  return x + y
}

// also good
function sum (x, y) {
  return x + y
}

if (Math.random() > 0.5) {
  sum(1, 3)
}
```

### Anonymous Functions

Use [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) for simple anonymous functions. <!-- FIXME: explain `this` -->

```js
// bad
[1, 2, 3].map(function (x) {
  return x * x
})

// good
[1, 2, 3].map((x) => {
  return x * x
})
```

Omit braces, parentheses, and return if the function accepts a single argument and fits into a single line.

```js
// better
[1, 2, 3].map(x => x * x)
```

Whenever a method is non-trivial, make the effort to **use a named function declaration rather than an anonymous function**. This will make it easier to pinpoint the root cause of an exception when analyzing stack traces.

```js
// bad
function once (fn) {
  let ran = false
  return function () {
    if (ran) { return }
    ran = true
    fn.apply(this, arguments)
  }
}

// good
function once (fn) {
  let ran = false
  return function run () {
    if (ran) { return }
    ran = true
    fn.apply(this, arguments)
  }
}
```

### Function Parameters

Learn about [default, rest, and spread parameters](https://github.com/lukehoban/es6features#default--rest--spread) in ES6.

**Never name function parameter `arguments`** since it is a reserved variable name (in fact, [it using it should cause a syntax error](http://ecma-international.org/ecma-262/5.1/#sec-12.2.1)).

```js
// bad
function nope (name, options, arguments) {
  // ...stuff...
}

// good
function yup (name, options, args) {
  // ...stuff...
}
```

**Do not use `arguments` keyword in function.** Use rest arguments instead.

```js
// bad
function concatenateAll () {
  const args = Array.prototype.slice.call(arguments)
  return args.join('')
}

// good
function concatenateAll (...args) {
  return args.join('')
}
```


**Use default parameter syntax** rather than mutating function arguments.

```js
// really bad
function handleThings (opts) {
  // No! We shouldn’t mutate function arguments.
  // Double bad: if opts is falsy it’ll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {}
  // ...
}

// still bad
function handleThings (opts) {
  if (opts === void 0) {
    opts = {}
  }
  // ...
}

// good
function handleThings (opts = {}) {
  // ...
}
```

### Conditional Return

Avoid keeping indentation levels from raising more than necessary by using guard clauses instead of flowing `if` statements.

```js
// bad
if (car) {
  if (black) {
    if (turbine) {
      return 'batman!'
    }
  }
}

// good
if (!car) { return }
if (!black) { return }
if (!turbine) { return }

return 'batman!'
```

```js
// bad
if (condition) {
  // 10+ lines of code
}

// good
if (!condition) { return }

// 10+ lines of code
```

## Destructuring

Use object destructuring when accessing and using multiple properties of an object.

```js
// bad
function fullName (user) {
  const firstName = user.firstName
  const lastName = user.lastName

  return `${firstName} ${lastName}`
}

// good
function fullName(obj) {
  const { firstName, lastName } = obj
  return `${firstName} ${lastName}`
}

// best
function fullName ({ firstName, lastName }) {
  return `${firstName} ${lastName}`
}
```

Use array destructuring.

```js
const arr = [1, 2, 3, 4]

// bad
const first = arr[0]
const second = arr[1]

// good
const [first, second] = arr
```

Use object destructuring for multiple return values, not array destructuring. If you add properties over time or change the order of things, you won't break call sites.

```js
// bad
function processInput (input) {
  // then a miracle occurs
  return [left, right, top, bottom]
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input)

// good
function processInput (input) {
  // then a miracle occurs
  return { left, right, top, bottom }
}

// the caller selects only the data they need
const { left, right } = processInput(input)
```

## Prototypes & Classes

### Extending Native Prototypes

Hacking native prototypes should be avoided at all costs. Use a function instead. If you must extend the functionality in a native type, try using something like [poser](https://github.com/bevacqua/poser) instead.

```js
// bad
String.prototype.half = function () {
  return this.substr(0, this.length / 2)
}

// good
function half (text) {
  return text.substr(0, text.length / 2)
}
```

### Prototypal Inheritance

**Avoid [prototypal inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) models** unless you have a very good _memory-consumption reason_ to justify yourself.

- Prototypal inheritance boosts need for `this` through the roof.
- It’s way more verbose than using plain objects.
- It causes headaches when creating `new` objects.
- Needs a closure to hide valuable private state of instances.
- Just use plain objects instead.

### Class Based Inheritance

**Avoid ES6 classes**. Since it is just a syntactic sugar over prototypal inheritance, [same caveats apply](#prototypes).

You should always prefer _[composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance)_, but class-based OOP makes inheritance very easy and composition unnecessarily complicated. Class-based OOP has a tendency to invite a lot of additional complexity to manage its own shortcomings.

Simple object factories and stateless, pure functions get you usually further without all the complexity introduced by class-based object-oriented programming.

```js
// bad
class Point {
  constructor (x, y) {
    [this.x, this.y] = [x, y]
  }
  get x () {
    return this.x
  }
  get y () {
    return this.y
  }
}

// good
function Point (x, y) {
  return Object.freeze({x, y})
}
```

### Recommended Reading

- The Two Pillars of JavaScript: [Part 1](https://medium.com/javascript-scene/the-two-pillars-of-javascript-ee6f3281e7f3) and [Part 2](https://medium.com/javascript-scene/the-two-pillars-of-javascript-pt-2-functional-programming-a63aa53a41a4).
- [How to Fix the ES6 `class` keyword](https://medium.com/javascript-scene/how-to-fix-the-es6-class-keyword-2d42bb3f4caf)
- [Think twice about ES6 classes](https://christianalfoni.github.io/javascript/2015/01/01/think-twice-about-classes.html)
- [Object Oriented Programming is an expensive disaster which must end](http://www.smashcompany.com/technology/object-oriented-programming-is-an-expensive-disaster-which-must-end)

## Arrays

Instantiate arrays using the square bracketed notation `[]`. If you have to declare a fixed-dimension array for performance reasons then it’s fine to use the `new Array(length)` notation instead.

### Loops

Don’t declare functions inside of loops. Whenever possible, use `.forEach` instead of a `for` loop. There is also no issue with declaring anonymous function within other functions.

```js
const values = [1, 2, 3]

// bad
for (let i = 0; i < values.length; i++) {
  setTimeout(function () {
    console.log(values[i])
  }, 1000 * i)
}

// also bad
for (let i = 0; i < values.length; i++) {
  setTimeout((i) => {
    return function () {
      console.log(values[i])
    }
  }(i), 1000 * i)
}

// somewhat acceptable
for (let i = 0; i < values.length; i++) {
  setTimeout(function (i) {
    console.log(values[i])
  }, 1000 * i, i)
}

// a bit more acceptable
for (let i = 0; i < values.length; i++) {
  wait(i)
}

function wait (i) {
  setTimeout(function () {
    console.log(values[i])
  }, 1000 * i)
}

// better
[1, 2, 3].forEach((value, i) => {
  setTimeout(() => {
    console.log(value)
  }, 1000 * i)
})
```

### Manipulation

It’s about time you master array manipulation! [Learn about the basics](http://blog.ponyfoo.com/2013/11/19/fun-with-native-arrays). It’s way easier than you might think.

- [`.forEach`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [`.slice`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
- [`.splice`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
- [`.join`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
- [`.concat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)
- [`.unshift`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)
- [`.shift`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
- [`.push`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
- [`.pop`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

Learn and abuse the functional collection manipulation methods. These are **so** worth the trouble.

- [`.filter`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [`.map`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [`.reduce`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- [`.reduceRight`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)
- [`.some`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
- [`.every`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
- [`.sort`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [`.reverse`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

### Copy

Use array spreads `...` to copy arrays.

```js
// bad
const len = items.length
const itemsCopy = []

for (let i = 0; i < len; i++) {
  itemsCopy[i] = items[i]
}

// good
const itemsCopy = [...items]
```

### Array-like Objects

Whenever you have to manipulate an array-like object, use [`Array.from`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

```js
const divs = document.querySelectorAll('div')

// bad
const nodes = []
for (let i = 0; i < divs.length; i++) {
  nodes.push(divs[i])
}

// good
const nodes = Array.from(divs)
```


## Objects

Instantiate using the egyptian notation `{}`.

```js
// bad
const obj = new Object()

// good
const obj = {}
```

When returning objects, consider using [`Object.freeze`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) to enforce immutability of the object, or at least [`Object.seal`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal) to maintain object’s properties.

### ES6 Shorthands

ES6 has some [nice improvements for object literal](https://github.com/lukehoban/es6features#enhanced-object-literals) which reduce boilerplate.

Use object method shorthand:

```js
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value
  },
}

// good
const atom = {
  value: 1,
  // you can omit function
  addValue(value) {
    return atom.value + value
  },
}
```

Use property value shorthand.

```js
const lukeSkywalker = 'Luke Skywalker'

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
}

// good
const obj = {
  lukeSkywalker,
}
```

## `console` statements

Preferably bake `console` statements into a service that can easily be disabled in production. Alternatively, don’t ship any `console.log` printing statements to production distributions.

## Comments

Comments **aren’t meant to explain what** the code does. Good **code is supposed to be self-explanatory**. If you’re thinking of writing a comment to explain what a piece of code does, chances are you need to change the code itself. The exception to that rule is explaining what a regular expression does. Good comments are supposed to **explain why** code does something that may not seem to have a clear-cut purpose.

```js
// bad
const p = $('<p/>') // create the centered container
p.center(div)
p.text('foo')

// good
const container = $('<p/>')
const contents = 'foo'

container.center(parent)
container.text(contents)
```

```js
// good
megaphone.on('data', function (value) {
  // the megaphone periodically emits updates for container
  container.text(value)
})
```

```js
// good
const numeric = /\d+/ // one or more digits somewhere in the string
if (numeric.test(text)) {
  console.log('so many numbers!')
}
```

Commenting out entire blocks of code _should be avoided entirely_, that’s why you have version control systems in place!

## Polyfills

Where possible use the native browser implementation and include [a polyfill that provides that behaviour](http://remysharp.com/2010/10/08/what-is-a-polyfill/) for unsupported browsers. This makes the code easier to work with and less involved in hackery to make things just work.

If you can’t patch a piece of functionality with a polyfill, then [wrap all uses of the patching code](http://blog.ponyfoo.com/2014/08/05/building-high-quality-front-end-modules) in a globally available method that is accessible from everywhere in the application.

## Asynchronous Behaviour

If your application works with external data or handles user interaction, you will have to deal with asynchronous code. Typically we pass around callbacks, for example (in Node):

```js
fs.readdir(source, function(err, files) {
  if (err) {
    // error
  } else {
    // do something else
  }
})
```

While callbacks are very simple in context, things get nasty if we need to do a more asynchronous operations which depend on each other. By layering callbacks upon callbacks, we may end up in [the Callback Hell](http://callbackhell.com/). Typical scenery also includes a gloomy vista upon the infamous [Pyramid of Doom](http://tritarget.org/blog/2012/11/28/the-pyramid-of-doom-a-javascript-style-trap/).

![Pyramid of Doom](images/pyramid_of_doom.png)

If you find yourself on a path to the Callback Hell, rethink your approach. Read the [recommendations by Max Ogden](http://callbackhell.com/):

- Name your functions (instead of using anonymous functions everywhere).
- Keep your code shallow.
- Split your code into smaller modules.

### Callbacks

If you write a library which accepts callback for parameters, stick to [_error-first_ convention](http://thenodeway.io/posts/understanding-error-first-callbacks/). You should pass an error as the first parameter and result of the call as second.

```js
function calculateAsync (callback) {
  try {
    // get result of some operation
    const result = calculate()
    callback(null, result)
  } catch(error) {
    // operation failed, return with error
    callback(error)
  }
}

```

For high-level operations with callbacks (i.e. such as resolving multiple callbacks into one), take a look at some library such as [async](https://github.com/caolan/async).

### Promises

A better approach may be using [Promises](https://developer.mozilla.org/en-US/docs/Mozilla/JavaScript_code_modules/Promise.jsm/Promise), which let you chain asynchronous operations without nesting:

```js
doAsync()
.then((data) => {
  return doMoreAsync()
})
.then((data) => {
  return evenMoreAsync()
})
.then((data) => {
  // we’re done
})
```

Native promises are only partially supported in [some browsers](http://caniuse.com/#feat=promises) and environments, so most applications rely on promise libraries like [Bluebird](https://github.com/petkaantonov/bluebird) which also provide some extra features. However, consider if introducing such a dependency is worth it, especially if you don’t do too much async operations.

Existing callback-based API can be easily converted to promises. Most libraries [provide `promisify`](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisification) and similar methods to convert existing callback-based functions to return promises (especially if they stick with [error-first convention](#callbacks)). Alternatively, you can [write similar wrapper yourself](http://stackoverflow.com/a/22519785/240963).

### Recommended Reading

- [Callback Hell](http://callbackhell.com/)
- [Control Flow](https://raynos.github.io/presentation/shower/controlflow.htm), a talk by Jake Verbaten
- [Callbacks are imperative, promises are functional: Node’s biggest missed opportunity](https://blog.jcoglan.com/2013/03/30/callbacks-are-imperative-promises-are-functional-nodes-biggest-missed-opportunity/) and [Callbacks, promises and simplicity](https://blog.jcoglan.com/2013/04/01/callbacks-promises-and-simplicity/)
- [From Callback to Future → Functor → Monad](https://medium.com/@yelouafi/from-callback-to-future-functor-monad-6c86d9c16cb5)
- [Promise anti-patterns](https://github.com/petkaantonov/bluebird/wiki/Promise-anti-patterns)
- [What Color is Your Function?](http://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/) (on general handling of asynchronous behaviour)

<!--
- [Taming Asynchronous Programming (Beyond Promises)](https://eamodeorubio.github.io/tamingasync/)
-->

## The Harmful Parts

JavaScript got some bad reputation, partly for its troubled past and incompatibility across implementations in various browsers, and partly for just being a misunderstood language. Some features of JavaScript look deceptively similar to other languages (typically Java), but in JavaScript they just _work different_. But many times these frustrations are self-inflicted; you can easily avoid the problematic features altogether.

We particularly draw from Douglas Crockford’s recommendations, or what he calls “the good parts” of JavaScript.

Resources:

* Book: [Douglas Crockford: JavaScript the Good Parts](http://shop.oreilly.com/product/9780596517748.do)
* [Douglas Crockford – The Better Parts](https://www.youtube.com/watch?v=PSGEjv3Tqo0) (Nordic.js 2014)

### `this` Keyword

`this` in JavaScript is a typical source of frustration. Based on our familiarity with class-based languages (Java, C#, PHP…) we may _assume_ it always points to the current object. However, the meaning of `this` depends on invocation of the function where it is used.

This way we can pass our objects to native methods as `this`:

```js
Array.prototype.slice.call(arrayLikeObject) // Array.slice() will operate on arrayLikeObject
```

But hardly ever we want to settle for such behaviour in our code.

`this` keyword in functions defaults to the _global object_ (i.e. `window` in browsers), which is extremely dangerous and lead to particularly nasty bugs:

```js
// very bad
function setThis () {
  this.foo = 'foo set'
}
setThis()
window.foo // => 'foo set'
```

With [strict mode](#strict-mode) `this` will be undefined and buggy code will crash early insted.

```js
function setThis () {
  'use strict'
  this.foo = 'foo set'
}
setThis() // => TypeError: Cannot set property 'foo' of undefined
```

Typically we use `this` keyword in [prototypal inheritance](#prototypes), or with [ES6 classes](#classes). The issue is with asynchronous code. Typically you want to call a method on your instance from the callback, but `this` doesn’t point to your original instance.

Coders typically avoid these issues by closure, i.e. storing `this` to an outside variable:

```js
function foo () {
  const that = this
  somethingThatRebindsThis(function() {
    that.whatever()
  })
}
```

It is preferred to avoid it with [`bind` function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) or [arrow functions](#anonymous-functions) in ES6 (where `this` is always bound to the outside scope). But as you can imagine, heavy reliance on `this` leads to verbose and unreadable code since we are working around JavaScript’s behaviour in attempt to make it behave like something else. We can certainly blame JavaScript for not being Java/C#/PHP/whatever, but perhaps we should blame ourselves for preferring familiarity over simplicity.

So **try to avoid `this`**. If you find yourself repeating `this` all over your code or fighting with binding, reconsider your approach. Perhaps you are using an [inappropriate abstraction](#classes), [mutating state](#immutability) or [relying on side effects](#pure-functions).

### `new` Operator

We typically use `new` to instantiate objects in class-based languages. But technically there are no classes in JavaScript, only functions. So the [`new` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) is instead a _syntactic sugar_ for object instantiation, which calls your constructing function with a correct binding for `this`.

```js
function Car (make, model, year) {
  this.make = make
  this.model = model
  this.year = year
}
const trabant = new Car('Trabant', '601', 1964)
```

Now, consider what happens if you call `Car` function without the `new` operator? If you read [previous section](#this-keyword), you probably know: `this` is a global object and you end-up setting properties on it – unless you [use the strict mode](#strict-mode).

```js
const trabant = Car('Trabant')
trabant // => undefined
make // => 'Trabant'
```

There are [techniques](http://elegantcode.com/2010/12/21/basic-javascript-part-4-enforcing-new-on-constructor-functions/) to guard your constructors from this misuse and [ES6 transpiler](#es6) can take care of necessary boilerplate code for you. But perhaps it is unnecessarily complicated to treat constructors and standard functions differently. You can easily avoid `new` operator by treating your constructors as simple functions which construct objects:

```js
function Car (make, model, year) {
  return Object.freeze({
    make,
    model,
    year
  })
}

const tesla = Car('Tesla', '3', 2016)
```

_Note the use of [ES6 property shorthands](#es6-shorthands). `Object.freeze` is optional but [recommended](#objects)._

#### But How About Performance?

Some JavaScript engines can optimise object creation with `new`. Additionally [prototypal inheritance](#prototypal-inehritance) should have smaller memory consumption, since you don’t duplicate properties. First thing: object creation performance hardly matters for most applications, unless you need to create hundreds of objects within milliseconds. The difference can be between allocation of 2 or 30 objects within _one microsecond_ (you have 16.67 _milliseconds_ for each frame in a browser, which uses 60 FPS). Your biggest enemy then will be garbage collector, which can kick in any time and kill your application performance. So _if_ you need to work with huge amounts of objects, perhaps you should [use object pool](http://www.html5rocks.com/en/tutorials/speed/static-mem-pools/) to avoid object allocation in the first place. Otherwise [do the sanity check](http://blog.getify.com/sanity-check-object-creation-performance/).

### Recommended Reading

- [JavaScript without “new” and “this”keywords ](https://nemisj.com/js-without-new-and-this/)
- [“new” Only Makes Javascript OO Harder](http://www.ianbicking.org/blog/2013/04/new-considered-harmful.html)
- [Javascript without the this](http://radar.oreilly.com/2014/03/javascript-without-the-this.html)


## Functional Programming

While JavaScript does not emphasise any particular programming paradigm, it goes especially well with some aspects of functional programming. Learn about functional programming and embrace it in your code. While you may have heard of some scare stories about it and/or you may think it is highly impractical stuff made up by some mathematicians, the reality is quite different.

Recommended reading:

* [Professor Frisby’s Mostly Adequate Guide to Functional Programming](http://drboolean.gitbooks.io/mostly-adequate-guide/)

The following section are few tips on how to start with functional style.

### Separate Data and Behaviour

**Put data into “dumb” structures and behaviour into functions.** Mainstream “object-oriented” programming languages bundle behaviour and data into classes. Many times, however, we operate with same data in different contexts. The fabulous OOP then becomes about managing inevitable complexity as we need to do various operations with same data, but classes and inheritance are extremely poor way to manage things.

If you need to do something with your data, just put them into object or array and write a simple function to do stuff. No need to write `DoStuffWithDataExecutor` class to encapsulate your data. Functions in JavaScript are first-class citizens, treat them as such.

Recommended reading: [Execution in the Kingdom of Nouns](http://steve-yegge.blogspot.cz/2006/03/execution-in-kingdom-of-nouns.html).

```js
// bad
class Person {
  constructor (first, last) {
    [this.first, this.last] = [first, last]
  }
  fullName () {
    return `${this.first} ${this.last}`
  }
}

const jane = new Person('Jane', 'Doe')
jane.fullName() // => 'Jane Doe'

// good
function fullName (person) {
  return `${person.first} ${person.last}`
}

const jane = {
  first: 'Jane',
  last: 'Doe',
}

fullName(jane) // => 'Jane Doe'
```


### Immutability

**Avoid mutating state.** Instead of modifying the original object, return a new object with changes.

```js
// bad
function rename (person, newName) {
  person.name = newName
  return person
}

const jane = { name: 'Jane' }
const anna = rename(jane, 'Anna')
jane.name // => 'Anna'
anna.name // => 'Anna'

// good
function rename (person, newName) {
  const clone = Object.assign({}, person)
  clone.name = newName
  return clone
}

const jane = { name: 'Jane' }
const anna = rename(jane, 'Anna')
jane.name // => 'Jane'
anna.name // => 'Anna'
```

This example uses [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) which is not yet widely supported. Most utility libraries provide `extend` or `clone` function, or you can use some [standalone implementation](http://npm.im/clone).

Arrays can be efficiently manipulated through standard functional methods like [`map`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/map) or [`filter`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) which return a new array.

```js
const words = ['foobar', 'baz', 'a']
const lengths = []

// really bad
for (let i = 0; i < words.length; i++) {
  let len = words[i].length
  lengths.push(len)
}

// bad
words.forEach((word) => {
  lengths.push(word.length)
})

// good
const lengths = words.map(word => word.length)

lengths // => [6, 3, 1]
```

`forEach` usually implies a side effect, in this case it is manipulating an outside array.

If you need to manipulate larger collections, consider using advanced libraries for immutable structures like [Immutable.js](https://facebook.github.io/immutable-js/) or [Mori](https://swannodette.github.io/mori/).

Recommended reading:

* [The Dao of Immutability](https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd)

### Pure Functions

**Avoid side-effects and outside state.** [Pure functions](http://drboolean.gitbooks.io/mostly-adequate-guide/content/ch3.html) always return the same output given the same parameters. However, since programming without side-effects would be impossible, try to contain it in dedicated functions and keep the rest of your code pure.

```js
// impure
const majorityAge = 21

function isUnderage (age) {
  return age < majorityAge
}

// pure
function isUnderage (age) {
  const majorityAge = 21
  return age < majorityAge
}
```

### Currying

The pure functions are easily testable and predictable. But don’t fret, you won’t need to pass _every single_ parameter _every time_ you call your function. With high-order functions, you can build your functions incrementally with [_currying_](http://drboolean.gitbooks.io/mostly-adequate-guide/content/ch4.html).

```js
// bad

function isUnderage (majorityAge, age) {
  return age < majorityAge
}

isUnderage(21, 19) // => true
isUnderage(18, 19) // => false
```

```js
// good

function underageChecker (majorityAge) {
  return function (age) {
    return age < majorityAge
  }
}

const isUnderageUsa = underageChecker(21)
const isUnderageEu = underageChecker(18)

isUnderageUsa(19) // => true
isUnderageEu(19) // => false

// treating curried function as anonymous does not look pretty
underageChecker(21)(19) // => true
```

Many libraries provide universal curry function, e.g. [Ramda](http://ramdajs.com/docs/#curry), or you can use [a stand-alone module](http://npm.im/curry-d).

```js
// better
const underageChecker = curry((majorityAge, age) => {
  return age < majorityAge
})

const isUnderageUsa = underageChecker(21)
isUnderageUsa(19) // => true

// works the same as before
underageChecker(21)(19) // => true

// but you can also pass all the parameters
underageChecker(21, 19) // => true
```

#### Parameters Order

When currying, keep the order of parameters in mind: you want to put the most variable parameter, i.e. data, as the last parameter. This allows you to easily compose functions.

For example popular libraries [Underscore](http://underscorejs.org/) and [lodash](https://lodash.com/) put data as the first parameter.

Recommended watching: [Hey Underscore, You’re Doing It Wrong!](https://www.youtube.com/watch?v=m3svKOdZijA).

```js
// bad
function firstTwoLetters (words) {
  return _.map(words, (word) => {
    return _.take(word, 2)
  })
}

firstTwoLetters(['foo', 'bar']) // => ['fo', 'ba']
```

On the other hand, functions in [Ramda](http://ramdajs.com) are curried and take data as the last parameter, so you can keep boilerplate to minimum.

```js
// good
const firstTwoLetters = R.map(R.take(2))

firstTwoLetters(['foo', 'bar']) // => ['fo', 'ba']
```

### Functional Composition

Pure functions and currying are powerful tools for manipulating data. You can [compose generic functions](http://drboolean.gitbooks.io/mostly-adequate-guide/content/ch5.html) to suit your specific needs with almost none code.

Use either `compose`, or `pipe` to combine functions into new functions. The only difference is order of functions:

Compose applies functions outside-in, i.e. `compose(a, b, c)(x)` is the same as `a(b(c(x)))`; so functions are applied from _right to left_.

Pipe does the opposite, i.e. `pipe(a, b, c)(x)` is the same as `c(b(a(x)))`; functions are applied from _left to right_.

While we find `pipe` more natural, use one , but be consistent within the project.

Try to keep your functions [pointfree](http://drboolean.gitbooks.io/mostly-adequate-guide/content/ch5.html#pointfree) to make them easier to extract and reuse.

```js
// not pointfree because we mention the data: word
function snakeCase (word) {
  return word.toLowerCase().replace(/\s+/ig, '_')
};

// pointfree, with some help from Ramda
import { toLower, replace, pipe } from 'ramda'
const snakeCase = pipe(toLower, replace(/\s+/ig, '_'))

snakeCase('ChunkyBacon') // => chunky_bacon
```

## Acknowledgements

This guide is based upon [JavaScript Quality Guide](https://github.com/bevacqua/js) by Nicolas Bevacqua and [AirBnB Style Guide](https://github.com/airbnb/javascript). Some examples are based on [Professor Frisby’s Mostly Adequate Guide to Functional Programming](http://drboolean.gitbooks.io/mostly-adequate-guide/). Many thanks to the original authors and [countless](https://github.com/bevacqua/js/graphs/contributors) [contributors](https://github.com/airbnb/javascript/graphs/contributors).

## License

MIT

> Fork away!
