# Fundamentals of TypeScript

## Basic TypeScript features

> TypeScript is a JavaScript superset, basically a programming language building up on JavaScript. **TypeScript can't be executed by JavaScript environments like the browser or Node.js**.

TypeScript is a programming language, but we must think of it as whole tool, including the compiler that compiles our TypeScript code into JavaScript. We write TypeScript code, with all the new features and all the advantages, then we compile it into regular JavaScript code.

**But how can TypeScript add new features if what we get in the end is regular JavaScript?**

The TypeScript compiler compiles these new features into JavaScript workarounds, we use the "new" and nicer way of doing things in TypeScript and then our code gets compiled into complex JavaScript snippets that we would have to write ourselves otherwise.

As the name implies, TypeScript incorporates types, which is a much safer way to develop our applications and allows us to identify errors before the error occurs at runtime in the browser or the runtime environment, and fix them at development stage.

**TypeScript's type system can only help us during development, before the code gets compiled**

TypeScript, unlike JavaScript, is a strongly typed programming language, as its name implies, the variable types are known at compile time. Remembering that JavaScript is dynamically-typed, the interpreter assigns variables a type at runtime based on the variable's value at the time.

The additions TypeScript brings to the table are, to mention some of them:

- Strict typing, it's in the language's name!

- Interfaces

- Generics

- Meta-programming features, like decorators

**In both TypeScript and JavaScript, all numbers are floats by default**. There's not a more precise distinction in assigning the required bytes to store the numeric variables, like there is for example in .NET with C#, where we can specify if the numeric variable is an int, long, float, double, short, uint, etc.

<div style="page-break-after: always;"></div>

## TypeScript data types

**Boolean** &rarr; The most basic datatype is the simple true/false value, which both JavaScripn and TypeScript call a boolean value.

```ts
let isDone: boolean = false;
```

**Number** &rarr; Just like in JavaScript, all numbers in TypeScript are either floating point values or BigIntegers. The floating point numbers get the type **number**, while BigIntegers get the type **bigint**. TypeScript also supports binary, hexadecimal and octal literals introduced in ES2015.

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

**String** &rarr; The ubiquitous type for textual data, to sorround string data we can use either double or single quotes for multiple characters. Additionally we can use backticks/backquotes to declare a *template string*, which have inside of them embedded expressions.

```ts
let color: string = "blue";
color = 'red';

let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}.
 
I'll be ${age + 1} years old next month.`;
```

**Array** &rarr; Collection of values of the same type. Arrays can be written in two ways, using the type of elements it will contain followed by **[]**. Or using a generic array type.

```ts
let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];
```

<div style="page-break-after: always;"></div>

## TypeScript data types

**Tuple** &rarr; Tuples are basically arrays, with a fixed size, whose types are known, **but need not be the same**. For example, we can have a tuple containing a number and a string.

```ts
let x: [string, number];
x = ["hello", 10]; // OK
x = [10, "hello"]; // Error
```

With tuples we basically specify what type we can store at what index in the fixed-size array. But as a note, the **.push()** method can be used to alter the length of our tuple, this method is an exception to this fixed-size constraint.

**Enum** &rarr; A set of predfined values or "options". Using enums we can focus on what the values mean rather than worry about how they are stored and accessed. In the background and by default, the available options for the enum are just numeric values, we can specify at what numeric value we want to begin numbering the enum members, since the default starting value is 0.

```ts
enum Role {
    ADMIN = 1,
    READ_ONLY,
    AUTHOR
};
```

**Any** &rarr; A special type that is used when we don't want a certain value to cause typechecking errors, by using any we basically lose the type checks. When a value is of type any, we can acess any property of it (which will in turn be of type any as well), call it like a function, assign it to (or from) a value of any type, or pretty much anything else that's sintactically legal.

```ts
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed 
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

<div style="page-break-after: always;"></div>

## TypeScript data types

