# JavaScript Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Variables & Data Types](#variables--data-types)
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Arrays](#arrays)
- [Objects](#objects)
- [ES6+ Features](#es6-features)
- [Destructuring](#destructuring)
- [Spread & Rest](#spread--rest)
- [Async JavaScript](#async-javascript)
- [DOM Manipulation](#dom-manipulation)
- [Events](#events)
- [Error Handling](#error-handling)
- [Classes & OOP](#classes--oop)
- [Modules](#modules)
- [Functional Programming](#functional-programming)
- [Advanced Concepts](#advanced-concepts)
- [Best Practices](#best-practices)

---

## Basics

### Console Methods
```javascript
console.log('Normal message');
console.error('Error message');
console.warn('Warning message');
console.info('Info message');
console.table([{name: 'John', age: 30}]);
console.time('Timer');
console.timeEnd('Timer');
console.clear();
console.group('Group');
console.groupEnd();
```

### Comments
```javascript
// Single line comment

/* 
   Multi-line
   comment
*/

/** 
 * JSDoc comment
 * @param {string} name - The name
 * @returns {string} Greeting message
 */
```

### Strict Mode
```javascript
'use strict';

// Enforces stricter parsing and error handling
```

---

## Variables & Data Types

### Variable Declaration
```javascript
// var (function-scoped, hoisted, can redeclare)
var x = 10;

// let (block-scoped, not hoisted, cannot redeclare)
let y = 20;

// const (block-scoped, cannot reassign)
const z = 30;
const arr = [1, 2, 3]; // can modify array contents
arr.push(4); // OK
// arr = []; // Error!
```

### Data Types

#### Primitive Types
```javascript
// String
let str = 'Hello';
let str2 = "World";
let str3 = `Template ${str}`; // Template literal

// Number
let num = 42;
let float = 3.14;
let negative = -10;
let infinity = Infinity;
let notANumber = NaN;

// BigInt (for very large integers)
let big = 9007199254740991n;
let big2 = BigInt(9007199254740991);

// Boolean
let isTrue = true;
let isFalse = false;

// Undefined
let undef;
let undef2 = undefined;

// Null
let empty = null;

// Symbol (unique identifier)
let sym = Symbol('description');
let sym2 = Symbol('description'); // Different from sym
```

#### Reference Types
```javascript
// Object
let obj = { name: 'John', age: 30 };

// Array
let arr = [1, 2, 3, 4, 5];

// Function
let func = function() { return 'Hello'; };

// Date
let date = new Date();

// RegExp
let regex = /pattern/gi;
```

### Type Checking
```javascript
typeof 'hello'; // 'string'
typeof 42; // 'number'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof null; // 'object' (quirk)
typeof {}; // 'object'
typeof []; // 'object'
typeof function(){}; // 'function'

// Better checks
Array.isArray([]); // true
Number.isNaN(NaN); // true
Number.isFinite(42); // true
```

### Type Conversion
```javascript
// To String
String(123); // '123'
(123).toString(); // '123'
123 + ''; // '123'

// To Number
Number('123'); // 123
parseInt('123'); // 123
parseFloat('123.45'); // 123.45
+'123'; // 123

// To Boolean
Boolean(1); // true
Boolean(0); // false
Boolean(''); // false
Boolean('text'); // true
!!value; // converts to boolean
```

---

## Operators

### Arithmetic
```javascript
let a = 10, b = 3;

a + b;  // 13 (addition)
a - b;  // 7 (subtraction)
a * b;  // 30 (multiplication)
a / b;  // 3.333... (division)
a % b;  // 1 (modulus/remainder)
a ** b; // 1000 (exponentiation)

// Increment/Decrement
a++;    // post-increment
++a;    // pre-increment
a--;    // post-decrement
--a;    // pre-decrement
```

### Assignment
```javascript
let x = 10;

x += 5;  // x = x + 5
x -= 3;  // x = x - 3
x *= 2;  // x = x * 2
x /= 4;  // x = x / 4
x %= 3;  // x = x % 3
x **= 2; // x = x ** 2
```

### Comparison
```javascript
// Equality (type coercion)
5 == '5';   // true
5 != '5';   // false

// Strict equality (no type coercion)
5 === '5';  // false
5 !== '5';  // true

// Comparison
5 > 3;      // true
5 < 3;      // false
5 >= 5;     // true
5 <= 5;     // true
```

### Logical
```javascript
true && false;  // false (AND)
true || false;  // true (OR)
!true;          // false (NOT)

// Short-circuit evaluation
let value = null;
let result = value || 'default'; // 'default'
let result2 = value && 'exists'; // null

// Nullish coalescing
let val = null ?? 'default'; // 'default'
let val2 = 0 ?? 'default';   // 0 (not null/undefined)

// Optional chaining
obj?.property?.nested;
arr?.[0];
func?.();
```

### Ternary
```javascript
condition ? valueIfTrue : valueIfFalse;

let age = 20;
let status = age >= 18 ? 'adult' : 'minor';

// Nested ternary
let result = age < 13 ? 'child' 
           : age < 18 ? 'teen' 
           : 'adult';
```

### Bitwise
```javascript
5 & 3;   // 1 (AND)
5 | 3;   // 7 (OR)
5 ^ 3;   // 6 (XOR)
~5;      // -6 (NOT)
5 << 1;  // 10 (left shift)
5 >> 1;  // 2 (right shift)
5 >>> 1; // 2 (unsigned right shift)
```

---

## Control Flow

### If/Else
```javascript
if (condition) {
    // code
}

if (condition) {
    // code
} else {
    // code
}

if (condition1) {
    // code
} else if (condition2) {
    // code
} else {
    // code
}
```

### Switch
```javascript
switch (value) {
    case 1:
        console.log('One');
        break;
    case 2:
        console.log('Two');
        break;
    case 3:
    case 4:
        console.log('Three or Four');
        break;
    default:
        console.log('Other');
}
```

### Loops

#### For Loop
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// For...of (iterate values)
for (let value of array) {
    console.log(value);
}

// For...in (iterate keys/indices)
for (let key in object) {
    console.log(key, object[key]);
}
```

#### While Loop
```javascript
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}

// Do...while (runs at least once)
let j = 0;
do {
    console.log(j);
    j++;
} while (j < 5);
```

#### Loop Control
```javascript
// Break (exit loop)
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i);
}

// Continue (skip iteration)
for (let i = 0; i < 10; i++) {
    if (i === 5) continue;
    console.log(i);
}

// Labels
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) break outer;
    }
}
```

---

## Functions

### Function Declaration
```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

greet('John'); // 'Hello, John!'
```

### Function Expression
```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

### Arrow Function
```javascript
// Basic
const greet = (name) => {
    return `Hello, ${name}!`;
};

// Concise (implicit return)
const greet = name => `Hello, ${name}!`;

// Multiple parameters
const add = (a, b) => a + b;

// No parameters
const sayHello = () => 'Hello!';

// Returning object (wrap in parentheses)
const makeObj = () => ({ name: 'John' });
```

### Default Parameters
```javascript
function greet(name = 'Guest') {
    return `Hello, ${name}!`;
}

greet();        // 'Hello, Guest!'
greet('John');  // 'Hello, John!'
```

### Rest Parameters
```javascript
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}

sum(1, 2, 3, 4); // 10
```

### Function Methods
```javascript
function greet(greeting) {
    return `${greeting}, ${this.name}!`;
}

const person = { name: 'John' };

// Call
greet.call(person, 'Hello'); // 'Hello, John!'

// Apply (args as array)
greet.apply(person, ['Hi']); // 'Hi, John!'

// Bind (create new function)
const boundGreet = greet.bind(person);
boundGreet('Hey'); // 'Hey, John!'
```

### IIFE (Immediately Invoked Function Expression)
```javascript
(function() {
    console.log('Runs immediately');
})();

// Arrow IIFE
(() => {
    console.log('Runs immediately');
})();
```

### Higher-Order Functions
```javascript
// Function that returns function
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = multiplier(2);
double(5); // 10

// Function that takes function
function applyOperation(a, b, operation) {
    return operation(a, b);
}

applyOperation(5, 3, (x, y) => x + y); // 8
```

---

## Arrays

### Creating Arrays
```javascript
let arr = [1, 2, 3, 4, 5];
let arr2 = new Array(1, 2, 3);
let arr3 = Array.of(1, 2, 3);
let arr4 = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
let arr5 = Array(5).fill(0); // [0, 0, 0, 0, 0]
```

### Array Properties & Methods

#### Basic Methods
```javascript
let arr = [1, 2, 3, 4, 5];

// Length
arr.length; // 5

// Add/Remove
arr.push(6);           // Add to end, returns new length
arr.pop();             // Remove from end, returns removed element
arr.unshift(0);        // Add to start
arr.shift();           // Remove from start

// Splice (add/remove at position)
arr.splice(2, 1);           // Remove 1 element at index 2
arr.splice(2, 0, 'a', 'b'); // Insert at index 2
arr.splice(2, 1, 'x');      // Replace 1 element at index 2

// Slice (extract portion)
arr.slice(1, 3);       // [2, 3] (doesn't modify original)
arr.slice(-2);         // Last 2 elements

// Concat
arr.concat([6, 7]);    // [1, 2, 3, 4, 5, 6, 7]

// Join
arr.join(', ');        // '1, 2, 3, 4, 5'

// Reverse
arr.reverse();         // Modifies original

// Sort
arr.sort();                        // Alphabetical
arr.sort((a, b) => a - b);        // Ascending
arr.sort((a, b) => b - a);        // Descending

// IndexOf/LastIndexOf
arr.indexOf(3);        // 2 (first occurrence)
arr.lastIndexOf(3);    // Last occurrence
arr.includes(3);       // true
```

#### Iteration Methods
```javascript
let arr = [1, 2, 3, 4, 5];

// forEach (iterate)
arr.forEach((item, index, array) => {
    console.log(item, index);
});

// map (transform)
let doubled = arr.map(x => x * 2); // [2, 4, 6, 8, 10]

// filter (select)
let evens = arr.filter(x => x % 2 === 0); // [2, 4]

// find (first match)
let found = arr.find(x => x > 3); // 4

// findIndex
let index = arr.findIndex(x => x > 3); // 3

// some (at least one matches)
arr.some(x => x > 4); // true

// every (all match)
arr.every(x => x > 0); // true

// reduce (accumulate)
let sum = arr.reduce((acc, curr) => acc + curr, 0); // 15

// reduceRight (right to left)
let result = arr.reduceRight((acc, curr) => acc + curr, 0);
```

#### Modern Array Methods
```javascript
// flat (flatten nested arrays)
let nested = [1, [2, [3, [4]]]];
nested.flat();     // [1, 2, [3, [4]]]
nested.flat(2);    // [1, 2, 3, [4]]
nested.flat(Infinity); // [1, 2, 3, 4]

// flatMap (map then flatten)
let arr = [1, 2, 3];
arr.flatMap(x => [x, x * 2]); // [1, 2, 2, 4, 3, 6]

// at (access by index, supports negative)
arr.at(0);   // 1
arr.at(-1);  // Last element

// fill
arr.fill(0, 2, 4); // Fill with 0 from index 2 to 4

// copyWithin
arr.copyWithin(0, 3, 5); // Copy elements from 3-5 to position 0

// entries, keys, values
for (let [index, value] of arr.entries()) {
    console.log(index, value);
}

for (let index of arr.keys()) {
    console.log(index);
}

for (let value of arr.values()) {
    console.log(value);
}
```

### Array Destructuring
```javascript
let [a, b, c] = [1, 2, 3];

// Skip elements
let [first, , third] = [1, 2, 3];

// Rest
let [head, ...tail] = [1, 2, 3, 4];

// Default values
let [x = 0, y = 0] = [1];
```

---

## Objects

### Creating Objects
```javascript
// Object literal
let obj = {
    name: 'John',
    age: 30,
    greet() {
        return `Hello, ${this.name}`;
    }
};

// Constructor function
function Person(name, age) {
    this.name = name;
    this.age = age;
}
let person = new Person('John', 30);

// Object.create
let proto = { greet() { return 'Hello'; } };
let obj2 = Object.create(proto);

// Class
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

### Accessing Properties
```javascript
let obj = { name: 'John', age: 30 };

// Dot notation
obj.name;

// Bracket notation
obj['name'];
let key = 'name';
obj[key];

// Optional chaining
obj?.name?.first;
```

### Object Methods
```javascript
let obj = { a: 1, b: 2, c: 3 };

// Keys, Values, Entries
Object.keys(obj);       // ['a', 'b', 'c']
Object.values(obj);     // [1, 2, 3]
Object.entries(obj);    // [['a', 1], ['b', 2], ['c', 3]]

// Assign (merge objects)
Object.assign({}, obj, { d: 4 }); // { a: 1, b: 2, c: 3, d: 4 }

// Freeze (immutable)
Object.freeze(obj);
obj.a = 10; // No effect

// Seal (can modify, not add/remove)
Object.seal(obj);

// Check
Object.isFrozen(obj);
Object.isSealed(obj);

// Get/Set property descriptors
Object.getOwnPropertyDescriptor(obj, 'a');
Object.defineProperty(obj, 'a', {
    value: 1,
    writable: false,
    enumerable: true,
    configurable: false
});

// Has own property
obj.hasOwnProperty('a');
Object.hasOwn(obj, 'a'); // Newer method
```

### Computed Property Names
```javascript
let key = 'name';
let obj = {
    [key]: 'John',
    ['age']: 30,
    [`${key}Count`]: 1
};
```

### Shorthand Properties
```javascript
let name = 'John';
let age = 30;

// Old way
let obj = { name: name, age: age };

// Shorthand
let obj2 = { name, age };
```

### Object Destructuring
```javascript
let obj = { name: 'John', age: 30, city: 'NYC' };

// Basic
let { name, age } = obj;

// Rename
let { name: fullName, age: years } = obj;

// Default values
let { name, country = 'USA' } = obj;

// Rest
let { name, ...rest } = obj; // rest = { age: 30, city: 'NYC' }

// Nested
let person = {
    name: 'John',
    address: { city: 'NYC', zip: '10001' }
};
let { address: { city } } = person;
```

### Getters & Setters
```javascript
let obj = {
    _value: 0,
    get value() {
        return this._value;
    },
    set value(val) {
        if (val < 0) throw new Error('Negative not allowed');
        this._value = val;
    }
};

obj.value = 10;  // Uses setter
obj.value;       // Uses getter
```

---

## ES6+ Features

### Template Literals
```javascript
let name = 'John';
let age = 30;

// Basic
let message = `Hello, ${name}!`;

// Multi-line
let text = `
    Line 1
    Line 2
    Line 3
`;

// Expressions
let result = `Sum: ${10 + 20}`;

// Tagged templates
function tag(strings, ...values) {
    console.log(strings); // ['Hello ', '!']
    console.log(values);  // ['John']
    return strings[0] + values[0] + strings[1];
}
tag`Hello ${name}!`;
```

### Enhanced Object Literals
```javascript
let name = 'John';
let age = 30;

let obj = {
    // Shorthand property
    name,
    age,
    
    // Computed property
    ['prop_' + name]: true,
    
    // Method shorthand
    greet() {
        return `Hello, ${this.name}`;
    },
    
    // Async method
    async fetchData() {
        return await fetch('/api/data');
    }
};
```

### Symbols
```javascript
// Create unique symbol
let sym1 = Symbol('description');
let sym2 = Symbol('description'); // Different from sym1

// Use as object key
let obj = {
    [sym1]: 'value'
};

// Well-known symbols
Symbol.iterator;
Symbol.toStringTag;
Symbol.hasInstance;

// Global registry
let globalSym = Symbol.for('key');
let same = Symbol.for('key'); // Same as globalSym
```

### Iterators & Generators
```javascript
// Iterator
let iterator = {
    [Symbol.iterator]() {
        let count = 0;
        return {
            next() {
                count++;
                return count <= 3
                    ? { value: count, done: false }
                    : { done: true };
            }
        };
    }
};

for (let val of iterator) {
    console.log(val); // 1, 2, 3
}

// Generator
function* generator() {
    yield 1;
    yield 2;
    yield 3;
}

let gen = generator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }

// Generator with loop
function* range(start, end) {
    for (let i = start; i <= end; i++) {
        yield i;
    }
}

for (let num of range(1, 5)) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

### Sets
```javascript
// Create set
let set = new Set([1, 2, 3, 3, 4]); // {1, 2, 3, 4}

// Methods
set.add(5);
set.has(3);       // true
set.delete(3);
set.clear();
set.size;         // Get size

// Iterate
for (let value of set) {
    console.log(value);
}

set.forEach(value => console.log(value));

// Convert to array
let arr = [...set];
let arr2 = Array.from(set);

// Use cases
let unique = [...new Set(array)]; // Remove duplicates
```

### Maps
```javascript
// Create map
let map = new Map([
    ['key1', 'value1'],
    ['key2', 'value2']
]);

// Methods
map.set('key3', 'value3');
map.get('key1');        // 'value1'
map.has('key1');        // true
map.delete('key1');
map.clear();
map.size;               // Get size

// Any type as key
map.set({}, 'object key');
map.set(function(){}, 'function key');

// Iterate
for (let [key, value] of map) {
    console.log(key, value);
}

map.forEach((value, key) => console.log(key, value));

// Keys, Values, Entries
for (let key of map.keys()) {}
for (let value of map.values()) {}
for (let [key, value] of map.entries()) {}

// Convert
let obj = Object.fromEntries(map);
let map2 = new Map(Object.entries(obj));
```

### WeakSet & WeakMap
```javascript
// WeakSet (only objects, can be garbage collected)
let weakSet = new WeakSet();
let obj = {};
weakSet.add(obj);
weakSet.has(obj);
weakSet.delete(obj);

// WeakMap (only object keys)
let weakMap = new WeakMap();
weakMap.set(obj, 'value');
weakMap.get(obj);
weakMap.has(obj);
weakMap.delete(obj);
```

---

## Destructuring

### Array Destructuring
```javascript
let arr = [1, 2, 3, 4, 5];

// Basic
let [a, b] = arr; // a=1, b=2

// Skip elements
let [first, , third] = arr; // first=1, third=3

// Rest operator
let [head, ...tail] = arr; // head=1, tail=[2,3,4,5]

// Default values
let [x = 0, y = 0, z = 0] = [1, 2]; // x=1, y=2, z=0

// Swapping
let a = 1, b = 2;
[a, b] = [b, a]; // a=2, b=1

// Nested
let nested = [1, [2, 3], 4];
let [a, [b, c], d] = nested;
```

### Object Destructuring
```javascript
let obj = { name: 'John', age: 30, city: 'NYC' };

// Basic
let { name, age } = obj;

// Rename variables
let { name: fullName, age: years } = obj;

// Default values
let { name, country = 'USA' } = obj;

// Rest operator
let { name, ...rest } = obj; // rest = { age: 30, city: 'NYC' }

// Nested
let person = {
    name: 'John',
    address: {
        city: 'NYC',
        zip: '10001'
    }
};
let { address: { city, zip } } = person;

// Function parameters
function greet({ name, age = 0 }) {
    return `${name} is ${age} years old`;
}
greet({ name: 'John', age: 30 });
```

---

## Spread & Rest

### Spread Operator (...)
```javascript
// Arrays
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

let combined = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]
let copy = [...arr1]; // Copy array

// Objects
let obj1 = { a: 1, b: 2 };
let obj2 = { c: 3, d: 4 };

let merged = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }
let clone = { ...obj1 }; // Clone object

// Override properties
let updated = { ...obj1, b: 10 }; // { a: 1, b: 10 }

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
let nums = [1, 2, 3];
sum(...nums); // 6

// Strings
let str = 'hello';
let chars = [...str]; // ['h', 'e', 'l', 'l', 'o']
```

### Rest Parameters
```javascript
// Collect remaining arguments
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}
sum(1, 2, 3, 4, 5); // 15

// With other parameters
function logInfo(first, second, ...others) {
    console.log(first, second, others);
}
logInfo('a', 'b', 'c', 'd', 'e'); // 'a' 'b' ['c', 'd', 'e']

// In destructuring
let [first, ...rest] = [1, 2, 3, 4];
let { name, ...details } = { name: 'John', age: 30, city: 'NYC' };
```

---

## Async JavaScript

### Callbacks
```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback('data');
    }, 1000);
}

fetchData(data => {
    console.log(data);
});
```

### Promises
```javascript
// Create promise
let promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('success');
        // or reject('error');
    }, 1000);
});

// Consume promise
promise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => console.log('Done'));

// Promise.all (wait for all)
Promise.all([promise1, promise2, promise3])
    .then(results => console.log(results));

// Promise.allSettled (wait for all, doesn't reject)
Promise.allSettled([promise1, promise2])
    .then(results => console.log(results));

// Promise.race (first to complete)
Promise.race([promise1, promise2])
    .then(result => console.log(result));

// Promise.any (first to succeed)
Promise.any([promise1, promise2])
    .then(result => console.log(result));

// Static methods
Promise.resolve('value'); // Resolved promise
Promise.reject('error');  // Rejected promise
```

### Async/Await
```javascript
// Async function (returns promise)
async function fetchData() {
    return 'data';
}

// Await (pause until promise resolves)
async function getData() {
    try {
        let data = await fetchData();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

// Multiple awaits
async function fetchAll() {
    let user = await fetchUser();
    let posts = await fetchPosts(user.id);
    return { user, posts };
}

// Parallel execution
async function fetchParallel() {
    let [user, posts] = await Promise.all([
        fetchUser(),
        fetchPosts()
    ]);
    return { user, posts };
}

// Top-level await (in modules)
let data = await fetch('/api/data');
```

### Async Iteration
```javascript
// Async generator
async function* asyncGenerator() {
    yield await Promise.resolve(1);
    yield await Promise.resolve(2);
    yield await Promise.resolve(3);
}

// For await...of
async function process() {
    for await (let value of asyncGenerator()) {
        console.log(value);
    }
}
```

---

## DOM Manipulation

### Selecting Elements
```javascript
// Single element
document.getElementById('id');
document.querySelector('.class');
document.querySelector('#id');

// Multiple elements
document.getElementsByClassName('class'); // HTMLCollection
document.getElementsByTagName('div');     // HTMLCollection
document.querySelectorAll('.class');      // NodeList

// From element
element.querySelector('.child');
element.querySelectorAll('div');

// Special selectors
document.body;
document.head;
document.documentElement; // <html>
```

### Creating & Modifying Elements
```javascript
// Create element
let div = document.createElement('div');
let text = document.createTextNode('Hello');

// Set content
element.textContent = 'Text only';
element.innerHTML = '<strong>HTML</strong>';
element.innerText = 'Text with formatting';

// Attributes
element.getAttribute('id');
element.setAttribute('id', 'my-id');
element.removeAttribute('id');
element.hasAttribute('id');

// Properties
element.id = 'my-id';
element.className = 'class1 class2';
element.classList.add('class3');
element.classList.remove('class1');
element.classList.toggle('active');
element.classList.contains('active');
element.classList.replace('old', 'new');

// Style
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.cssText = 'color: red; background: blue;';

// Data attributes
element.dataset.userId = '123';
element.dataset.userId; // '123'

// Insert into DOM
parent.appendChild(child);
parent.append(child1, child2, 'text'); // Can add multiple
parent.prepend(child);
parent.insertBefore(newNode, referenceNode);
element.insertAdjacentHTML('beforebegin', '<div>Before</div>');
element.insertAdjacentHTML('afterend', '<div>After</div>');
element.insertAdjacentElement('beforebegin', newElement);

// Remove
element.remove();
parent.removeChild(child);

// Replace
parent.replaceChild(newChild, oldChild);
element.replaceWith(newElement);

// Clone
let clone = element.cloneNode(true); // true = deep clone
```

### Traversing DOM
```javascript
// Parent
element.parentElement;
element.parentNode;
element.closest('.ancestor'); // Nearest ancestor matching selector

// Children
element.children;        // HTMLCollection
element.childNodes;      // NodeList (includes text nodes)
element.firstElementChild;
element.lastElementChild;
element.childElementCount;

// Siblings
element.nextElementSibling;
element.previousElementSibling;

// Check
element.matches('.selector'); // Does element match selector?
element.contains(otherElement); // Does element contain other?
```

### Element Properties
```javascript
// Dimensions
element.offsetWidth;     // Width including border
element.offsetHeight;    // Height including border
element.clientWidth;     // Width excluding border
element.clientHeight;    // Height excluding border
element.scrollWidth;     // Full scrollable width
element.scrollHeight;    // Full scrollable height

// Position
element.offsetTop;
element.offsetLeft;
element.getBoundingClientRect(); // { top, right, bottom, left, width, height }

// Scroll position
element.scrollTop;
element.scrollLeft;
window.scrollX; // or window.pageXOffset
window.scrollY; // or window.pageYOffset

// Scroll to
element.scrollTo(0, 100);
element.scrollIntoView();
window.scrollTo({ top: 0, behavior: 'smooth' });
```

---

## Events

### Adding Event Listeners
```javascript
// addEventListener
element.addEventListener('click', function(event) {
    console.log('Clicked!', event);
});

// Arrow function
element.addEventListener('click', (e) => {
    console.log('Clicked!', e);
});

// Options
element.addEventListener('click', handler, {
    once: true,      // Run once then remove
    passive: true,   // Won't call preventDefault
    capture: false   // Bubbling (false) or capturing (true)
});

// Remove listener
function handler(e) {
    console.log('Click');
}
element.addEventListener('click', handler);
element.removeEventListener('click', handler);
```

### Common Events
```javascript
// Mouse events
element.addEventListener('click', handler);
element.addEventListener('dblclick', handler);
element.addEventListener('mousedown', handler);
element.addEventListener('mouseup', handler);
element.addEventListener('mousemove', handler);
element.addEventListener('mouseenter', handler);
element.addEventListener('mouseleave', handler);
element.addEventListener('mouseover', handler);
element.addEventListener('mouseout', handler);
element.addEventListener('contextmenu', handler); // Right click

// Keyboard events
element.addEventListener('keydown', handler);
element.addEventListener('keyup', handler);
element.addEventListener('keypress', handler); // Deprecated

// Form events
form.addEventListener('submit', handler);
input.addEventListener('input', handler);  // Every change
input.addEventListener('change', handler); // On blur
input.addEventListener('focus', handler);
input.addEventListener('blur', handler);

// Document events
document.addEventListener('DOMContentLoaded', handler);
window.addEventListener('load', handler);
window.addEventListener('unload', handler);
window.addEventListener('beforeunload', handler);

// Window events
window.addEventListener('resize', handler);
window.addEventListener('scroll', handler);

// Touch events
element.addEventListener('touchstart', handler);
element.addEventListener('touchmove', handler);
element.addEventListener('touchend', handler);

// Drag events
element.addEventListener('drag', handler);
element.addEventListener('dragstart', handler);
element.addEventListener('dragend', handler);
element.addEventListener('dragover', handler);
element.addEventListener('drop', handler);
```

### Event Object
```javascript
element.addEventListener('click', (event) => {
    // Common properties
    event.type;              // 'click'
    event.target;            // Element that triggered event
    event.currentTarget;     // Element with listener
    event.timeStamp;         // When event occurred
    
    // Mouse events
    event.clientX;           // X coordinate relative to viewport
    event.clientY;           // Y coordinate relative to viewport
    event.pageX;             // X coordinate relative to document
    event.pageY;             // Y coordinate relative to document
    event.button;            // Which mouse button (0=left, 1=middle, 2=right)
    
    // Keyboard events
    event.key;               // Key name ('Enter', 'a', etc.)
    event.code;              // Physical key code ('KeyA', 'Enter')
    event.keyCode;           // Deprecated
    event.shiftKey;          // Is Shift pressed?
    event.ctrlKey;           // Is Ctrl pressed?
    event.altKey;            // Is Alt pressed?
    event.metaKey;           // Is Meta/Cmd pressed?
    
    // Methods
    event.preventDefault();  // Prevent default behavior
    event.stopPropagation(); // Stop event bubbling
    event.stopImmediatePropagation(); // Stop all handlers
});
```

### Event Delegation
```javascript
// Instead of adding listener to each item
document.getElementById('list').addEventListener('click', (e) => {
    if (e.target.matches('.item')) {
        console.log('Item clicked:', e.target);
    }
});
```

### Custom Events
```javascript
// Create custom event
let customEvent = new CustomEvent('myEvent', {
    detail: { data: 'value' },
    bubbles: true,
    cancelable: true
});

// Dispatch event
element.dispatchEvent(customEvent);

// Listen for custom event
element.addEventListener('myEvent', (e) => {
    console.log(e.detail.data);
});
```

---

## Error Handling

### Try/Catch
```javascript
try {
    // Code that might throw error
    throw new Error('Something went wrong');
} catch (error) {
    console.error(error.message);
    console.error(error.stack);
} finally {
    // Always executes
    console.log('Cleanup');
}
```

### Error Types
```javascript
// Built-in errors
throw new Error('Generic error');
throw new TypeError('Type error');
throw new ReferenceError('Reference error');
throw new SyntaxError('Syntax error');
throw new RangeError('Range error');
throw new URIError('URI error');

// Custom error
class CustomError extends Error {
    constructor(message) {
        super(message);
        this.name = 'CustomError';
    }
}

throw new CustomError('Custom error message');
```

### Async Error Handling
```javascript
// Promises
promise
    .then(result => console.log(result))
    .catch(error => console.error(error));

// Async/await
async function fetchData() {
    try {
        let response = await fetch('/api/data');
        let data = await response.json();
        return data;
    } catch (error) {
        console.error('Fetch failed:', error);
        throw error;
    }
}
```

### Global Error Handling
```javascript
// Catch unhandled errors
window.addEventListener('error', (event) => {
    console.error('Global error:', event.error);
});

// Catch unhandled promise rejections
window.addEventListener('unhandledrejection', (event) => {
    console.error('Unhandled rejection:', event.reason);
});
```

---

## Classes & OOP

### Class Syntax
```javascript
class Person {
    // Constructor
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Method
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    // Getter
    get info() {
        return `${this.name} is ${this.age} years old`;
    }
    
    // Setter
    set info(value) {
        let [name, age] = value.split(',');
        this.name = name.trim();
        this.age = parseInt(age);
    }
    
    // Static method (called on class, not instance)
    static species() {
        return 'Homo sapiens';
    }
    
    // Static property
    static planet = 'Earth';
}

// Create instance
let person = new Person('John', 30);
person.greet();

// Call static method
Person.species();
```

### Inheritance
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Call parent constructor
        this.breed = breed;
    }
    
    // Override method
    speak() {
        return `${this.name} barks`;
    }
    
    // Call parent method
    parentSpeak() {
        return super.speak();
    }
}

let dog = new Dog('Buddy', 'Golden Retriever');
dog.speak(); // 'Buddy barks'
```

### Private Fields
```javascript
class BankAccount {
    #balance = 0; // Private field
    
    constructor(initialBalance) {
        this.#balance = initialBalance;
    }
    
    deposit(amount) {
        this.#balance += amount;
    }
    
    getBalance() {
        return this.#balance;
    }
    
    // Private method
    #calculateInterest() {
        return this.#balance * 0.05;
    }
}

let account = new BankAccount(1000);
account.deposit(500);
// account.#balance; // Error! Private field
```

### Static Initialization Blocks
```javascript
class MyClass {
    static {
        // Static initialization code
        console.log('Class initialized');
    }
}
```

### Prototypes
```javascript
// Constructor function (old way)
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// Add method to prototype
Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

// Inheritance with prototypes
function Employee(name, age, job) {
    Person.call(this, name, age);
    this.job = job;
}

Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;

// Check prototype chain
person instanceof Person; // true
Object.getPrototypeOf(person) === Person.prototype; // true
```

---

## Modules

### ES6 Modules

#### Exporting
```javascript
// Named exports
export const PI = 3.14159;
export function add(a, b) {
    return a + b;
}
export class Calculator {}

// Export multiple
const multiply = (a, b) => a * b;
const divide = (a, b) => a / b;
export { multiply, divide };

// Rename on export
export { multiply as mult };

// Default export (one per module)
export default class MyClass {}
export default function() {}
```

#### Importing
```javascript
// Named imports
import { PI, add } from './math.js';
import { multiply, divide } from './math.js';

// Rename on import
import { multiply as mult } from './math.js';

// Import all
import * as math from './math.js';
math.add(1, 2);

// Default import
import MyClass from './myclass.js';

// Mixed
import MyClass, { PI, add } from './module.js';

// Dynamic import
const module = await import('./module.js');

// Import for side effects only
import './styles.css';
```

### CommonJS (Node.js)
```javascript
// Export
module.exports = function() {};
module.exports.add = (a, b) => a + b;
exports.multiply = (a, b) => a * b;

// Import
const module = require('./module');
const { add } = require('./module');
```

---

## Functional Programming

### Pure Functions
```javascript
// Pure (no side effects, same input = same output)
function add(a, b) {
    return a + b;
}

// Impure (side effects)
let total = 0;
function addToTotal(value) {
    total += value; // Modifies external state
    return total;
}
```

### Immutability
```javascript
// Arrays
const arr = [1, 2, 3];
const newArr = [...arr, 4];           // Add
const filtered = arr.filter(x => x !== 2); // Remove
const mapped = arr.map(x => x * 2);   // Transform

// Objects
const obj = { a: 1, b: 2 };
const updated = { ...obj, c: 3 };     // Add
const { b, ...rest } = obj;           // Remove
const modified = { ...obj, a: 10 };   // Update
```

### Higher-Order Functions
```javascript
// Function that returns function
function multiplier(factor) {
    return num => num * factor;
}
const double = multiplier(2);
double(5); // 10

// Function that takes function
function repeat(n, action) {
    for (let i = 0; i < n; i++) {
        action(i);
    }
}
repeat(3, console.log); // 0, 1, 2
```

### Composition
```javascript
// Compose functions (right to left)
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const add1 = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

const compute = compose(square, double, add1);
compute(3); // square(double(add1(3))) = square(8) = 64

// Pipe (left to right)
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
const compute2 = pipe(add1, double, square);
compute2(3); // square(double(add1(3))) = 64
```

### Currying
```javascript
// Regular function
function add(a, b, c) {
    return a + b + c;
}

// Curried function
function curriedAdd(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}
curriedAdd(1)(2)(3); // 6

// Arrow function currying
const curriedAdd2 = a => b => c => a + b + c;

// Partial application
const add5 = curriedAdd(5);
const add5and10 = add5(10);
add5and10(3); // 18
```

### Memoization
```javascript
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) {
            return cache.get(key);
        }
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

// Expensive function
const factorial = memoize(function(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
});
```

---

## Advanced Concepts

### Closures
```javascript
function outer() {
    let count = 0;
    
    return function inner() {
        count++;
        return count;
    };
}

const counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3

// Private variables
function createCounter() {
    let count = 0;
    return {
        increment() { count++; },
        decrement() { count--; },
        getCount() { return count; }
    };
}
```

### Hoisting
```javascript
// Variables declared with var are hoisted
console.log(x); // undefined
var x = 10;

// Functions are hoisted
greet(); // Works!
function greet() {
    console.log('Hello');
}

// let/const are not hoisted (temporal dead zone)
// console.log(y); // ReferenceError
let y = 20;
```

### This Keyword
```javascript
// Global context
console.log(this); // window (browser) or global (Node.js)

// Object method
const obj = {
    name: 'John',
    greet() {
        console.log(this.name); // 'John'
    }
};

// Arrow functions (inherit this from parent)
const obj2 = {
    name: 'John',
    greet: () => {
        console.log(this.name); // undefined (this is window)
    }
};

// Class methods
class Person {
    constructor(name) {
        this.name = name;
    }
    greet() {
        console.log(this.name);
    }
}

// Explicit binding
function greet() {
    console.log(this.name);
}
const person = { name: 'John' };
greet.call(person);  // 'John'
greet.apply(person); // 'John'
const boundGreet = greet.bind(person);
boundGreet();        // 'John'
```

### Call Stack & Event Loop
```javascript
// Synchronous (blocking)
console.log('1');
console.log('2');
console.log('3');

// Asynchronous (non-blocking)
console.log('1');
setTimeout(() => console.log('2'), 0);
console.log('3');
// Output: 1, 3, 2

// Microtasks (promises) execute before macrotasks (setTimeout)
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Output: 1, 4, 3, 2
```

### Regular Expressions
```javascript
// Create regex
let regex = /pattern/flags;
let regex2 = new RegExp('pattern', 'flags');

// Flags
// g - global (find all matches)
// i - case insensitive
// m - multiline
// s - dotAll (. matches newlines)
// u - unicode
// y - sticky

// Methods
regex.test(string);        // true/false
regex.exec(string);        // match array or null
string.match(regex);       // matches array
string.search(regex);      // index of match
string.replace(regex, replacement);
string.split(regex);

// Common patterns
/\d+/;           // One or more digits
/\w+/;           // One or more word characters
/\s+/;           // One or more whitespace
/[a-z]/i;        // Any letter (case insensitive)
/^start/;        // Starts with "start"
/end$/;          // Ends with "end"
/(cat|dog)/;     // "cat" or "dog"
/\d{3}-\d{3}-\d{4}/; // Phone number pattern

// Groups
let match = /(\d{3})-(\d{3})-(\d{4})/.exec('555-123-4567');
// match[0] = '555-123-4567' (full match)
// match[1] = '555' (first group)
// match[2] = '123' (second group)
// match[3] = '4567' (third group)

// Named groups
let regex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
let match = regex.exec('2024-01-15');
match.groups.year;  // '2024'
match.groups.month; // '01'
match.groups.day;   // '15'
```

### Proxy & Reflect
```javascript
// Proxy (intercept operations)
let handler = {
    get(target, prop) {
        console.log(`Getting ${prop}`);
        return target[prop];
    },
    set(target, prop, value) {
        console.log(`Setting ${prop} to ${value}`);
        target[prop] = value;
        return true;
    }
};

let obj = { name: 'John' };
let proxy = new Proxy(obj, handler);
proxy.name; // Logs: Getting name
proxy.age = 30; // Logs: Setting age to 30

// Reflect (perform default operations)
Reflect.get(obj, 'name');
Reflect.set(obj, 'age', 30);
Reflect.has(obj, 'name');
Reflect.deleteProperty(obj, 'age');
```

### Web APIs

#### LocalStorage/SessionStorage
```javascript
// Store data
localStorage.setItem('key', 'value');
sessionStorage.setItem('key', 'value');

// Retrieve data
localStorage.getItem('key');

// Remove data
localStorage.removeItem('key');
localStorage.clear();

// Store objects (must stringify)
localStorage.setItem('user', JSON.stringify({ name: 'John' }));
let user = JSON.parse(localStorage.getItem('user'));
```

#### Fetch API
```javascript
// Basic fetch
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

// With options
fetch('https://api.example.com/data', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token'
    },
    body: JSON.stringify({ data: 'value' })
})
    .then(response => response.json())
    .then(data => console.log(data));

// Async/await
async function fetchData() {
    try {
        let response = await fetch('https://api.example.com/data');
        let data = await response.json();
        return data;
    } catch (error) {
        console.error('Fetch error:', error);
    }
}
```

#### setTimeout/setInterval
```javascript
// setTimeout (run once after delay)
let timeoutId = setTimeout(() => {
    console.log('Executed after 1 second');
}, 1000);

// Cancel timeout
clearTimeout(timeoutId);

// setInterval (run repeatedly)
let intervalId = setInterval(() => {
    console.log('Executed every second');
}, 1000);

// Cancel interval
clearInterval(intervalId);
```

---

## Best Practices

### Code Organization
```javascript
// Use meaningful variable names
// Bad
let x = 'John';
let fn = () => {};

// Good
let userName = 'John';
let calculateTotal = () => {};

// Use const by default, let only when reassigning
const MAX_USERS = 100;
let currentCount = 0;

// Group related code
const userConfig = {
    name: 'John',
    age: 30,
    email: 'john@example.com'
};
```

### Function Best Practices
```javascript
// Keep functions small and focused
// Bad
function processUser(user) {
    // Validate
    // Transform
    // Save to database
    // Send email
    // Log
}

// Good
function validateUser(user) {}
function transformUser(user) {}
function saveUser(user) {}
function sendWelcomeEmail(user) {}

// Use descriptive function names
// Bad
function doStuff() {}

// Good
function calculateUserAge() {}

// Avoid side effects in pure functions
// Bad
let total = 0;
function add(value) {
    total += value; // Side effect!
}

// Good
function add(a, b) {
    return a + b; // Pure function
}
```

### Error Handling
```javascript
// Always handle errors
async function fetchUser() {
    try {
        let response = await fetch('/api/user');
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        console.error('Failed to fetch user:', error);
        throw error;
    }
}

// Validate inputs
function divide(a, b) {
    if (b === 0) {
        throw new Error('Cannot divide by zero');
    }
    return a / b;
}
```

### Performance
```javascript
// Avoid unnecessary operations in loops
// Bad
for (let i = 0; i < array.length; i++) {
    // length calculated every iteration
}

// Good
const len = array.length;
for (let i = 0; i < len; i++) {
    // length calculated once
}

// Use appropriate array methods
// Use map for transformation
// Use filter for selection
// Use reduce for accumulation
// Use forEach only for side effects

// Debounce expensive operations
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

const expensiveOperation = debounce(() => {
    // Expensive code
}, 300);
```

### Modern JavaScript
```javascript
// Use template literals
const message = `Hello, ${name}!`;

// Use destructuring
const { name, age } = user;
const [first, second] = array;

// Use spread operator
const newArray = [...oldArray, newItem];
const newObject = { ...oldObject, newProp: value };

// Use arrow functions (when appropriate)
const add = (a, b) => a + b;

// Use optional chaining
const city = user?.address?.city;

// Use nullish coalescing
const name = user.name ?? 'Guest';

// Use async/await instead of promises chains
// Good
async function getData() {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    return { user, posts };
}
```

---

**Pro Tip:** Master async/await for clean asynchronous code, embrace functional programming principles, and always write testable, maintainable code!