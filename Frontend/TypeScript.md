# TypeScript Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Basic Types](#basic-types)
- [Functions](#functions)
- [Interfaces](#interfaces)
- [Type Aliases](#type-aliases)
- [Union & Intersection Types](#union--intersection-types)
- [Literal Types](#literal-types)
- [Enums](#enums)
- [Arrays & Tuples](#arrays--tuples)
- [Objects](#objects)
- [Classes](#classes)
- [Generics](#generics)
- [Type Guards & Narrowing](#type-guards--narrowing)
- [Utility Types](#utility-types)
- [Advanced Types](#advanced-types)
- [Decorators](#decorators)
- [Modules](#modules)
- [Configuration](#configuration)
- [Best Practices](#best-practices)

---

## Basics

### Setup
```bash
# Install TypeScript
npm install -g typescript

# Initialize tsconfig.json
tsc --init

# Compile TypeScript
tsc file.ts

# Watch mode
tsc --watch

# Compile all files
tsc
```

### Basic Syntax
```typescript
// Variable with type annotation
let name: string = 'John';
let age: number = 30;
let isActive: boolean = true;

// Type inference (TypeScript infers the type)
let message = 'Hello'; // inferred as string

// Multiple variables
let x: number, y: number, z: number;
```

---

## Basic Types

### Primitive Types
```typescript
// String
let str: string = 'Hello';
let template: string = `Value: ${str}`;

// Number
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;

// Boolean
let isDone: boolean = false;

// Null and Undefined
let n: null = null;
let u: undefined = undefined;

// Void (no return value)
function log(): void {
    console.log('Message');
}

// Never (function never returns)
function error(message: string): never {
    throw new Error(message);
}

function infiniteLoop(): never {
    while (true) {}
}

// Any (opt-out of type checking)
let notSure: any = 4;
notSure = 'maybe a string';
notSure = false;

// Unknown (type-safe any)
let value: unknown;
value = 'string';
value = 123;
// let str: string = value; // Error!
if (typeof value === 'string') {
    let str: string = value; // OK
}

// BigInt
let big: bigint = 100n;

// Symbol
let sym: symbol = Symbol('key');
```

---

## Functions

### Function Types
```typescript
// Named function
function add(x: number, y: number): number {
    return x + y;
}

// Anonymous function
let subtract = function(x: number, y: number): number {
    return x - y;
};

// Arrow function
let multiply = (x: number, y: number): number => x * y;

// Function type
let divide: (x: number, y: number) => number;
divide = (x, y) => x / y;
```

### Optional & Default Parameters
```typescript
// Optional parameter
function greet(name: string, greeting?: string): string {
    return greeting ? `${greeting}, ${name}` : `Hello, ${name}`;
}

// Default parameter
function greet2(name: string, greeting: string = 'Hello'): string {
    return `${greeting}, ${name}`;
}

// Rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((acc, num) => acc + num, 0);
}
```

### Function Overloads
```typescript
// Overload signatures
function makeDate(timestamp: number): Date;
function makeDate(year: number, month: number, day: number): Date;

// Implementation signature
function makeDate(yearOrTimestamp: number, month?: number, day?: number): Date {
    if (month !== undefined && day !== undefined) {
        return new Date(yearOrTimestamp, month, day);
    }
    return new Date(yearOrTimestamp);
}
```

### This Parameter
```typescript
interface User {
    name: string;
    age: number;
    greet(this: User): void;
}

const user: User = {
    name: 'John',
    age: 30,
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};
```

---

## Interfaces

### Basic Interface
```typescript
interface Person {
    name: string;
    age: number;
}

let person: Person = {
    name: 'John',
    age: 30
};
```

### Optional Properties
```typescript
interface Config {
    host: string;
    port?: number; // Optional
    timeout?: number;
}

let config: Config = {
    host: 'localhost'
    // port and timeout are optional
};
```

### Readonly Properties
```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}

let point: Point = { x: 10, y: 20 };
// point.x = 5; // Error! Cannot assign to readonly property
```

### Function Types in Interfaces
```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = (src, sub) => {
    return src.includes(sub);
};
```

### Indexable Types
```typescript
// String index
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray = ['Bob', 'Alice'];

// Dictionary
interface Dictionary {
    [key: string]: number;
}

let dict: Dictionary = {
    age: 30,
    count: 5
};
```

### Class Implements Interface
```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    
    setTime(d: Date) {
        this.currentTime = d;
    }
}
```

### Extending Interfaces
```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square: Square = {
    color: 'blue',
    sideLength: 10
};

// Multiple inheritance
interface PenStroke {
    penWidth: number;
}

interface FilledSquare extends Square, PenStroke {
    filled: boolean;
}
```

---

## Type Aliases

### Basic Type Alias
```typescript
type Name = string;
type Age = number;
type User = {
    name: Name;
    age: Age;
};

let user: User = {
    name: 'John',
    age: 30
};
```

### Union Types in Aliases
```typescript
type ID = number | string;

let userId: ID = 123;
userId = 'abc123';
```

### Function Type Alias
```typescript
type Operation = (x: number, y: number) => number;

const add: Operation = (x, y) => x + y;
const subtract: Operation = (x, y) => x - y;
```

### Type vs Interface
```typescript
// Interface
interface PersonInterface {
    name: string;
    age: number;
}

// Type alias
type PersonType = {
    name: string;
    age: number;
};

// Key differences:
// 1. Interface can be extended, type can use intersection
// 2. Interface can be merged (declaration merging)
// 3. Type can represent primitives, unions, tuples
```

---

## Union & Intersection Types

### Union Types
```typescript
// Can be one of several types
type StringOrNumber = string | number;

let value: StringOrNumber;
value = 'hello';
value = 123;

// Union in function
function format(value: string | number): string {
    if (typeof value === 'string') {
        return value.toUpperCase();
    }
    return value.toFixed(2);
}

// Union of object types
type Success = { success: true; data: any };
type Failure = { success: false; error: string };
type Result = Success | Failure;
```

### Intersection Types
```typescript
// Combines multiple types
type Person = {
    name: string;
    age: number;
};

type Employee = {
    employeeId: number;
    department: string;
};

type EmployeePerson = Person & Employee;

let employee: EmployeePerson = {
    name: 'John',
    age: 30,
    employeeId: 12345,
    department: 'IT'
};
```

### Discriminated Unions
```typescript
interface Circle {
    kind: 'circle';
    radius: number;
}

interface Square {
    kind: 'square';
    sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case 'circle':
            return Math.PI * shape.radius ** 2;
        case 'square':
            return shape.sideLength ** 2;
    }
}
```

---

## Literal Types

### String Literal Types
```typescript
type Direction = 'north' | 'south' | 'east' | 'west';

let direction: Direction;
direction = 'north'; // OK
// direction = 'up'; // Error!

// Function with literal types
function move(direction: Direction, distance: number): void {
    console.log(`Moving ${direction} for ${distance} units`);
}
```

### Numeric Literal Types
```typescript
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;

function rollDice(): DiceRoll {
    return (Math.floor(Math.random() * 6) + 1) as DiceRoll;
}
```

### Boolean Literal Types
```typescript
type Success = true;
type Failure = false;

function isSuccess(result: Success | Failure): void {
    if (result === true) {
        console.log('Success!');
    }
}
```

---

## Enums

### Numeric Enums
```typescript
enum Direction {
    Up,    // 0
    Down,  // 1
    Left,  // 2
    Right  // 3
}

let dir: Direction = Direction.Up;

// Custom starting value
enum Status {
    Active = 1,
    Inactive,  // 2
    Pending    // 3
}

// Custom values
enum HttpStatus {
    OK = 200,
    BadRequest = 400,
    Unauthorized = 401,
    NotFound = 404
}
```

### String Enums
```typescript
enum Direction {
    Up = 'UP',
    Down = 'DOWN',
    Left = 'LEFT',
    Right = 'RIGHT'
}

let dir: Direction = Direction.Up; // 'UP'
```

### Const Enums
```typescript
// Inlined at compile time (better performance)
const enum Colors {
    Red = 'RED',
    Green = 'GREEN',
    Blue = 'BLUE'
}

let color = Colors.Red; // Compiled to: let color = "RED"
```

### Enum as Type
```typescript
enum UserRole {
    Admin = 'ADMIN',
    User = 'USER',
    Guest = 'GUEST'
}

function checkPermission(role: UserRole): boolean {
    return role === UserRole.Admin;
}
```

---

## Arrays & Tuples

### Arrays
```typescript
// Array of numbers
let numbers: number[] = [1, 2, 3, 4, 5];

// Generic array type
let strings: Array<string> = ['a', 'b', 'c'];

// Array of objects
interface User {
    name: string;
    age: number;
}
let users: User[] = [
    { name: 'John', age: 30 },
    { name: 'Jane', age: 25 }
];

// Readonly array
let readonlyArray: ReadonlyArray<number> = [1, 2, 3];
// readonlyArray.push(4); // Error!
```

### Tuples
```typescript
// Fixed length array with known types
let tuple: [string, number] = ['John', 30];

// Access elements
let name = tuple[0]; // string
let age = tuple[1];  // number

// Optional elements
let optionalTuple: [string, number?] = ['John'];

// Rest elements
let restTuple: [string, ...number[]] = ['John', 1, 2, 3];

// Named tuples
let namedTuple: [name: string, age: number] = ['John', 30];

// Readonly tuples
let readonlyTuple: readonly [string, number] = ['John', 30];
```

---

## Objects

### Object Types
```typescript
// Inline type
let person: { name: string; age: number } = {
    name: 'John',
    age: 30
};

// Optional properties
let config: { host: string; port?: number } = {
    host: 'localhost'
};

// Index signatures
let scores: { [key: string]: number } = {
    math: 90,
    english: 85
};
```

### Readonly Properties
```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}

let point: Point = { x: 10, y: 20 };
// point.x = 5; // Error!
```

### Extending Object Types
```typescript
interface BasicAddress {
    street: string;
    city: string;
}

interface AddressWithCountry extends BasicAddress {
    country: string;
}

let address: AddressWithCountry = {
    street: '123 Main St',
    city: 'New York',
    country: 'USA'
};
```

---

## Classes

### Basic Class
```typescript
class Person {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    greet(): string {
        return `Hello, I'm ${this.name}`;
    }
}

let person = new Person('John', 30);
```

### Access Modifiers
```typescript
class BankAccount {
    public accountNumber: string;      // Accessible everywhere
    protected balance: number;         // Accessible in class and subclasses
    private pin: number;               // Accessible only in class
    
    constructor(accountNumber: string, balance: number, pin: number) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.pin = pin;
    }
    
    public deposit(amount: number): void {
        this.balance += amount;
    }
    
    private validatePin(pin: number): boolean {
        return this.pin === pin;
    }
}
```

### Parameter Properties
```typescript
// Shorthand for declaring and initializing properties
class Person {
    constructor(
        public name: string,
        private age: number,
        protected email: string
    ) {}
    
    greet(): string {
        return `Hello, I'm ${this.name}`;
    }
}
```

### Readonly Properties
```typescript
class Person {
    readonly id: number;
    
    constructor(id: number) {
        this.id = id;
    }
}

let person = new Person(1);
// person.id = 2; // Error! Cannot assign to readonly property
```

### Getters & Setters
```typescript
class Employee {
    private _fullName: string = '';
    
    get fullName(): string {
        return this._fullName;
    }
    
    set fullName(name: string) {
        if (name.length > 0) {
            this._fullName = name;
        }
    }
}

let employee = new Employee();
employee.fullName = 'John Doe';
console.log(employee.fullName);
```

### Static Members
```typescript
class MathUtil {
    static PI: number = 3.14159;
    
    static calculateCircumference(radius: number): number {
        return 2 * MathUtil.PI * radius;
    }
}

console.log(MathUtil.PI);
console.log(MathUtil.calculateCircumference(10));
```

### Abstract Classes
```typescript
abstract class Animal {
    abstract makeSound(): void; // Must be implemented by subclasses
    
    move(): void {
        console.log('Moving...');
    }
}

class Dog extends Animal {
    makeSound(): void {
        console.log('Woof!');
    }
}

// let animal = new Animal(); // Error! Cannot create instance of abstract class
let dog = new Dog();
dog.makeSound();
```

### Inheritance
```typescript
class Animal {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    move(distance: number = 0): void {
        console.log(`${this.name} moved ${distance}m`);
    }
}

class Dog extends Animal {
    bark(): void {
        console.log('Woof! Woof!');
    }
}

class Snake extends Animal {
    constructor(name: string) {
        super(name); // Call parent constructor
    }
    
    move(distance: number = 5): void {
        console.log('Slithering...');
        super.move(distance); // Call parent method
    }
}
```

---

## Generics

### Generic Functions
```typescript
// Generic function
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>('hello'); // Explicit type
let output2 = identity(123); // Type inference

// Generic with array
function getFirst<T>(arr: T[]): T {
    return arr[0];
}

let first = getFirst([1, 2, 3]); // number
let firstStr = getFirst(['a', 'b']); // string
```

### Generic Interfaces
```typescript
interface GenericIdentity<T> {
    (arg: T): T;
}

let myIdentity: GenericIdentity<number> = (arg) => arg;

// Generic interface with property
interface Container<T> {
    value: T;
    getValue(): T;
}

let numberContainer: Container<number> = {
    value: 123,
    getValue() {
        return this.value;
    }
};
```

### Generic Classes
```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
    
    constructor(zeroValue: T, addFn: (x: T, y: T) => T) {
        this.zeroValue = zeroValue;
        this.add = addFn;
    }
}

let myGenericNumber = new GenericNumber<number>(0, (x, y) => x + y);
myGenericNumber.add(5, 10); // 15

let stringNumeric = new GenericNumber<string>('', (x, y) => x + y);
stringNumeric.add('Hello', ' World'); // 'Hello World'
```

### Generic Constraints
```typescript
// Constrain to types with length property
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

logLength('hello'); // OK
logLength([1, 2, 3]); // OK
// logLength(123); // Error! number doesn't have length

// Using type parameters in constraints
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

let person = { name: 'John', age: 30 };
getProperty(person, 'name'); // OK
// getProperty(person, 'invalid'); // Error!
```

### Default Generic Types
```typescript
interface Container<T = string> {
    value: T;
}

let stringContainer: Container = { value: 'hello' }; // T defaults to string
let numberContainer: Container<number> = { value: 123 };
```

---

## Type Guards & Narrowing

### typeof Type Guards
```typescript
function process(value: string | number): string {
    if (typeof value === 'string') {
        return value.toUpperCase(); // TypeScript knows it's string
    }
    return value.toFixed(2); // TypeScript knows it's number
}
```

### instanceof Type Guards
```typescript
class Dog {
    bark() { console.log('Woof!'); }
}

class Cat {
    meow() { console.log('Meow!'); }
}

function makeSound(animal: Dog | Cat): void {
    if (animal instanceof Dog) {
        animal.bark();
    } else {
        animal.meow();
    }
}
```

### in Type Guards
```typescript
interface Fish {
    swim(): void;
}

interface Bird {
    fly(): void;
}

function move(animal: Fish | Bird): void {
    if ('swim' in animal) {
        animal.swim();
    } else {
        animal.fly();
    }
}
```

### Custom Type Guards
```typescript
interface Fish {
    swim(): void;
}

interface Bird {
    fly(): void;
}

// Type predicate
function isFish(animal: Fish | Bird): animal is Fish {
    return (animal as Fish).swim !== undefined;
}

function move(animal: Fish | Bird): void {
    if (isFish(animal)) {
        animal.swim(); // TypeScript knows it's Fish
    } else {
        animal.fly(); // TypeScript knows it's Bird
    }
}
```

### Nullish Narrowing
```typescript
function process(value: string | null | undefined): string {
    // Null/undefined check
    if (value === null || value === undefined) {
        return 'No value';
    }
    return value.toUpperCase(); // TypeScript knows it's string
}

// Truthiness narrowing
function getLength(value: string | null): number {
    if (value) {
        return value.length; // TypeScript knows it's string
    }
    return 0;
}
```

### Discriminated Unions
```typescript
interface Success {
    status: 'success';
    data: any;
}

interface Error {
    status: 'error';
    message: string;
}

type Response = Success | Error;

function handle(response: Response): void {
    if (response.status === 'success') {
        console.log(response.data); // TypeScript knows it's Success
    } else {
        console.log(response.message); // TypeScript knows it's Error
    }
}
```

---

## Utility Types

### Partial<T>
```typescript
// Makes all properties optional
interface User {
    name: string;
    age: number;
    email: string;
}

type PartialUser = Partial<User>;
// { name?: string; age?: number; email?: string; }

function updateUser(user: User, updates: Partial<User>): User {
    return { ...user, ...updates };
}
```

### Required<T>
```typescript
// Makes all properties required
interface Config {
    host?: string;
    port?: number;
}

type RequiredConfig = Required<Config>;
// { host: string; port: number; }
```

### Readonly<T>
```typescript
// Makes all properties readonly
interface User {
    name: string;
    age: number;
}

type ReadonlyUser = Readonly<User>;
// { readonly name: string; readonly age: number; }

let user: ReadonlyUser = { name: 'John', age: 30 };
// user.name = 'Jane'; // Error!
```

### Record<K, T>
```typescript
// Creates object type with keys K and values T
type PageInfo = Record<'home' | 'about' | 'contact', { title: string }>;

let pages: PageInfo = {
    home: { title: 'Home' },
    about: { title: 'About' },
    contact: { title: 'Contact' }
};
```

### Pick<T, K>
```typescript
// Pick specific properties from type
interface User {
    name: string;
    age: number;
    email: string;
    password: string;
}

type UserPreview = Pick<User, 'name' | 'email'>;
// { name: string; email: string; }
```

### Omit<T, K>
```typescript
// Remove specific properties from type
interface User {
    name: string;
    age: number;
    email: string;
    password: string;
}

type UserWithoutPassword = Omit<User, 'password'>;
// { name: string; age: number; email: string; }
```

### Exclude<T, U>
```typescript
// Exclude types from union
type T1 = Exclude<'a' | 'b' | 'c', 'a' | 'b'>;
// 'c'

type T2 = Exclude<string | number | boolean, string>;
// number | boolean
```

### Extract<T, U>
```typescript
// Extract types from union
type T1 = Extract<'a' | 'b' | 'c', 'a' | 'f'>;
// 'a'

type T2 = Extract<string | number | boolean, string | boolean>;
// string | boolean
```

### NonNullable<T>
```typescript
// Remove null and undefined
type T1 = NonNullable<string | number | null | undefined>;
// string | number
```

### ReturnType<T>
```typescript
// Get return type of function
function getUser() {
    return { name: 'John', age: 30 };
}

type User = ReturnType<typeof getUser>;
// { name: string; age: number; }
```

### Parameters<T>
```typescript
// Get parameters of function as tuple
function greet(name: string, age: number): string {
    return `Hello ${name}, age ${age}`;
}

type GreetParams = Parameters<typeof greet>;
// [name: string, age: number]
```

### Awaited<T>
```typescript
// Unwrap Promise type
type A = Awaited<Promise<string>>;
// string

type B = Awaited<Promise<Promise<number>>>;
// number
```

---

## Advanced Types

### Conditional Types
```typescript
// T extends U ? X : Y
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false

// Distributive conditional types
type ToArray<T> = T extends any ? T[] : never;
type StrOrNumArray = ToArray<string | number>;
// string[] | number[]

// Infer keyword
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;

type GetElementType<T> = T extends (infer U)[] ? U : T;
type NumType = GetElementType<number[]>; // number
```

### Mapped Types
```typescript
// Create new types by mapping over properties
type Nullable<T> = {
    [P in keyof T]: T[P] | null;
};

interface User {
    name: string;
    age: number;
}

type NullableUser = Nullable<User>;
// { name: string | null; age: number | null; }

// With modifiers
type ReadonlyNullable<T> = {
    readonly [P in keyof T]: T[P] | null;
};

// Remove modifiers
type Mutable<T> = {
    -readonly [P in keyof T]: T[P];
};

type Optional<T> = {
    [P in keyof T]?: T[P];
};
```

### Template Literal Types
```typescript
// String manipulation types
type Greeting = 'Hello' | 'Hi';
type Name = 'World' | 'TypeScript';

type GreetingMessage = `${Greeting} ${Name}`;
// 'Hello World' | 'Hello TypeScript' | 'Hi World' | 'Hi TypeScript'

// Uppercase, Lowercase, Capitalize, Uncapitalize
type UpperGreeting = Uppercase<'hello'>; // 'HELLO'
type LowerGreeting = Lowercase<'HELLO'>; // 'hello'
type CapitalGreeting = Capitalize<'hello'>; // 'Hello'
type UncapitalGreeting = Uncapitalize<'Hello'>; // 'hello'

// Event names
type PropEventSource<T> = {
    on<K extends string & keyof T>(
        eventName: `${K}Changed`,
        callback: (newValue: T[K]) => void
    ): void;
};

interface Person {
    name: string;
    age: number;
}

let person: PropEventSource<Person>;
person.on('nameChanged', (newName) => {
    // newName is string
});
```

### Index Signature Types
```typescript
// String index
interface StringMap {
    [key: string]: string;
}

// Number index
interface NumberArray {
    [index: number]: number;
}

// Mixed (number must be subtype of string)
interface MixedMap {
    [key: string]: any;
    [index: number]: string; // OK, string is subtype of any
}
```

### Intersection Types with Conflicts
```typescript
interface A {
    value: string;
}

interface B {
    value: number;
}

type C = A & B;
// { value: string & number } = { value: never }

// Handling conflicts
type Merge<A, B> = {
    [K in keyof A | keyof B]: K extends keyof B
        ? B[K]
        : K extends keyof A
        ? A[K]
        : never;
};
```

---

## Decorators

### Enable Decorators
```json
// tsconfig.json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

### Class Decorators
```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class BugReport {
    type = 'report';
    title: string;
    
    constructor(t: string) {
        this.title = t;
    }
}

// Decorator factory
function component(config: any) {
    return function(constructor: Function) {
        // Do something with config and constructor
    };
}

@component({ selector: 'my-component' })
class MyComponent {}
```

### Method Decorators
```typescript
function enumerable(value: boolean) {
    return function(
        target: any,
        propertyKey: string,
        descriptor: PropertyDescriptor
    ) {
        descriptor.enumerable = value;
    };
}

class Greeter {
    greeting: string;
    
    constructor(message: string) {
        this.greeting = message;
    }
    
    @enumerable(false)
    greet() {
        return `Hello, ${this.greeting}`;
    }
}
```

### Accessor Decorators
```typescript
function configurable(value: boolean) {
    return function(
        target: any,
        propertyKey: string,
        descriptor: PropertyDescriptor
    ) {
        descriptor.configurable = value;
    };
}

class Point {
    private _x: number;
    
    constructor(x: number) {
        this._x = x;
    }
    
    @configurable(false)
    get x() {
        return this._x;
    }
}
```

### Property Decorators
```typescript
function format(formatString: string) {
    return function(target: any, propertyKey: string) {
        let value: string;
        
        const getter = function() {
            return value;
        };
        
        const setter = function(newVal: string) {
            value = formatString.replace('%s', newVal);
        };
        
        Object.defineProperty(target, propertyKey, {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true
        });
    };
}

class User {
    @format('Hello, %s')
    name: string;
}
```

### Parameter Decorators
```typescript
function required(
    target: any,
    propertyKey: string,
    parameterIndex: number
) {
    console.log(`Parameter ${parameterIndex} of ${propertyKey} is required`);
}

class Greeter {
    greet(@required name: string) {
        return `Hello, ${name}`;
    }
}
```

---

## Modules

### Export
```typescript
// Named export
export const PI = 3.14159;
export function add(a: number, b: number): number {
    return a + b;
}
export class Calculator {}

// Export existing
const multiply = (a: number, b: number) => a * b;
export { multiply };

// Rename on export
export { multiply as mult };

// Default export
export default class DefaultClass {}

// Re-export
export { add, multiply } from './math';
export * from './utils';
```

### Import
```typescript
// Named import
import { PI, add } from './math';

// Rename on import
import { add as sum } from './math';

// Import all
import * as math from './math';

// Default import
import Calculator from './calculator';

// Mixed
import Calculator, { PI, add } from './math';

// Type-only imports
import type { User } from './types';
import { type User, getName } from './utils';
```

### Namespaces
```typescript
namespace Validation {
    export interface StringValidator {
        isValid(s: string): boolean;
    }
    
    export class EmailValidator implements StringValidator {
        isValid(s: string): boolean {
            return s.includes('@');
        }
    }
}

let validator = new Validation.EmailValidator();
```

### Module Augmentation
```typescript
// Extend existing module
declare module './original' {
    export interface Config {
        newProperty: string;
    }
}
```

### Global Augmentation
```typescript
// Add to global scope
declare global {
    interface Window {
        myCustomProperty: string;
    }
}

window.myCustomProperty = 'value';
```

---

## Configuration

### tsconfig.json
```json
{
  "compilerOptions": {
    // Type Checking
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    
    // Modules
    "module": "commonjs",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "resolveJsonModule": true,
    
    // Emit
    "target": "es2020",
    "lib": ["es2020", "dom"],
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "removeComments": true,
    
    // Interop Constraints
    "isolatedModules": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    
    // Language and Environment
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    
    // Other
    "skipLibCheck": true,
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## Best Practices

### Type Safety
```typescript
// Use strict mode
// "strict": true in tsconfig.json

// Avoid 'any'
// Bad
let data: any = fetchData();

// Good
interface Data {
    id: number;
    name: string;
}
let data: Data = fetchData();

// Use 'unknown' instead of 'any' when type is truly unknown
let value: unknown = getValue();
if (typeof value === 'string') {
    console.log(value.toUpperCase());
}
```

### Naming Conventions
```typescript
// Interfaces: PascalCase
interface UserProfile {}

// Types: PascalCase
type UserId = string;

// Enums: PascalCase
enum UserStatus {}

// Variables/Functions: camelCase
let userName: string;
function getUserName(): string {}

// Constants: UPPER_CASE or camelCase
const MAX_USERS = 100;
const apiUrl = 'https://api.example.com';

// Private class members: _camelCase (optional)
class User {
    private _id: number;
}
```

### Type vs Interface
```typescript
// Use interface for object shapes that might be extended
interface User {
    name: string;
    age: number;
}

interface Admin extends User {
    role: string;
}

// Use type for unions, intersections, primitives
type ID = string | number;
type Callback = () => void;
type Status = 'pending' | 'approved' | 'rejected';
```

### Immutability
```typescript
// Use readonly for immutable data
interface Config {
    readonly apiUrl: string;
    readonly timeout: number;
}

// Use ReadonlyArray or readonly modifier
const numbers: ReadonlyArray<number> = [1, 2, 3];
const names: readonly string[] = ['John', 'Jane'];

// Use 'as const' for literal types
const config = {
    host: 'localhost',
    port: 3000
} as const;
```

### Null Safety
```typescript
// Enable strictNullChecks
// Use union types for nullable values
function getUserName(user: User | null): string {
    return user?.name ?? 'Guest';
}

// Use optional chaining and nullish coalescing
const city = user?.address?.city ?? 'Unknown';
```

### Function Typing
```typescript
// Explicit return types for public APIs
function calculateTotal(items: Item[]): number {
    return items.reduce((sum, item) => sum + item.price, 0);
}

// Use arrow functions for callbacks
items.filter((item): item is ValidItem => item.isValid());

// Avoid function overloads when union types suffice
// Bad
function format(value: string): string;
function format(value: number): string;

// Good
function format(value: string | number): string {
    return String(value);
}
```

### Generic Best Practices
```typescript
// Use meaningful generic names
// Bad
function map<T, U>(arr: T[], fn: (item: T) => U): U[] {}

// Good (when context is clear)
function map<Item, Result>(
    arr: Item[],
    fn: (item: Item) => Result
): Result[] {}

// Constrain generics when possible
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}
```

### Error Handling
```typescript
// Create custom error types
class ValidationError extends Error {
    constructor(public field: string, message: string) {
        super(message);
        this.name = 'ValidationError';
    }
}

// Type-safe error handling
type Result<T, E = Error> =
    | { success: true; value: T }
    | { success: false; error: E };

function divide(a: number, b: number): Result<number> {
    if (b === 0) {
        return { success: false, error: new Error('Division by zero') };
    }
    return { success: true, value: a / b };
}
```

### Documentation
```typescript
/**
 * Calculates the total price of items in cart
 * @param items - Array of cart items
 * @param taxRate - Tax rate as decimal (e.g., 0.1 for 10%)
 * @returns Total price including tax
 * @throws {ValidationError} If items array is empty
 */
function calculateTotal(items: CartItem[], taxRate: number): number {
    if (items.length === 0) {
        throw new ValidationError('items', 'Cart cannot be empty');
    }
    const subtotal = items.reduce((sum, item) => sum + item.price, 0);
    return subtotal * (1 + taxRate);
}
```

---

**Pro Tip:** Enable strict mode, leverage TypeScript's type inference, use utility types effectively, and always prefer type safety over convenience!