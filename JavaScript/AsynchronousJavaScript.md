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

<div style="page-break-after: always;"></div>

## Promises

### What is a promise?

 A promise is a special JavaScript object that represents an eventual result of an asynchronous action, it's of a proxy (intermediary) for a value we do not have yet, such value will be coming from an async process as already mentioned.

 A promise allows us to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values as if they were synchronous methods: **instead of inmediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future**.

 A promise object has two internal properties, the **PromiseStatus** and the **PromiseValue**, if everything goes well in the async process, the PromiseValue property will contain the real value resolved by the promise.

 A promise can be in one of 3 different states:

- **Pending**: The initial state, the promise is neither fulfilled nor rejected.

- **Fulfilled**: Meaning the operation was completed successfully.

- **Rejected**: Meaning the operation failed.

### Creating a promise

To create a new promise, we just need to instantiate the special JavaScript class *Promise*. The *Promise* class constructor takes a single argument, which is required, this argument is a function called *executor*, and said function is invoked the very moment we create the new promise. 

The *executor* argument function takes two arguments: **resolve** and **reject**, both are functions as well. As mentioned already, initially the promise is at the *pending* state. **To transfer the promise to the rejected state, we must invoke the reject function, and to transfer the promise to the fulfilled state, we need to invoke the resolve function, inside of the body of the executor function which was the argument to the promise constructor**.

<div style="page-break-after: always;"></div>

Example promise that gets fulfilled or rejected randomly

```js
const myCustomPromise = new Promise(function(resolve, reject) {
    setTimeout(function() {
        const randomNumber = Math.floor((Math.random() * 10) + 1);
        if (randomNumber > 5) {
            reject({ message : 'Error. The random value is greater than five', randomNumber })
        }
        resolve(randomNumber)
    }, 1500);
});

myCustomPromise.then(function(fulfillmentValue) {
    console.log(`The promise resolved successfully: ${fulfillmentValue}`);
}).catch(function(rejectionValue) {
    console.log(`The promise was rejected!`);
    console.log(`Rejection info: ${JSON.stringify(rejectionValue)}`);
})
```

In this example we can see our *executor* function contains a setTimeout invocation meant to simulate an asynchronous process, after the waiting time gets exhausted, a random number in the range of 1 to 10 gets generated, if the number is greater to 5 then the promise is transferred to the rejected state and we provide an object as the rejection value, if not, the promise gets transfered to the fulfilled state, and the random value gets passed as the resolving value.

We can see that we have declared the handler functions in charge of dealing with the promise being **fulfilled** (**.then()**), or the promise being **rejected** (**.catch()**). 

**Output**

The random number is less than 5 and the promise is fulfilled

```console
The promise resolved successfully: 3
```

The random number is greater than 5, and the promise ends up being rejected

```console
The promise was rejected!
app.js:15 Rejection info: {"message":"Error. The random value is greater than five","randomNumber":9}
```

<div style="page-break-after: always;"></div>

### The final states of the promise

When we transfer a promise either to the fulfilled or rejected state, it is said that the promise has reached its final state, once that has happened, we cannot modify the state of the promise. In short, **after we have fulfilled a promise, we don't have the option of rejecting it any longer, and the other way around, if we have rejected a promise in the executor body, then we are not able to transfer it to the fulfilled status**. Let's review an example of both cases

We can't transfer the promise to the rejected state once it has been fulfilled, or fulfill it with a different value

```js
const myPromise = new Promise(function(resolve, reject) {
    resolve('I got resolved!');
    resolve('I got resolved, again!'); 
    reject('And finally I got rejected'); 
});

console.log(myPromise);
```

Output

```console
Promise {<fulfilled>: 'I got resolved!'}
[[Prototype]]: Promise
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: "I got resolved!"
```

And the other way around, we cannot transfer a promise to the fulfilled state once we have rejected it, and we can't change the rejection value either

```js
const myPromise = new Promise(function(resolve, reject) {
    reject('I was rejected!')
    resolve('I changed my mind and I am trying to be fulfilled, but that is not possible') 
    resolve('Trying to be fulfilled yet again') 
});
```

Output

```console
Promise {<rejected>: 'I was rejected!'}
[[Prototype]]: Promise
[[PromiseState]]: "rejected"
[[PromiseResult]]: "I was rejected!"
app.js:33 Uncaught (in promise) I was rejected!
```

<div style="page-break-after: always;"></div>

### Using and handling promises

> A great advantage promises offer over the callback function pattern, is that in the asynchronous code, it is possible to invoke the error-first callback function more than once by mistake, 'resolving' two times the asynchronous process, which would be a huge problem if we introduced a bug like this by not being careful. However, using promises, we saw in the previous code examples that we cannot resolve a promise (transfer the promise to its final state) more than once by design, the subsequent invocations to its resolve or reject methods are simply ignored, which acts as a safe mechanism.

When we provide the *onFulfilled* and the *onRejected* functions to the promise (the handlers for the promise), we are kind of subscribing to the result of the promise. These functions will be called asynchronously after the promise is either fulfilled or rejected, they are added to the message/task queue and will be executed only after the call stack become empty. Let's look at an example:

