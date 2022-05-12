# Asynchronous JavaScript

> Asynchronous (async) programming allows us to execute a block of code without stopping (or blocking) the entire thread where the action is being executed. A common myth about async code is that it improves performance, which isn't necessarily true.

A small example

```js
let a = 1;
let b = 2;

setTimeout(function() {
  console.log('After timeout!')
}, 300);

console.log(a);
console.log(b);
```

Output:

```console
1
2
After timeout!
```

In this example, we can appreciate how we are invoking the setTimeout method, which executes a provided function after a certain period of time which we also specify, we are making the call to this setTimeout function **before** logging the values of **'a'** and **'b'**, however, in the output we can clearly see that in reality both the log statements were executed before executing the function we have provided to the setTimeout call, this means that the execution thread was not blocked by invoking the **asynchronous** setTimeout function, after making this call, the code execution continued, without the need of actually waiting for the time provided to run out, and after the time actully ran out, the asynchronous setTimeout function resolved by executing the function we provided as a parameter.

In simple words, when you execute something **synchronously**, you need to wait until it finishes executing completely before you can move on to the next operation. When executing something **asynchronously**, you can move to the next process or statement without having to wait for the asynchronous process to be finished, then at some point, the asynchronous process resumes.

<div style="page-break-after: always;"></div>

## About the call stack and task/message queue in the JS engine

The JavaScript engine is the software component that executes the JavaScript code while browsing, or using the Node.js runtime environment. The JavaScript engine uses two important data structures in order to execute the code: **call stack** and **message/task queue**.

**The call stack**: When a function starts executing, a new execution context gets created and placed on top of the call stack, when the function returns or exhausts all its statements, the execution context associated to this function is removed from the call stack.

**The message/task queue**: The message queue contains a list of tasks to be processed, each task refers to a function that needs to be executed at some point in time, there are different ways of adding tasks to this queue.

> The event loop is a background process in the JavaScript engine that is constantly checking if there are any new tasks in the message/task queue that need to be executed.

<div style="page-break-after: always;"></div>

## Callback functions

A callback function is any function that is passed as an argument to another function, and then invoked from within that function.

> JavaScript functions are first class citizens, this means that functions themselves are objects. Functions can be stored in variables, be passed as arguments to other functions, and even be returned form other functions.

Callback functions can be passed as a parameter in both **synchronous** and **asynchronous** functions. So, not every function that receives a callback function as a parameter is sure to be asynchronous. Let's look at an example.

**Passing a callback to an asynchronous function**

```js
function f(callbackFn) {
    setTimeout(() => callbackFn(), 0);
}

f(() => console.log('This is my callback argument!'));
console.log('Hello, world!')
```

**Output**

```console
Hello, world!
This is my callback argument!
```

In this case, even when we have provided 0 miliseconds as the expiry time for the setTimeout function, the "Hello, world!" statement was logged first, because despite the 0 miliseconds waiting time, the setTimeout function has put our callback function inside of the message/task queue, and the task/message queue starts being executed until the current call stack is empty (the execution context associated to the entire script needs to be completed first, which includes the log statement with the "Hello, world!" message).

**Passing a callback to a synchronous function**

```js
function f(callbackFn) {
    callbackFn();
}

f(() => console.log('This is my callback argument!'));
console.log('Hello, world!')
```

<div style="page-break-after: always;"></div>

**Output**

```console
This is my callback argument!
Hello, world!
```

In this scenario we have removed the setTimeout invocation, so this time our callback function doesn't get placed in the task/message queue unlike the previous example, instead it goes normally as an additional execution context into the call stack, **so basically, not just beacuse a function takes a callback function as a paremeter it is treated as an asynchronous function, it can remain as synchronous, depending on its contents**.

**Handling errors in asynchronous code with our callback functions**

We cannot handle errors in our asynchronous functions using a conventional try-catch block, because the execution runs through our asynchronous function execution in the try clause and moves on, since the asynchronous process got placed in the message/task queue, that's why the error escapes when it is thrown inside our async function, fot that reason, we can implement an **error-first callback function**, which is a callback function that as its first parameter receives the potential error coming from our async operation.

In the definition of this **error-first callback function** we need to test if we actually received an error object coming from the asynchronous operation, if so, we handle the error, if not, we just move on, with the option of further processing any data we may have received from the asynchronous process that called our callback function. Let's review an example.

```js
function calculateSquare(number, callback) {
    setTimeout(function() {
        if (typeof number !== 'number') {
            callback(new Error('Argument of type number is expected'));
            return;
        }
        const result = number * number;
        callback(null, result);
    }, 1000);
}

function myCallback(error, result) {
    if (error) {
        console.log(`Caught error: ${String(error)}`);
        return;
    }
    //The error argument passed was falsy (null), we can proceed with the result
    console.log(`This is my result: ${result}`);
}
```

<div style="page-break-after: always;"></div>

In this case the *calculateSquare* function is our async process, which after some time will yield a result (we are simulating the time required for a calculation with *setTimeout*), after the timeout, the async process can either return a result (if the argument provided for the calculation is a number as expected) or an error (if the type of the first argument provided to *calculateSquare* is other than number), given these two possible outcomes, we need to handle both inside our **error-first callback function**, since as already mention, we can't just wrap the execution of *calculateSquare* in a try-catch block, the error will just leak unhandled.

Now, for the definition of our error-first callback function, we can see that we require two paremeters, the first one being the potential error (as already advertised by this technique's name) and the second parameter being the possible result our asynchronous process may pass to our callback function, we can see that we just need to define what to do in case *error* is a truthy value (not null), or carry on with our result if this is not the case (in this callback function we are just logging the result to the console).

Now, testing the behavior of our error-first callback function, with a bad argument (non numeric type), our callback function handles the error by logging it

```js
calculateSquare('bad argument', myCallback);
```

Output

```console
Caught error: Error: Argument of type number is expected
```

Trying again with the correct type as the argument, we access the happy path section of the callback function, since *error* is a falsy value (null)

```js
calculateSquare(15, myCallback);
```

Output

```console
This is my result: 225
```

As an additional note, the **benefits** of callback functions are their **simplicity** to implement and also their **popularity** among several frameworks and packages, such as React or jQuery. The **disadvantages** of using callback functions are the **lack of readability** and also the so called **callback hell (many nested callback function invocations)**.




