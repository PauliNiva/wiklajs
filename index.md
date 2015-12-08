---
title: wiklajs
---

JavaScript Best Practices - created with ❤ at the course "Ohjelmointitekniikka: JavaScript" arranged by Arto Wikla at the University of Helsinki

Group members:

- Veeti "walther" Haapsamo
- Pauli "iluaP" Niva
- Niko "aozi" Novitsky
- Ville "vvainio" Vainio

## Prequisites

In this document, we are referring to varying JavaScript feature levels, including the upcoming ES6. ES6 hasn't been standardized yet, so some browsers might have difficulties.

One solution for this problem is [Babel](https://babeljs.io) - a compiler for writing next-generation JavaScript. Babel compiles source code written in ECMAScript 6 (ES2015) to ECMAScript 5 (ES5) syntax. The transpiled source-code can then be run on any environment supporting ES5 (Internet Explorer 9 for example).

## Being safe with JavaScript

### Types

JavaScript is a _weakly_ typed or a _dynamic_ language.

Datatypes in JavaScript:

- Boolean
- Null
- Undefined
- Number
- String
- Object
- Symbol (ECMAScript 6)

### Type checkers and Linters

Due to the dynamic nature of JavaScript, type errors are common and extra carefulness is required when programming. For example, automatic type coercion might result in unexpected behavior when attempting to submit data to a backend.

Lack of static typing might also slow developers down as bugs are harder to find (e.g. debugging the root cause of a crash) and code maintenance is more difficult (e.g. refactoring is risky without complete knowledge of dependencies). The recommended way to minimize such problems is to implement basic type checking.

An example of a function that would normally result in a crash when executing:

```javascript
function length(x) {
    return x.length;
}

var total = length('Hello') + length(null);
```

### Libraries

There are numerous methods to get around the issues of type safety. The simplest way is to write your own functions for type checking,this however is not advised since existing solutions exist. Such as [Iodash](https://lodash.com) or [Predicates](https://github.com/wookieb/predicates).

### Linters

You can also use Linters. Linters are code-quality tools that detect problems in the source code. They scan the source code for syntax errors, style conventions and structural problems. In case of a problem, a message describing the problem and its approximate location in the source code is displayed. [JSLint](http://www.jslint.com/help.html) is an example of such tool, but is heavily opinionated and provides no way to add custom rules.

An alternative to [JSLint](http://www.jslint.com/help.html) is [JSHint](http://jshint.com/about) which is customizable and can be easily adjusted to match particular coding guidelines and conventions.

### Static type checkers

Static type checkers are different from linters, they are more like compilers that often require some specific syntax in the code itself. They are generally superior to linters and provide more functionality. [Flow](http://flowtype.org/) by Facebook and [Google Closure compiler](https://developers.google.com/closure/compiler/?hl=en) are two examples of static type checkers.

Flow would catch the error in the example provided above at compile time, and point out that `x` can be null (so its length property should not be accessed).

### Meta languages

Finally, you can utilize meta languages that compile into JavaScript and provide better type safety such as [Dart](https://www.dartlang.org/), [TypeScript](http://www.typescriptlang.org/), [PureScript](http://www.purescript.org/), [asm.js](http://asmjs.org/) and numerous others. 

### Conclusion

The benefits of dynamic typing are immense and often overcome the aforementioned problems. Dynamic typing results in more concise & less verbose code, enables easy use of "hacks" like duck-typing and monkey-patching, and makes it easier to write generic code.

However, it is generally recommended to write type-checked, strict and validated code, to reduce the amount of bugs and unexpected behavior.

## JavaScript styles

### Algorithms & programming styles

Algorithms can be written in JavaScript just like with any other programming language.

When writing algorithms in JavaScript, functions should be kept short to increase legibility and to reduce the need of tracking different variable types. Declaring the types in comments is often used to describe the types of arguments passed. It's a good idea to follow the [JSDoc](https://github.com/jsdoc3/jsdoc) syntax in the comments. Several IDE's, compilers and linters can extract type information and generate documentation from this syntax.

```javascript
/*
 * @param {String} x
 * @param {String} y
 * @return {Boolean}
 */
function match(x, y) {
    return x === y;
}
```

Same functionalities can be written in several ways, next is an example for getting the nth Fibonacci number.

Utilizing a loop:

```javascript
function fibonacci(n) {
    var a = 0, b = 1, f = 1;
    for(var i = 2; i <= n; i++) {
        f = a + b;
        a = b;
        b = f;
    }
    return f;
}

> fibonacci(12)
144
```

As a recursive function:

```javascript
function fibonacci(n){
    return n <= 1 ? n : (fibonacci(n-1) + fibonacci (n-2));
}

> fibonacci(12)
144
```

Here are some [example implementations of various algorithms](https://github.com/mgechev/javascript-algorithms)

### Functional vs imperative

Officially, JavaScript is a multi-paradigm language: it allows for simple scripting as well as object-oriented programming, imperative as well as functional programming. Here we will focus on the two perhaps most opposing paradigms: functional versus imperative.

From the traditional imperative paradigm, JavaScript has the common structures of `while` and `for`-loops, `if`-`else` -blocks, `switch`-cases and so on. However, with the advent of ECMAScript 6, lots of functional features have been brought in, including `map`, `every`, `let` and so on. For a comprehensive list, see for example [this](http://es6-features.org/) website!

Specific functions are not the paradigm's defining features though. Let's go deeper with some examples:


**Imperative approach**

```javascript
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
```

Here we have a list of `nums`, integers from 1 to 10. We define `even`, a function that accumulates a list of return values by going through all the elements of the input list and checking if the number has a remainder of 0 when divided by 2. At the end, the `ret` list is returned.

**Functional approach**

```javascript
var nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

var even = xs => xs.filter(x => x % 2 == 0)

console.log(even(nums));
```

Here we define `even` with arrow notation as a function that is essentially a lambda: input values xs tend towards a filtered version of xs, where the filter criterion is a lambda of divisibility by two.

See, we can make JavaScript look more like Haskell than C!

All joking aside, it is wonderful that JavaScript has support for both styles. This allows users to code in a way familiar to them or in the best ways relevant to the use-case at hand. However, in many cases functional style allows for shorter and more concise expression, which aids readability and hence maintainability of the code base. Another interesting effect of using functional approach, arrow notation and lambdas is that we do not explicitly define return arrays or any other state at all; the code is *pure*.

## JavaScript practicalities

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

A good practice is to declare each variable at the beginning of each function in order to avoid unexpected behavior.

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

These are caused by *type coercion*: when using a simple `==`for checking equality, the types of the parameters are coerced to match. A string is happily converted into a number, and so on. In some cases this might be useful, but it can also be a big source of confusion - use caution when using `==`.

Usually it is better to just use `===` for stricter and more logical checking. In addition to making more sense, `===` can provide a performance boost, as there is no overhead from type conversion from coercion.

### Exceptions in Javascript

Runtime errors, more familiarly known as exceptions are errors that happen while the program is running. Exceptions can be handled with JavaScript's error handling, that is, with try statements. The try statement consists of a try block, which contains one or more statements, and at least one catch clause or a finally clause, or both.

There are three forms of the try statement:

```javascript
try {
    Block of code to try
}
catch(err) {
    Block of code to handle errors
}
finally {
    Block of code to be executed regardless of the try..catch result
}
```

A `catch` clause specifies what to do if an exception is thrown in the `try` block. If the `try` block does not succeed, that is, if any statement within the `try` block (or in a method called from within the block) throws an exception, control shifts to the `catch` clause. If no exception is thrown in the `try` block, the `catch` clause is skipped.

The `finally` clause always executes, regardless of whether or not an exception was thrown or caught. It executes after the `try` block and catch clause or clauses but before any statements following the try statement.

`Try` statements can be nested. If an inner `try` statement does not have a `catch` clause, the enclosing `try` statement's `catch` clause is entered.

```javascript
try {
  try {
    throw new Error("inner");
  }
  finally {
    console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

// Output:
// "finally"
// "outer" "inner"
```
When a single, unconditional `catch` clause is used, the `catch` block is entered when any exception is thrown. One can also use one or more conditional `catch` clauses to handle specific exceptions. This functionality is not part of the ECMAScript specification though. But in case you decide to use this kind of functionality, then the appropriate `catch` clause is entered when the specified exception is thrown. When an exception occurs, control is transferred to the appropriate `catch` clause. If the exception is not one of the specified exceptions and an unconditional `catch` clause is found, control transfers to that one.

### Exceptions in ECMAScript

Here is how to implement the same conditional catch clauses using only simple JavaScript that does not conform to the ECMAScript specification:

```javascript
try {
    myroutine(); // may throw three types of exceptions
} catch (e if e instanceof TypeError) {
    // statements to handle TypeError exceptions
} catch (e if e instanceof RangeError) {
    // statements to handle RangeError exceptions
} catch (e if e instanceof EvalError) {
    // statements to handle EvalError exceptions
} catch (e) {
    // statements to handle any unspecified exceptions
    logMyErrors(e); // pass exception object to error handler
}
```

Here is how to implement the same conditional catch clauses using only simple JavaScript conforming to the ECMAScript specification:

```javascript
try {
    myroutine(); // may throw three types of exceptions
} catch (e) {
    if (e instanceof TypeError) {
        // statements to handle TypeError exceptions
    } else if (e instanceof RangeError) {
        // statements to handle RangeError exceptions
    } else if (e instanceof EvalError) {
        // statements to handle EvalError exceptions
    } else {
       // statements to handle any unspecified exceptions
       logMyErrors(e); // pass exception object to error handler
    }
}
```

ECMAScript specifies six different kinds of exceptions.

1. `EvalError`, not in use, only included for backward compatibility's sake.
2. `RangeError`, if numerical value exceeds its range.
3. `ReferenceError`, for invalid references.
4. `SyntaxError`, for syntactical errors.
5. `TypeError`, if type deviates from expected type.
6. `URIError`, if global URI handling method are used improperly.

When an exception is thrown in the try block, exception variable (the `e` in `catch (e)`) holds the value specified by the `throw` statement. This identifier can be used to get information about the thrown exception. This identifier is local to the `catch` clause. That is, it is created when the `catch` clause is entered, and after it finishes executing, the identifier is not available anymore.

The `finally` clause contains statements that are to be executed after the `try` and `catch` clauses execute, but before any statements following the `try` statement. As said earlier the `finally` clause executes regardless of whether an exception was thrown. If exception is thrown, then the statements in the `finally` clause will execute even if `catch` clause did not handle the exception. One can use the finally clause to make scripts fail gracefully when an exception occurs.

If the `finally` clause returns a value, then this value becomes the return value of the entire `try` statement, regardless of any return statements in the `try` and catch blocks.

The `throw` statement allows the creation of custom errors. Execution of the current function will stop (the statements after `throw` won't be executed), and control will be passed to the first `catch` block in the call stack. If no `catch` block exists among caller functions, the program will terminate.

The raised exception can be JavaScript `String, Number, Boolean or an Object`. Throw can be used alongside with try and catch to control program flow and generate custom error messages.

```javascript
throw "Something happened";   // throws a String
throw 42;                     // throws a number
throw true;                   // throws a Boolean
```

### Closures

Explaining what a closure is, is a bit difficult. It's easier to demonstrate it and expand from the examples provided.

Here is a simple function that uses a closure:

```javascript
function sayHello(name) {
    var text = 'Hello ' + name; // Local variable
    var sayAlert = function() { alert(text); }
    return sayAlert;
}

var say2 = sayHello('Arto');
say2(); // alerts "Hello Arto"
```

So what exactly is going on here? We're first defining a new variable, we use the `sayHello` function with a parameter.

The `sayHello` function defines its own variable called text, however the text itself is not returned. Rather we define another function inside the `sayHello` function. This is the function that actually displays any text.

The interesting thing about closures, is that they can access all data contained in the environment where they were created. So in this case, the closure sees the text variable and can access it.

And finally, we actually return the `sayAlert` variable, which in itself is a function, that uses variables defined in the `sayHello` function.

Usually in programming languages when you return from a function, any reference to that function itself is removed. So in this case, the variable text should not be accessible since the function environment no longer exists.

Yet by calling the function with `say2();` we get the print defined in the text variable. Why is this?

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
```

Each call to the `sayNumber` object we defined, increments the result by one, the old result is also stored. So it is clear that reference to the original environment is kept, and the values are not just copied inside the closure.

This means that when we increment the number inside the closure, it's not stored in the closure, rather the closure accesses the parent function, and stores the number there, where it then again accesses it during the next call.

Now here's something a bit more interesting, what if we define global closure functions, inside a function?

```javascript
function setupSomeGlobals() {
    // Local variable that ends up within closure
    var num = 666;
    // Store some references to functions as global variables
    gAlertNumber    = function() { alert(num); }
    gIncreaseNumber = function() { num++; }
    gSetNumber      = function(x) { num = x; }
}

setupSomeGlobals();
```

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
```

All of these functions refer to the exact same closure!
And when we call the original function again, the values are reset! Unless....


```javascript
var oldIncrease = gIncreaseNumber;
var oldValue = gAlertNumber;
```

We store the reference into a new variable!

```javascript
setupSomeGlobals();
oldValue();     //Prints 5
oldIncrease();
oldValue();     //Prints 6!
gAlertNumber(); //Prints 666!
```

So even though we defined global functions that all refer to the exact same closure, if we store a reference to these functions, they execute in the environment they were defined in! Even after the function terminates, even after we reset the closure where the global functions point to, if we store a reference to a function, it still executes within the environment that was in place when we define it! So the reference still exists there somewhere!

This has some unfortunate implications, that are still interesting to consider.

```javascript
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
```


Looking at this, you would expect the function to generate a list that prints the item number on the screen for every item on the list. However what you get might be surprising.....

The function actually prints `item4 undefined` five times......Why? Because closures are evaluated in the environment they were defined in.

So when we first generate the list, we loop through the list we give as a parameter;


```javascript
for (var i = 0; i < list.length; i++) {
    var item = 'item' + i;
    result.push( function() {alert(item + ' ' + list[i])} );
}
```

Thus far pretty easy, however we then use the `i` variable to index the elements....Because that's what we do with for loops, then things get weird. Because closures are weird.


```javascript
result.push( function() {alert(item + ' ' + list[i])} );
```

So we push a new function to the list, a function which generates an alert based on the current index of the loop....except it doesn't. Because closures store the reference environment and use that to deal with variables, and in addition they are evaluated when called, not when generated.

We use the function to generate the alert, and store that in the result list. But when we try to call the function of the specific item on the list, the closure goes and sees that the variable `i`, which we used for our loop, is actually 4, because we iterated through the list!

So it simply sees that `i = 4` and since nothing changes it, everything is evaluated to `item4 undefined`.

Closures can be extremely powerful tools when used properly, but in order to use them properly you really need to understand how they work. 


## JavaScript and Objects

### Creating objects in Javascript

There are numerous ways to create objects in javascript, each of them has its own special use and niche it fits in, so there is really no "best" solution. Every approach can be used in almost any circumstances but they all have their strengths and weaknesses.

Let's start with something simple, an object literal:

```javascript
var person = {
    name : "Wikla",
    getName : function (){
        return this.name
    }
}
```

We're creating an object called person with its own properties and function declared right there. This means we can't actually create instances of this object, so the main use of this is to create single objects.

Next up, the object constructor:

```javascript
var person = new Object();

person.name = "Wikla",
person.getName = function(){
    return this.name ;
};
```

This is slightly different, we're first using the `new` keyword to create an instance of the base class `Object`. We then proceed to use the traditional JavaScript syntax to add more fields to this new object. Javascript actually allows us to add new fields to an object easily.

Next up, the function constructor:

```javascript
function Person(name){
  this.name = name
  this.getName = function(){
    return this.name
  }
}
```

This would be the closest to something like Java. We're creating a new function that serves as the constructor function whenever we create a new instance of this object. This approach has the obvious benefit of being able to create multiple objects from the same constructor but with different properties.

Next up is the prototype way:

```javascript
function Person(){};
Person.prototype.name = "Wikla";
```

Now what's going on here? We're first creating a new function called Person, just like with functions constructors. We leave everything empty, but Javascript still creates  a constructor function that sets up all the basic fields javascript objects have, such as the `prototype` field.

The `prototype` field in this case, refers to the constructor function which is used to create an instance of the Person object. By adding new field to this, we're also adding new fields to any future instances created from this function.

We can also combine any of these approaches. For example, here's a combination of function constructors and prototypes:

```javascript
function Person(name){
  this.name = name;
}
Person.prototype.getName = function(){
  return this.name
}
```

So we first create a new function called `Person`, this function contains a single value `name`. We then use the prototype field like in the previous example, to add properties to the function prototype.

This time we add in an actual function that returns the name. Since we're adding this to the Person prototype, which has the property name already, this getName function can obviously see it.

One more thing worth mentioning is the `Object.create()` function. For the most part it works exactly the same as the new function, with one major difference.
`Object.create()` allows you to define the prototype of the object you're creating. When using a constructor function, the object inherits properties directly from the constructor's prototype. You can also define objects inside the object.create argument definitions.

To put it simple:

```javascript
var a = new Person();
```

Is equal to:
```javascript
var a = Object.create(Person.prototype);
```


As mentioned before, we can quite freely add fields to an object or the object prototype, in fact we can climb up the prototype chain using the `__proto__` field in order to assign new fields and properties to anything in the chain.

This is of course not very sensible nor advised, you can easily break literally everything by making a mistake in the prototype chain. But as a property of Javascript, it is a good idea to learn it and make use of it, carefully.

One very interesting property of javascript is the ability to dynamically name fields, simply put you can use variables to name any field or other variable inside an object.

```javascript
function foobar(arg1) {
   this.a = arg1;

   name = this['a'];
   alert(name);
}
new foobar("Javascript");
```

This, once again can be extremely powerful and extremely easy to screw up and break your entire codebase.

Ideally you'd want to stick with one type of Object declarations to make your code easily readable, branch out if the other styles provide you with some benefit you wouldn't get else where.

### Module pattern

Now how do you make the best use of objects in Javascript? One good thing to do is to make good use of modules. Modules are a design pattern in Javascript that allows you to easily create public and private functions, along with encapsulating data.

```javascript
MyNamespace.MyModule = function()
{
  var myPrivateProperty = 2;
  var myPublicProperty = 1;
  var myPrivateFunction = function()
{
  console.log("myPrivateFunction()");
};
var myPublicFunction = function()
{
  console.log("myPublicFunction()");
  myPrivateFunction();
};
var init = function()
{
  console.log("init()");
};
var oPublic =
{
  init: init,
  myPublicProperty: myPublicProperty,
  myPublicFunction: myPublicFunction
};
return oPublic;
}();

// Let's call our setup function....
console.log("Calling init()...");
MyNamespace.MyModule.init();
// And our public function.  Internally, this will call myPrivateFunction.
// This wil work because it's being called from inside the module.
console.log("Calling myPublicFunction()...");
MyNamespace.MyModule.myPublicFunction();
// This will throw an error because myPrivateFunction() is not accessible
// from outside the module.
console.log("Calling myPrivateFunction()...");
MyNamespace.MyModule.myPrivateFunction();
```

You might notice how the syntax for public and private properties is exactly the same. So how does this work exactly? Instead of creating an instance of the function we defined, the function itself returns an object literal that we define.

This function literal stores the functions definitions we have created and allows us to use the public properties, the public properties can use private properties since we have defined them inside the module. And all of this works primarily thanks to Closures which allow us to keep the contents of the function alive even after it is discarded by the garbage collector.

### Object.defineProperty()

The `Object.defineProperty()` method defines a new property directly on an object, or modifies an existing property on an object, and returns the object.

```javascript
Object.defineProperty(obj, prop, desc)
```

Function has three parameters:
`obj`, The object on which to define the property.
`prop`, The name of the property to be defined or modified.
`desc`, The descriptor for the property being defined or modified.

`Object.defineProperty()` allows precise addition or a modification of a property of an object. Property addition through assignment creates properties which show up when properties are enumerated. These values may be changed, and/or deleted.

`Object.defineProperty()` function allows these extra details to be changed from their defaults. Values added using `Object.defineProperty()` are immutable.

There are two kinds of descriptors in objects: data descriptors and accessor descriptors. A data descriptor is a property that has a value, which can be writable. An accessor descriptor is a property described by a getter-setter pair of functions. A descriptor must be either one of these. It cannot be both.

Both data and accessor descriptors are objects. They have the following common required keys:

1. `configurable`, true if and only if the descriptors type may be changed and if the property may be deleted from the corresponding object.
2. `enumerable`, true if and only if this property shows up during enumeration of the properties of the object. Both of these default to false.

Data descriptor has these two optional keys:

1. `value`, The value associated with the property. Can be any valid value (number, object, function, etc). This key defaults to undefined.
2. `writable`, true if and only if the value can be changed with an assignment operator. This key defaults to false.

Accessor descriptor has these two optional keys:

1. `get`...getter function for the property. Can be undefined if there is no getter. The function return will be used as the value of property.
2. `set`...setter function for the property, or undefined if there is no setter. The function will receive the new value being assigned to the property
as its only argument.

Both of these default to undefined.

Keep in mind that these options are not necessarily own properties so inherited
options are also considered. In order to ensure that these defaults are
preserved, programmer must freeze the Object.prototype beforehand,
explicitly specify all the options, or point to `null` as `__proto__` property.

```javascript
// using __proto__
var obj = {};
Object.defineProperty(obj, 'key', {
  __proto__: null, // no inherited properties
  value: 'static'  // not enumerable
                   // not configurable
                   // not writable
                   // as defaults
});

// being explicit
Object.defineProperty(obj, 'key', {
  enumerable: false,
  configurable: false,
  writable: false,
  value: 'static'
});

// recycling same object
function withValue(value) {
  var d = withValue.d || (
    withValue.d = {
      enumerable: false,
      writable: false,
      configurable: false,
      value: null
    }
  );
  d.value = value;
  return d;
}
// ... and ...
Object.defineProperty(obj, 'key', withValue('static'));

// if freeze is available, prevents adding or
// removing the object prototype properties
// (value, get, set, enumerable, writable, configurable)
(Object.freeze || Object)(Object.prototype);
```

### Creating a property

When the object does not have have the specified property, then`Object.defineProperty()` creates a new property as described. Fields can be omitted from the descriptor, and default values for those fields are considered. All Boolean fields default to false. The value of get and set fields default to undefined.

When a property is defined without get/set/value/writable, then it is
called *generic* and is *typed* as a data descriptor.

```javascript
var o = {}; // Creates a new object

// Example of an object property added with defineProperty with a data property descriptor
Object.defineProperty(o, 'a', {
  value: 37,
  writable: true,
  enumerable: true,
  configurable: true
});
// 'a' property exists in the o object and its value is 37

// Example of an object property added with defineProperty with an accessor property descriptor
var bValue = 38;
Object.defineProperty(o, 'b', {
  get: function() { return bValue; },
  set: function(newValue) { bValue = newValue; },
  enumerable: true,
  configurable: true
});
o.b; // 38
// 'b' property exists in the o object and its value is 38
// The value of o.b is now always identical to bValue, unless o.b is redefined

// You cannot try to mix both:
Object.defineProperty(o, 'conflict', {
  value: 0x9f91102,
  get: function() { return 0xdeadbeef; }
});
// throws a TypeError: value appears only in data descriptors, get appears only in accessor descriptors
```
### Modifying a property

When the property already exists, `Object.defineProperty()` attempts to modify the property according to the values that are in the descriptor. If the old descriptor had its configurable attribute set to false, that is the property is "non-configurable", then no attribute besides writable can be changed. In this case, it is not possible to switch back and forth between the data and accessor property types.

If a property is non-configurable, its writable attribute can only be changed to false. When attempts are made to change non-configurable property attributes a `TypeError` is thrown. (unless the current and new value are the same).

When the `writable` property attribute is set to false, then it cannot be reassigned and the property is said to be `non-writable`. Trying to write into the non-writable property does not throw an error.

```javascript
var o = {}; // Creates a new object

Object.defineProperty(o, 'a', {
  value: 37,
  writable: false
});

console.log(o.a); // logs 37
o.a = 25; // No error thrown (it would throw in strict mode, even if the value had been the same)
console.log(o.a); // logs 37. The assignment didn't work.
```

### Enumerable attribute

The enumerable property attribute defines if the property shows up in a
for...in loop and Object.keys().

```javascript
var o = {};
Object.defineProperty(o, 'a', { value: 1, enumerable: true });
Object.defineProperty(o, 'b', { value: 2, enumerable: false });
Object.defineProperty(o, 'c', { value: 3 }); // enumerable defaults to false
o.d = 4; // enumerable defaults to true when creating a property by setting it

for (var i in o) {
  console.log(i);
}
// logs 'a' and 'd' (in undefined order)

Object.keys(o); // ['a', 'd']

o.propertyIsEnumerable('a'); // true
o.propertyIsEnumerable('b'); // false
o.propertyIsEnumerable('c'); // false
```

### Configurable attribute

The configurable attribute controls whether the property can be deleted from the object and whether its attributes (other than writable) can be changed.

```javascript
var o = {};
Object.defineProperty(o, 'a', {
  get: function() { return 1; },
  configurable: false
});

Object.defineProperty(o, 'a', { configurable: true }); // throws a TypeError
Object.defineProperty(o, 'a', { enumerable: true }); // throws a TypeError
Object.defineProperty(o, 'a', { set: function() {} }); // throws a TypeError (set was undefined previously)
Object.defineProperty(o, 'a', { get: function() { return 1; } }); // throws a TypeError (even though the new get does exactly the same thing)
Object.defineProperty(o, 'a', { value: 12 }); // throws a TypeError

console.log(o.a); // logs 1
delete o.a; // Nothing happens
console.log(o.a); // logs 1
```

Example of adding properties and default values:

```javascript
var o = {};

o.a = 1;
// is equivalent to:
Object.defineProperty(o, 'a', {
  value: 1,
  writable: true,
  configurable: true,
  enumerable: true
});


// On the other hand,
Object.defineProperty(o, 'a', { value: 1 });
// is equivalent to:
Object.defineProperty(o, 'a', {
  value: 1,
  writable: false,
  configurable: false,
  enumerable: false
});
```

Example of custom getters and setters:

```javascript
function Archiver() {
  var temperature = null;
  var archive = [];

  Object.defineProperty(this, 'temperature', {
    get: function() {
      console.log('get!');
      return temperature;
    },
    set: function(value) {
      temperature = value;
      archive.push({ val: temperature });
    }
  });

  this.getArchive = function() { return archive; };
}

var arc = new Archiver();
arc.temperature; // 'get!'
arc.temperature = 11;
arc.temperature = 13;
arc.getArchive(); // [{ val: 11 }, { val: 13 }]
```

### Prototype Inheritance

Implementing inheritance can be done with prototypes:

```javascript
// Constructor with instance properties
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

// Prototype properties are shared across all instances of the class.
// However, they can still be overwritten (example later).
Person.prototype.toString = function () {
    return [this.firstname, this.lastname].join(" ");
}

// Use Person.call to inherit the Person constructor (similar to `super`).
function Employee(firstname, lastname, job) {
    Person.call(this, firstname, lastname);
    this.job = job;
}

// Set Employees's prototype to an instance of Person, so that Employee inherits
// all of Person's properties.
Employee.prototype = new Person(); // required for `toString` to work

var person1 = new Person("John", "Doe");
alert(person1); // > John Doe

var employee1 = new Employee("John", "Doe", "Programmer")
alert(employee1); // > John Doe

// Prototype's properties can also be overwritten
Employee.prototype.toString = function () {
    return this.job;
}

alert(employee1); // > Programmer
alert(person1); // > John Doe
```

Methods can be defined directly on objects:

```javascript
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
    this.toString = function () {
        return [this.firstname, this.lastname].join(" ");
    }
}

var person1 = new Person("John", "Doe");
alert(person1); // John Doe
```

However, here the `toString` function is created separately for each `Person` instance.  A more optimal way is to define the method on the `Person.prototype` like in the first example. The `toString` function is now shared (inherited) with all `Person` instances, which is in turn more memory efficient.

ECMAScript 6 introduces a new set of keywords for implementing classes (syntactic sugar):

```javascript
Class Shape {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
}

class Square extends Shape {
    constructor(sideLength) {
        super(sideLength, sideLength);
    }
    get area() {
        return this.height * this.width;
    }
    set sideLength(newLength) {
        this.height = newLength;
        this.width = newLength;
    }
}

var square = new Square(2);
```
### Prototype inhertiance example and image

Here is a simple object and some functions.

```javascript
function Piste(x,y) {
  this.x = x;
  this.y = y;
}
Piste.prototype.tulo =  function() {return this.x*this.y}

var a = new Piste(3, 6)
var b = new Piste(3.14, 2.8)
```

And here is the visualization on how the prototype chain works.

![alt text](https://github.com/Walther/wiklajs/blob/gh-pages/inheritance.png "Proto chain")


### Encapsulation

In JavaScript, variables have either function or global scope (however, block scope was introduced in ES6 with the keyword `let`). An Immediately-Invoked Function Expression (IIFE) is a pattern which produces a temporary local scope for the variables. The function is executed immediately upon creation. IIFEs can be used to avoid polluting the global environment and to allow access to public methods while encapsulating the function's variables.

```javascript
(function () {
  // Example of an IIFE
}());
```

IIFEs are heavily utilized in the Module pattern (similar to the example seen earlier):

```javascript
var person = (function () {
  var firstName = "John";
  var lastName = "Doe";
  
  return {
    getFullName: function () {
      return [firstName, lastName].join(" ");
    }
  };
}());

console.log(person.firstName); // > undefined
console.log(person.getFullName()); // > John Doe
```

<link rel="stylesheet" type="text/css" href="http://walther.guru/hilightjs_monokai.css">
<script src="http://walther.guru/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
