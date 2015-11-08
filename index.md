# wiklajs

Simple site for submitting answers for the exercises on the course "Ohjelmointitekniikka: JavaScript".

Group members:

- veeti "walther" haapsamo
- pauli "iluaP" niva
- ville "vvainio" vainio
- niko "aozi" novitsky

## Exercises

### Week 1

~~~{.javascript}
// Functions for typechecking

function isNumber(a) {
    return (!isNaN(a) && typeof a === "number") ? true : false;
}

function isInt (a) {
    return (isNumber(a) && a%1===0) ? true : false; // remainder with 1 must be explicitly 0

}

function isString(a) {
    return (typeof a === "string") ? true : false;
}

function isArray(a) {
    return (Array.isArray(a)) ? true : false;
}

function isArrayOfNumbers (a) {
    return (isArray(a) && a.every(isNumber)) ? true : false;
}

function isArrayOfInts (a) {
    return (isArray(a) && a.every(isInt)) ? true : false;
}

function isArrayOfString (a) {
    return (isArray(a) && a.every(isString)) ? true : false;
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
~~~



<link rel="stylesheet" type="text/css" href="http://walther.guru/hilightjs_monokai.css">
<script src="http://walther.guru/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