**Union Types** &rarr; We can build new types out of existing ones, combining two or more other types, representing a value that can be any of those types. We refer to each of those types as the union's members. But in a union type, whenever we want to access methods or operators specific only to one of the union's members, we need to *narrow* the union with code. Narrowing occurs when TypeScript can deduce a more specific type for a value based on the structure of the code. Just to clarify, the union type in the example is the parameter 'x' the function takes in **string[] | string**, they are two distinct data types combined with the pipe operator.

```ts
function welcomePeople(x: string[] | string) {
  if (Array.isArray(x)) {
    // Here: 'x' is 'string[]', TypeScript is sure of that because of 
    //the structure of the code, by checking if the value is array first
    console.log("Hello, " + x.join(" and "));
  } else {
    // Here: 'x' is 'string'
    console.log("Welcome lone traveler " + x);
  }
}
```

**Literal Types** &rarr; We can refer to specific strings and numbers in type positions, they aren't really useful on their own inside a variable, but by combining literals into unions, we can express handy concepts, like limiting the permiited valid options we can provide as a parameter to a certain function. **When forming these union types, we can also combine literals with non-literal types**.

```ts
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
//Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.

function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}

interface Options { width: number }
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic");
//Argument of type '"automatic"' is not assignable to parameter of type 'Options | "auto"'.
```

<div style="page-break-after: always;"></div>

## TypeScript data types

**Type aliases** &rarr; A type alias is very simple, exactly what the name implies, *a name for any type*.  A type alias is very similar to an interface.

```ts
type Point = {
  x: number;
  y: number;
};
 
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });


type ID = number | string;
```

**Functions** &rarr; After the parameter parentheses in the function declaration, using a colon character we can specify the return type of the function. Additionally, when declaring a variable that will store a function reference, we can specify its **function type**, which is basically the signature, that's to say, the number and type of the parameters, and the return type of the function it should hold.

```ts
function add(n1: number, n2: number): number {
    return n1 + n2;
}

function printResult(num: number): void {
    console.log('Result is: ' + num);
}

let combineValues: (x: number, y: number) => number = add; //OK, matching func types

combineValues = printResult; //Error, printResult doesn't have the required function type (signature)

//Type '(num: number) => void' is not assignable to type '(x: number, y: number) => number'.
//Type 'void' is not assignable to type 'number'.
```

**Null and undefined** &rarr; They are both two primitive values. **Null** indicates that the field does not have a value, it is an **intentional** absence of value. **Undefined** means the value is not assigned, it is an **unintentional** absence of value.

<div style="page-break-after: always;"></div>

## TypeScript data types

**Unknown** &rarr; It's very similar to *any*. As the name implies, we don't exactly know what type of value it will hold, you can assign different types to a variable of type *unknown*. The difference with *any* is that *unknown* is safer, it's not legal to do just any sintactically valid operation with an *unkown* type.

```ts
let userInput: unknown;
let userName: string;

userInput = 5;
userInput = { message : 'Hello' };
userInput = 'Aldo';

userName = userInput; //Generates error, we don't know for sure if userName holds a string

if (typeof userInput === 'string') {
    //Here we are narrowing the type with code, this one does not generate an error
    // because inside this if statement TS knows for sure that userInput will
    //contain a string
    userName = userInput;
}
```

**Never** &rarr; Some functions never return a value, under any circumstance. The never type represents values which are never observed. In a return type, this means tha the function throws an exception or terminates execution of the program.

```ts
type errorContent = { message : string, code : number }

function generateError(error : errorContent): never {
    throw { errorMessage : `Error: ${error.message}`, errorCode : error.code };
}

const result = generateError( { message: 'Stack overflow!', code: 500 });
```

**Void** &rarr; Represents the return type of functions which don't return a value. It's the inferred type any time a function doesn't have any return statement, or doesn't return any explicit value from its return statement.

```ts
function addAndPrint(x: number, y: number): void {
    const result = x + y;
    console.log(`I added: ${result}`);
}
//Even if we don't explicitly specify, the inferred return type for 
//the function would be void anyway, because of the lack of a return statement
```

<div style="page-break-after: always;"></div>

## The TypeScript compiler