# Design patterns

**What is a design pattern?**

A software design pattern is basically a **technique**, a reusable solution of how to solve a common problem when designing an application or system. Unlike a library or framework, which can be inserted and used right away, a design pattern is more of a template to approach the problem at hand.

There are three different types of design patterns

- **Creational design patterns** deal with object creation and initialization, providing guidance about which objects are created for a given situation. These design patterns are used to increase flexibility and to reuse existing code.

- **Structural design patterns** deal with class and object composition, or how to assemble objects and classes into larger structures.

- **Behavioral design patterns** are concerned with communication between objects and how responsibilities are assigned between objects.

<div style="page-break-after: always;"></div>

## Singleton

The *singleton* design pattern aims to use only a singular instance of a class across the application, it is a **creational** design pattern. Basically the approach is to have a static property in the class that holds the single instance, and in the constructor generate said single instance if it hasn't been created yet and assign it to the static field, otherwise, simply return the already created single instance.

Example in JavaScript

```js
class Singleton {
    
    static instance = null;

    constructor() {

        this.random = Math.random();

        if (Singleton.instance) {
            // We check if our static property already contains the single instance
            //if so, simply return that single instance
            return Singleton.instance;
        }

        // If we skip the if, meaning our single instance is not yet assigned,
        // we assign it, using the current object (this)
        Singleton.instance = this;

    }
}
```

Now, using the *random* property in the class, we'll see that we are getting the same instance every time:

```js
for (let i = 1; i <= 10; i ++) {
    let currentSingleton = new Singleton();
    console.log(`Singleton ${i}. Random: ${currentSingleton.random}`);
}
```

<div style="page-break-after: always;"></div>

Output

```console
Singleton 1. Random: 0.23071954710893583
singleton.js:99 Singleton 2. Random: 0.23071954710893583
singleton.js:99 Singleton 3. Random: 0.23071954710893583
singleton.js:99 Singleton 4. Random: 0.23071954710893583
singleton.js:99 Singleton 5. Random: 0.23071954710893583
singleton.js:99 Singleton 6. Random: 0.23071954710893583
singleton.js:99 Singleton 7. Random: 0.23071954710893583
singleton.js:99 Singleton 8. Random: 0.23071954710893583
singleton.js:99 Singleton 9. Random: 0.23071954710893583
singleton.js:99 Singleton 10. Random: 0.23071954710893583
```

We get the same random number every time, this is due to the fact that we are getting the exact same instance of the class because of the static *instance* property and the code in the constructor. **We are just asking if we already have the single instance, if we do, we return it when creating the new object, if we don't we get the current instance in the constructor and we assign it to the static property**.

Another thing we can do is verifying if we have the exact same instance using the strict comparison:

```js
const singleton1 = new Singleton();
const singleton2 = new Singleton();

console.log(`Are they the exact same instance? ${singleton1 === singleton2}`);
```

Output

```console
Are they the exact same instance? true
```

<div style="page-break-after: always;"></div>

Now let's see how we can structure a *singleton* using TypeScript, which is very similar to the way we've done it using JavaScript, except we can take advantage of the benefits TypeScript offers us, like the visibility modifiers for the properties and methods of the class.

```ts
class Singleton {

    private static instance: Singleton;
    private static instanceCount: number = 0;
    public random: number;

    private constructor() {
        this.random = Math.random();
    }

    public static getInstance(): Singleton {

        Singleton.instanceCount++;

        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }

        return Singleton.instance;
    }

    public static getInstancesCount(): number {
        return Singleton.instanceCount;
    }

}
```

In this implementation, we are restricting the use of the class' constructor outside of the own class, since it has the *private* visibility modifier we can access said constructor exclusively in the context of the class, that's why we have the *getInstance* static method, which will be the intermediary so we can get a "new" instance of this class. In this *getInstance* method we are checking if we already have our single instance hosted in the *instance* private static property, we've made this *instance* static property private so it can't be modified by mistake outside of our *getInstance* method, since its assignment to an actual instance should happen only once, this way we are careful, we are not exposing the core of the *singleton* to external processes.

Again we've added a *random* public property, to verify that we are actually getting the exact same instance (our single instance) every time, and also an *instanceCount* static counter was added so we can know how many times we've invoked the *getInstance* method at any point.

<div style="page-break-after: always;"></div>

Now, invoking *getInstance* multiple times, we'll see once again that we obtain the same random number each time, since we are getting the exact same instance of our singleton class:

```ts
for (let i = 1; i <= 10; i++) {
    let currentSingleton = Singleton.getInstance();
    console.log(`We have asked for ${Singleton.getInstancesCount()} instance(s) of the singleton!`);
    console.log(`Yet the random number of the singleton is: ${currentSingleton.random}`);
}
```

Output

```console
We have asked for 1 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 2 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 3 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 4 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 5 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 6 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 7 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 8 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 9 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
We have asked for 10 instance(s) of the singleton!
Yet the random number of the singleton is: 0.4848404541144413
```
