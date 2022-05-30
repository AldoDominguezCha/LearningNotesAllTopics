# ReactJS

## JavaScript related concepts

> Hoisting: Hoisting is the process where the JS interpreter 'moves' the declaration of functions, variables and classes to the top of their scope, prior to the execution of the code

**const**: Introduced in ES6, we use it to declare a 'variable' that cannot have its value reassigned, as the name implies, **it is constant**. If we try to assign it a new value, we'll get a TypeError with the "Assignment to constant variable" message.

**let**: Introduced in ES6, we use it to declare a standard variable to which we can assign new values. It's **block scoped**, it only exists inside of the block (set of curly braces) where it was declared. The variables declared with let are hoisted, however, they are not initialized with a default value like it happens with _var_.

The _var_ function scope versus the _let_ block scope: Variables declared with _var_ have a function scope, which means they exist all the way across of the closest function where they are declared, they leak out of the possible blocks all the way to nearest function. Both _var_ and _let_ variables are hoisted, their declaration is moved to the top of the scope where they exist. The difference is that _var_ variables are initialized with a default value when hoisted, while _let_ variables are not initialized with a default value even though _let_ variables are hoisted all the same, let's review an example:

```js
function testVar() {
  console.log(`Value of i before the for loop where it is declared: ${i}`);
  for (var i = 1; i < 4; i++) {
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

Using _var_ for its declaration, we can see the _i_ variable is able to be referenced outside of the block it was declared, that's beacuse it exists in the scope of the entire _testVar_ function, the _i_ variable has been hoisted (its declaration is 'moved' to the top of the function scope) and initialized with a default value (_undefined_), that's why we are able to access it before the for loop without getting an error.

<div style="page-break-after: always;"></div>

On the other, using _let_ and referencing the variable before or after the block it is actually defined in (for loop), we'll get a reference error:

```js
function testLet() {
  //console.log(`Value of i before the for loop where it is declared: ${i}`);
  for (let i = 1; i < 4; i++) {
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

Additionally, as already mentioned, we can't access a _let_ variable before its initialization despite the variable being hoisted in its scope, in this case the interpreter knows about the existence of the variable, it's just not allowed to access it because it didn't get a default value in the hoisting process like _var_ variables do:

```js
console.log(foo);
let foo = "bar";
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

### ES6 Exports and Imports

When we use the ES6 export syntax, we can have a default export like so:

```js
//In person.js (the exporting module)
const person = {
  name: "Aldo",
};

export default person;
```

Then, that being the default export, if we import from that module without specifying a named import, we'll get that object, it doesn't matter if we assign it a different name:

```js
//In the importing module
import guy from "./person.js";

//'guy' will be our default object we exported in person.js, we just changed its name
```

If we have multiple exports in a module, we can have named imports, which is just us referencing the desired object from the exporting module by its original name, so the interpreter easily knows what we are asking for:

```js
//In utility.js (the exporting module)
export const clean = () => {...}
export const maxTimeoutInSeconds = 10;
```

Then in the module where we'll be importing we'll have to reference the object by their original name, with the possibility of associating an alias to the named import:

```js
//In the importing module
import { clean, maxTimeoutInSeconds as timeout } from "./utility.js";
```

<div style="page-break-after: always;"></div>

Finally, if we have multiple exports coming from a module, we can bundle together all the objects we are importing from that module, all objects will be grouped in a single object, the property name being the original name for the exported object:

```js
//In the importing module
import * as bundled from "./utility.js";
```

### The Spread and Rest operator

Spread operator

It allows us to split up or 'untangle' array elements or object properties. It's just three dots together "...":

```js
const oldArray = [1, 2, 3];
const newArray = [...oldArray, 4, 5];
console.log(newArray.join(", "));
```

Output

```console
"1, 2, 3, 4, 5"
```

The operator as the name indicates, allowed us to spread the elements from the original array to create the new array with additional elements.

We can perform the exact same operation with object properties:

```js
const oldObject = {
  name: "Aldo",
  age: 26,
};

const newObject = {
  ...oldObject,
  city: "CDMX",
  country: "MEX",
};

console.log(newObject);
```

<div style="page-break-after: always;"></div>

Output

```console
[object Object] {
  age: 26,
  city: "CDMX",
  country: "MEX",
  name: "Aldo"
}
```

Rest operator

This one is less common, it's used to merge a non-fixed size list of arguments in a function into an array:

```js
const test = (...args) => {
  args.forEach((el, index, array) => {
    console.log(`Argument number ${index + 1}. Value: ${el}`);
  });
};

test(5, 20, 36, 42, 726);
```

Output

```console
"Argument number 1. Value: 5"
"Argument number 2. Value: 20"
"Argument number 3. Value: 36"
"Argument number 4. Value: 42"
"Argument number 5. Value: 726"
```

### Reference types versus Primitive types

**Primitive types**: Numbers, strings, booleans, null, undefined and symbol. The JavaScript engine stores primitve types on the satck. Copying a primitve type value from one variable to another will create an independent value copy.

**Reference types**: Copying a reference type from one variable to another creates a new reference, so now the two variables refer to the exact same object, hence, if you do something to one of the variables, it will affect the other, since both are just different references for the same object.

<div style="page-break-after: always;"></div>

Primitive types

```js
let originalAge = 26;
let copyAge = originalAge;

copyAge = copyAge + 32;

console.log(`Original age value: ${originalAge}. Copied age value: ${copyAge}`);
```

Output

```console
"Original age value: 26. Copied age value: 58"
```

With our primitive type, when we created a copy from the original value and then modified the copy, it didn't change our original primitive type variable

Reference types

```js
let originalObject = {
  name: "Aldo",
  age: 26,
  city: "CDMX",
};

let copyObject = originalObject;

copyObject.name = "I changed this in the copy!";
copyObject.occupation = "Software developer";

console.log("Original object:");
console.log(originalObject);

console.log("Copy object:");
console.log(copyObject);
```

In this case, when we declared the _copyObject_ variable using the _originalObject_ variable, which stores a reference (it's a reference type) we just added a new reference to the stack for the same object tha lives in the heap. Basically we've just come up with two different ways to refer to the same object, that's why all the changes we did to the object using _copyObject_ are totally visible when inspecting _originalObject_.

<div style="page-break-after: always;"></div>

The same thing happens with arrays, they are reference types as well:

```js
let originalArray = [1, 2, 3];
let copyArray = originalArray;

copyArray.push(4);

console.log(`Original array: ${originalArray.join(", ")}`);
console.log(`Copy array: ${copyArray.join(", ")}`);
```

Output

```cosnole
"Original array: 1, 2, 3, 4"
"Copy array: 1, 2, 3, 4"
```

<div style="page-break-after: always;"></div>

## React basics

Components are the reusable building blocks that comprise our application, they are just a combination of HTML, CSS and possibly JS code for the associated logic in the component's behavior. A component doesn't necessarily have to be reused to be considered a component, reusability is just one of its potential perks.

A component is basically a custom HTML element. The _root component_ is main component being rendered in our starting file (_index.js_ at the moment of writing this), all additional components will be nested at different levels in our root component.

**A component in React is just a JavaScript function**.

### JSX syntax in React

When defining a component in React, we have access to the JSX syntax. **JSX** stands for *JavaScript XML*, it's the syntax that allows us to write HTML code directly in our components, making them easier to read and develop, as an example:

```js
import React from 'react';
import './ExpenseDate.css';

function ExpenseDate(props) {
  const month = new Intl.DateTimeFormat('en-US', { month: 'long' }).format(props.date);
  const year = props.date.getFullYear();
  const day = new Intl.DateTimeFormat('en-US', { day: '2-digit' }).format(props.date);

  return (
    <div className='expense-date'>
        <div className='expense-date__month'>{month}</div>
        <div className='expense-date__year'>{year}</div>
        <div className='expense-date__day'>{day}</div>
    </div>
  );
}

export default ExpenseDate;
```

We can see that we are returning what seems to be HMTL code with some dynamic content (the month, year and day), this is the **JSX** syntax.

<div style="page-break-after: always;"></div>

If we wanted to get rid of the JSX syntax, we'd have to use the *createElement* function coming from the *React* object, which is basically what happens in the background when using JSX anyway, we would just be removing the syntactic sugar, we can look at an example:

```js
import React from 'react';
import expenses from './TestData/ExpensesData';
import ExpensesHolder from './components/ExpensesHolder';


function App() {

  return React.createElement(
    'div', 
    {}, 
    React.createElement('h1', {}, 'We are just getting started!'),
    React.createElement(ExpensesHolder, { expenseObjects: expenses }),
  );
  
  //With the syntactic sugar from JSX  
  // return (
  //   <div>
  //     <h2>Let's get started!</h2>
  //     <ExpensesHolder expenseObjects={expenses}/>
  //   </div>
  // );
  
}

export default App;
```

In this case, instead of returning some HTML contanining our custom components, we return what we get from invoking *createElement* from the *React* object we got by importing the *react* module (at the top of the script). The first argument for the *createElement* function is the type of element we are creating, wheter it is a standard HTML element or a custom component, next comes the configuration object for the element, and finally, with a dynamic size, we provide the content of the element, which are tipically other elements, that's why we have nested *createElement* invokations.

If we take a look at this more low-level way of creating elements, we can understand why in JSX we can't return multiple sibling elements, we need to return a single element, because under the hood that JSX code is being transalated to the *createElement* syntax, and we can't have multiple return statements in a function, it doesn't make sense, that's why we need to wrap the entire content of our component in a single root element, tipically a div.

<div style="page-break-after: always;"></div>

### React component props

Standing for 'properties', the component props are custom HTML attributes used to pass data between components. Inside every React component, we can gain access to the *props* object as an argument to the function that defines the component, and in that object we'll find our custom HTML attributes with the value we provided for them when we used our component, as key-value pairs. Let's look at an example:

```js
import React from 'react';
import './ExpensesHolder.css';
import Card from './../UI/Card';
import ExpenseItem from './ExpenseItem';

function ExpensesHolder(props) {

    const expenseElements = [];
    props.expenseObjects.forEach(expense => {
        expenseElements.push(
            <ExpenseItem expenseObject={expense} />
        );
    });

    return(
        <Card className="expenses">
            {expenseElements}
        </Card>
    );
}

export default ExpensesHolder;
```

Here we can see that when adding an *ExpenseItem* component, we are providing it with a *expenseObject* attribute, for which are provding an object that contains the information our component will be rendering. 

<div style="page-break-after: always;"></div>

Now inside the actual component:

```js
import React from 'react';
import ExpenseDate from './ExpenseDate';
import Card from './../UI/Card';
import './ExpenseItem.css';

function ExpenseItem(props) {

  let { title: expenseTitle, amount: expenseAmount, date: expenseDate } = props.expenseObject;
  
  
  return (
    <Card className="expense-item">
      <ExpenseDate date={expenseDate}/>
      <div className='expense-item__description'>
        <h2>{expenseTitle}</h2>
        <div className='expense-item__price'>{expenseAmount}</div>
      </div>
    </Card>
  );
}

export default ExpenseItem;
```

Here we are accessing the object we passed to the component through the *props* argument we added to the function definition, and the key in the props object is the attribute name we gave to the component (*props.expenseObject*), additionally we can see how we are simply deconstructing this object so we can use its properties in different elements of the component, that's how we pass data to a react component.

## React states and events

The state hook adds reactivity to our application, it allows us to define an initial state for a variable inside the component, then update the state or value of that variable, only for that particular component instance, then that specific component instance is rendered once again, so it acn reflect its latest state in the DOM.

> The state is separated on a per-component instance basis. Every instance of the same component has its own state, this is really good since we can have multiple instances of the same component, this way only the state of the particular instance we are interacting with will be updated, and the instance rendered again.