---
title: wiklajs
---

Simple site for submitting answers for the exercises on the course "Ohjelmointitekniikka: JavaScript".

Group members:

- veeti "walther" haapsamo
- pauli "iluaP" niva
- ville "vvainio" vainio
- niko "aozi" novitsky

## Week 1

### Types

JavaScript is a _loosely_ typed or a _dynamic_ language:

- Primitive datatypes:
    - Boolean
    - Null
    - Undefined
    - Number
    - String
    - Symbol (ECMAScript 6)
- and Object

### Variable hoisting

```javascript
var sum = 21;

function alertSum() {
    if (false) {
        var sum = 42;
    }

    alert(sum);
}

alertSum();
```

Alerts `undefined`, not `42` or `21`!

Why? Because the JavaScript compiler interprets the code as follows:
```javascript
var sum = 21;

function alertSum() {
    var sum;

    if (false) {
        sum = 42;
    }

    alert(sum);
}

alertSum();
```

### Equality

In JavaScript, there is a big difference between the functions `==`and `===`. See some differences in action:

```javascript
// These return true:
0     == "0"
0     == ""
false == "0"
null  == undefined
" "   == 0

// These return false:
""    == "0"
false == "false"
false == undefined
false == null

// However, all these return false:
0     === "0"
0     === ""
false === "0"
null  === undefined
" "   === 0

```

These are caused by *type coercion*: when using a simple `==`for checking equality, the types of the parameters are coerced to match. A string is happily converted into a number, and so on. In some cases this might be useful, but it can also be a big source of confusion - use caution when using `==`. Usually it is better to just use `===` for stricter and more logical checking. In addition to making more sense, `===` can provide a performance boost, as there is no overhead from type conversion from coercion.

### Functions for checking data types

Here are some example functions for checking the types of values.

```javascript
function isNumber(a) {
    return !isNaN(a) && typeof a === "number";
}

function isInt (a) {
    return isNumber(a) && a % 1 === 0; // remainder with 1 must be explicitly 0
}

function isString(a) {
    return typeof a === "string";
}

function isArray(a) {
    return Array.isArray(a);
}

function isArrayOfNumbers (a) {
    return isArray(a) && a.every(isNumber);
}

function isArrayOfInts (a) {
    return isArray(a) && a.every(isInt);
}

function isArrayOfString (a) {
    return isArray(a) && a.every(isString);
}

function isBoolean(a) {
    return typeof a === "boolean";
}

function isUndefined(a) {
    return typeof a === "undefined";
}

function isNull(a) {
    return a === null;
}

// Tests

// number
console.assert(true === isNumber(5), "isNumber 5");
console.assert(false === isNumber("5"), 'not isNumber "5"');
// int
console.assert(true === isNumber(5), "isNumber 5");
console.assert(false === isNumber("5"), 'not isNumber "5"');
// string
console.assert(true === isString("5"), 'isString "5"');
console.assert(false === isString(5), 'not isString 5');
// array
console.assert(true === isArray(["5"]), 'isArray ["5"]');
console.assert(false === isArray(5), 'not isArray 5');
// number array
console.assert(true === isArrayOfNumbers([5]), 'isArrayOfNumbers [5]');
console.assert(false === isArrayOfNumbers(5), 'not isArrayOfNumbers 5');
// int array
console.assert(true === isArrayOfInts([5]), 'isArrayOfInts [5]');
console.assert(false === isArrayOfInts([5.2]), 'not isArrayOfInts [5.2]');
// string array
console.assert(true === isArrayOfNumbers([5]), 'isArrayOfNumbers [5]');
console.assert(false === isArrayOfNumbers(["5"]), 'not isArrayOfNumbers ["5"]');
```

## Week 2

### Algorithm styles

### Functional vs imperative

Officially, JavaScript is a multi-paradigm language: it allows for simple scripting as well as object-oriented programming, imperative as well as functional programming. Here we will focus on the two perhaps most opposing paradigms: functional versus imperative.

From the traditional imperative paradigm, JavaScript has the common structures of `while` and `for`-loops, `if`-`else` -blocks, `switch`-cases and so on. However, with the advent of ECMAScript 6, lots of functional features have been brought in, including `map`, `every`, `let` and so on. For a comprehensive list, see for example [this](http://es6-features.org/) website!

Specific functions are not the paradigm's defining features though. Let's go deeper with some examples:


**Imperative approach**

~~~{.javascript}
var nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

var even = function(a) {
  var ret = []
  for (var i=0; i<nums.length; i++) {
    if (a[i] % 2 === 0) {
      ret.push(a[i]);
    }
  }
  return ret;
}

console.log(even(nums));
~~~

**Functional approach**

~~~{.javascript}
var nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

var even = xs => xs.filter(x => x % 2 == 0)

console.log(even(nums));
~~~

See, we can make JavaScript look more like Haskell than C!


### Closures

### Exceptions

<link rel="stylesheet" type="text/css" href="http://walther.guru/hilightjs_monokai.css">
<script src="http://walther.guru/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
