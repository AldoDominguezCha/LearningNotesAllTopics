# ReactJS

## JavaScript related concepts

> Hoisting: Hoisting is the process where the JS interpreter 'moves' the declaration of functions, variables and classes to the top of their scope, prior to the execution of the code

**const**: Introduced in ES6, we use it to declare a 'variable' that cannot have its value reassigned, as the name implies, **it is constant**. If we try to assign it a new value, we'll get a  TypeError with the "Assignment to constant variable" message.

**let**: Introduced in ES6, we use it to declare a standard variable to which we can assign new values. It's **block scoped**, it only exists inside of the block (set of curly braces) where it was declared. The variables declared with let are hoisted, however, they are not initialized with a default value like it happens with *var*.

The *var* function scope versus the *let* block scope: Variables declared with *var* have a function scope, which means they exist all the way across of the closest function where they are declared, they leak out of the possible blocks all the way to nearest function. Both *var* and *let* variables are hoisted, their declaration is moved to the top of the scope where they exist. The difference is that *var* variables are initialized with a default value when hoisted, while *let* variables are not initialized with a default value even though *let* variables are hoisted all the same, let's review an example:

```js
function testVar() {
  console.log(`Value of i before the for loop where it is declared: ${i}`);
  for(var i = 1; i < 4; i++) {
    console.log(`i inside the for loop where it is declared: ${i}`);
  }
  console.log(`Value of i after the for loop where it is declared: ${i}`);
}

testVar();
```

Output

```console
"Value of i before the for loop where it is declared: undefined"
"i inside the for loop where it is declared: 1"
"i inside the for loop where it is declared: 2"
"i inside the for loop where it is declared: 3"
"Value of i after the for loop where it is declared: 4"
```

Using *var* for its declaration, we can see the *i* variable is able to be referenced outside of the block it was declared, that's beacuse it exists in the scope of the entire *testVar* function, the *i* variable has been hoisted (its declaration is 'moved' to the top of the function scope) and initialized with a default value (*undefined*), that's why we are able to access it before the for loop without getting an error.

<div style="page-break-after: always;"></div>

On the other, using *let* and referencing the variable before or after the block it is actually defined in (for loop), we'll get a reference error:

```js
function testLet() {
  //console.log(`Value of i before the for loop where it is declared: ${i}`);
  for(let i = 1; i < 4; i++) {
    console.log(`i inside the for loop where it is declared: ${i}`);
  }
  console.log(`Value of i after the for loop where it is declared: ${i}`);
}

testLet();
```

Output

```console

  //console.log(`Value of i before the for loop where it is declared: ${i}`);
  for(let i = 1; i < 4; i++) {
    console.log(`i inside the for loop where it is declared: ${i}`);
  }
  console.log(`Value of i after the for loop where it is declared: ${i}`);
}
​
testLet();
​
"i inside the for loop where it is declared: 1"
"i inside the for loop where it is declared: 2"
"i inside the for loop where it is declared: 3"
"ReferenceError: i is not defined
    at testLet (null.js:8:95)
    at null.js:11:1
    at https://static.jsbin.com/js/prod/runner-4.1.8.min.js:1:13924
    at https://static.jsbin.com/js/prod/runner-4.1.8.min.js:1:10866"
```

Additionally, as already mentioned, we can't access a *let* variable before its initialization despite the variable being hoisted in its scope, in this case the interpreter knows about the existence of the variable, it's just not allowed to access it because it didn't get a default value in the hoisting process like *var* variables do:

```js
console.log(foo);
let foo = 'bar';
```

<div style="page-break-after: always;"></div>

Output

```console
"error"
"ReferenceError: Cannot access 'foo' before initialization
    at paroxubofi.js:2:40
    at https://static.jsbin.com/js/prod/runner-4.1.8.min.js:1:13924
    at https://static.jsbin.com/js/prod/runner-4.1.8.min.js:1:10866"
```


