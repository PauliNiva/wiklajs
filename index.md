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

All joking aside, it is wonderful that JavaScript has support for both styles. This allows users to code in a way familiar to them or in the best ways relevant to the use-case at hand. However, in many cases functional style allows for shorter and more concise expression, which aids readability and hence maintainability of the code base.

**TODO: improve this section. Not a lot of good content yet :(**

### Exceptions

Runtime errors, more familiarly known as exceptions are errors that happen while the program is running.
Exceptions can be handled with JavaScript's error handling, that is, with try statements.
The try statement consists of a try block, which contains one or more statements, and at least
one catch clause or a finally clause, or both. That is, there are three forms of the try statement<br />

1. try..catch<br />
2. try..finally<br />
3. try..catch..finally<br />

A catch clause specifies what to do if an exception is thrown in the try block.
If the try block does not succeed, that is, if any statement within the try block
(or in a method called from within the block) throws an exception, control shifts
to the catch clause. If no exception is thrown in the try block, the catch clause is skipped.
The finally clause always executes, regardless of whether or not an exception was thrown or caught.
It executes after the try block and catch clause or clauses but before any statements following the try statement.
Try statements can be nested. If an inner try statement does not have a catch clause, the enclosing try statement's
catch clause is entered.

When a single, unconditional catch clause is used, the catch block is entered when any exception is thrown.
One can also use one or more conditional catch clauses to handle specific exceptions. This functionality is not
part of the ECMAScript specification though. But in case you decide to use this kind of functionality, then the appropriate
catch clause is entered when the specified exception is thrown. When an exception occurs, control is transferred
to the appropriate catch clause. If the exception is not one of the specified exceptions and an unconditional catch
clause is found, control transfers to that one.

ECMAScript specifies six different kinds of exceptions.<br />

1. EvalError, not in use, only included for backward compatibility's sake.<br />
2. RangeError, if numerical value exceeds its range.<br />
3. ReferenceError, for invalid references.<br />
4. SyntaxError, for syntactical errors.<br />
5. TypeError, if type deviates from expected type.<br />
6. URIError, if global URI handling method are used improperly.<br />

When an exception is thrown in the try block, exception variable (the e in catch (e)) holds the value specified
by the throw statement. This identifier can be used to get information about the thrown exception.
This identifier is local to the catch clause. That is, it is created when the catch clause is entered, and after it
finishes executing, the identifier is not available anymore.

The finally clause contains statements that are to be executed after the try and catch clauses execute, but before any statements
following the try statement. As said earlier the finally clause executes regardless of whether an exception was thrown.
If exception is thrown, then the statements in the finally clause will execute even if catch clause did not handle the exception.
One can use the finally clause to make scripts fail gracefully when an exception occurs.

If the finally clause returns a value, then this value becomes the return value of the entire try statement, regardless of
any return statements in the try and catch blocks.

### Closures

Explaining what a closure is, is a bit difficult. It's easier to demosntrate it and expand from the examples provided. 

Here is a simple function that uses a closure
```javascript
function sayHello(name) {
    var text = 'Hello ' + name; // Local variable
    var sayAlert = function() { alert(text); }
    return sayAlert;
}
´´´

var say2 = sayHello('Arto');
say2(); // alerts "Hello Arto"

So what exactly is going on here? We're first defining a new variable, we use the sayHello function with a parameter.

The sayHello function defines its own variable called text, however the text itself is not returned. Rather we define another function inside the sayHello function. This is the function that actually displays any text.

The interesting thing about closures, is that they can access all data contained in the environment where they were created. So in this case, the closure sees the text variable and can access it.

And finally, we actually return the sayAlert variable, which in itself is a function, that uses variables defined in the sayHello function.

Usually in programming languages when you return from a function, any reference to that function itself is removed. So in this case, the variable text should not be accessible since the function environment no longer exists.

Yet by calling the function with say2(); we get the print defined in the text variable. Why is this?

As stated previously, a closure can access any data from the environment where it was created, however the closure also maintains a reference to the environment where it was defined, and this has a lot of interesting proeprties that can be used and abused (like most things in javascript...)

Here's a function that clearly shows that the variables are kept in reference, rather than just copied.

```javascript
function say666() {
    // Local variable that ends up within closure
    var num = 666;
    var sayAlert = function() {num++; alert(num); }
    return sayAlert;
}
var sayNumber = say666();
sayNumber(); // alerts 667
sayNumber(); // alerts 668
sayNumber(); // alerts 669
´´´

Each call to the sayNumber object we defined, increments the result by one, the old result is also stored. So it is clear that reference to the original environment is kept, and the values are not just copied inside the closure.

This means that when we increment the number inside the closure, it's not stored in the closure, rather the closure accesses the parent function, and stores the number there, where it then again accesses it during the next call.

Now here's somethign a bit more interesting, what if we define global closure functions, inside a function?
```javascript
function setupSomeGlobals() {
    // Local variable that ends up within closure
    var num = 666;
    // Store some references to functions as global variables
    gAlertNumber = function() { alert(num); }
    gIncreaseNumber = function() { num++; }
    gSetNumber = function(x) { num = x; }
}

setupSomeGlobals();
´´´
Right now we're not even instantiating an object, we're simply calling a function that defines it's variable, and few global functions.

So lets poke the stuff inside the function and see what happens!

```javascript
gAlertNumber(); //prints 666
gIncreaseNumber();
gAlertNumber(); //667
setupSomeGlobals();
gAlertNumber(); //prints 666!
gSetNumber(5);
gAlertNumber(); //prints 5
´´´

All of these functions refer to the exact same closure!
And when we call the original function again, the values are reset! Unless....


```javascript
var oldIncrease = gIncreaseNumber;
var oldValue = gAlertNumber;
´´´

We store the reference into a new variable!

```javascript
setupSomeGlobals();
oldValue(); //Prints 5
oldIncrease();
oldValue(); //Prints 6!
gAlertNumber(); //Prints 666!
´´

So even though we defined global functions that all refer to the exact same closure, if we store a reference to these functions, they execute in the environemnt they were defined in! Even after the function terminates, even after we reset the closure where the global functions point to, if we store a reference to a function, it still executes within the environment that was in place when we define it! So the reference still exists there somewhere!

This has some unfortunate implications, that are still interesting to consider. 

function makeList(list) {
    var result = [];
    for (var i = 0; i < list.length; i++) {
        var item = 'item' + i;
        result.push( function() {alert(item + ' ' + list[i])} );
    }
    return result;
}

var weird = makeList([1,2,3,4,5]);

//testing the list
for (var j = 0; j < weird.length; j++) {
    weird[j]();
}

Looking at this, you would expect the function to generate a list that prints the item number on the screen for every item on the list. However what you get might be surprising.....

The function actually prints "item4 undefined" five times......Why? Because closures are evaluated in the environment they were defined in.

So when we first generate the list, we loopthrough the list we give as a parameter;

    for (var i = 0; i < list.length; i++) {
        var item = 'item' + i;
        result.push( function() {alert(item + ' ' + list[i])} );
    }
    
Thus far pretty easy, however we then use the i variable to index the elements....Because that's what we do with for loops. But thigns go weird with closures, we use a closure in the actual return function.

    result.push( function() {alert(item + ' ' + list[i])} );

So we push a new function to the list, a function which generates an alert based on the current index of the loop....except it doesn't. Because closures store the refenrence environment and use that to deal with variables, and in addition they are evaluated when called, not when generated.

We use the function to generate the alert, and store that in the result list. But when we try to call the function of the specific item on the list, the closure goes and sees that the variable i, which we used for our loop, is actually 4, because we iterated through the list!

So it simply sees that i is 4 and since nothing changes it, everything is evaluated to item4 undefined.



<link rel="stylesheet" type="text/css" href="http://walther.guru/hilightjs_monokai.css">
<script src="http://walther.guru/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