```js
const myPromise = new Promise(function(resolve, reject) {
    resolve('I was fulfilled/resolved inmediately!');
});

myPromise.then(function(result) {
    console.log(`From the onFulfilled handler function: ${result}`);
});

console.log('1');
console.log('2');
console.log('3');
console.log('In the original execution context of the script, the original entry in the call stack!');
```

Output

```console
1
2
3
In the original execution context of the script, the original entry in the call stack!
From the onFulfilled handler function: I was fulfilled/resolved inmediately!
```

As expected, the *onFulfilled* handler got placed in the task/message queue and executed only after the original execution context was cleared from the call stack.

<div style="page-break-after: always;"></div>

Another example, rewriting the previous *calculateSquare* demo function, but instead of using the callback pattern, accepting an error-first callback function, we return a promise, to which we are able to associate the *onFulfilled* and *onRejection* handlers, we can pass **both handlers** to the **.then()** method. Just as a note, in the definition of the *calculateSquarePromise*, we have a return in the *reject* invocation when provided an argument of the incorrect type, if we didn't have this return statement, the promise would be settled anyway by being rejected as expected, but the callback function in the *setTimeout* invocation would be added to the task/message queue and we would have to wait unecessarily.

```js
function calculateSquarePromise(number) {
    return new Promise(function(resolve, reject) {
        if (typeof number !== 'number') {
            return reject(new Error('Argument of type number is expected'));
        }
        setTimeout(function() {
            const squared = number * number;
            resolve(squared);
        }, 1500);
    })
}

calculateSquarePromise('NaN')
    .then( result => console.log(`This is the squared number: ${result}`), 
        error => console.log(`Promise was rejected. ${error.message}`))

console.log('Hello 1');
console.log('Hello 2');
console.log('Hello 3');
```

Output

```console
Hello 1
Hello 2
Hello 3
Promise was rejected. Argument of type number is expected
```

Just to reiterate, we can see that the *onRejected* handler got executed at the very end of the script because it was originally added to the task/message queue, and it was executed only after the original execution context of the script was cleared from the call stack.

<div style="page-break-after: always;"></div>

It's possible to chain multiple promises, thanks to the **.then()** method that returns a promise itself, in the *onFulfillment* function that is the first argument for the .then() method, we need to return a value, and that value will be the resolving/fulfillment value for the newly created promise, or we can even create a new explicit promise, as shown in the following example:

```js
function calculateSquarePromise(number) {
    return new Promise((resolve, reject) => {
        if (typeof number !== 'number') {
            return reject(new Error("Argument of type number is expected"));
        }
        setTimeout(function() {
            const result = number * number;
            resolve(result);
        }, 1500);
    })
}

calculateSquarePromise(2)
    .then(value => {
        console.log(`In the first then clause. Value: ${value}`);
        return calculateSquarePromise(value);
    })
    .then(value => {
        console.log(`In the second then clause. Value: ${value}`);
        return calculateSquarePromise(value);
    })
    .then(value => {
        console.log(`In the third then clause. Value: ${value}`);
    })
```

Output

```console
In the first then clause. Value: 4
In the second then clause. Value: 16
In the third then clause. Value: 256
```

As we can appreciate, in the first two *.then()* clauses, we are returning new explicit promises, we are invoking *calculateSquarePromise* using the value resolved from the previous promise, thus returning yet a new promise. But as already mentioned, we can simply return a value inside the *.then()* clause to make that our resolving value in this newly created promise, like this:

<div style="page-break-after: always;"></div>

```js
calculateSquarePromise(2)
    .then(value => {
        console.log(`In the first then clause. Value: ${value}`);
        return 'Hello, I am returninng this as my resolving value';
    })
    .then(value => {
        console.log(`I got this as the resolving value: ${value}`);
        
    })
```

Output

```console
In the first then clause. Value: 4
I got this as the resolving value: Hello, I am returninng this as my resolving value
```

Another thing really worth mentioning, is that the executor body in the promise, and even the *.then()* clause that returns a promise itself both have "invisible" try-catch statements, this means that we don't really need to reject the promise explicitly, if we throw an error inside the executor body or the *.then()* clause, the promise will automatically be rejected for us using that error, as the following example illustrates:

```js
const myTestPromise = new Promise(function(resolve, reject) {
    throw new Error('I throwed an error instead of explicitly rejecting!');
});

myTestPromise.catch(reason => {
    console.log(`The promise was rejected, ${reason}`);
})
```

Output

```console
The promise was rejected, Error: I throwed an error instead of explicitly rejecting!
```

<div style="page-break-after: always;"></div>

### Promise.all()

The *Promise.all()* mtehod takes an array or iterable of promises as its argument, and then returns a single promise that resolves to an array of the results of the input promises: This returned promise will resolve when all of the input's promises have resolved, or if the input iterable contains no promises. It rejects inmediately upon any of the input promises rejecting, and will reject with this first rejection message/error.