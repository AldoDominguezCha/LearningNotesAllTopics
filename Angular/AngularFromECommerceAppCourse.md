# Angular in the ECommerce app development course

These are just some miscellaneous notes on Angular while using it to develop a POC ECommerce app.

**POC: Proof of concept**.

> Angular, in its official website, defines itself as a development platform built on Typescript. 

As a platform, Angular includes:
- A component-based framework to build scalable web applications
- A collection of well-integrated libraries that cover a wide variety of features, including routing, forms management, client-server communication, and more
- A suite of developer tools to help you develop, build, test, and update your code

**Every angular app must have at least one module, they are decorated with the '@NgModule' decorator**

With the **@NgModule** decorator, the class is marked as an angular module and it is supplied some configuration metadata. **Inside our modules we declare our angular components which are the building blocks of the application**

It's tricky to get Angular to work with jQuery, since jQuery is a DOM manipulation library, the DOM in our browser gets manipulated by jQuery, and Angular tracks changes in the DOM and reacts to those changes, Angular's change detection gets confused, to use Bootstrap with our Angular app, we can use Angular bootstrap (ngx-bootstrap), which is an npm package that provides Bootstrap components powered by Angular, like wrapping the famous ready-to-use Bootstrap components in Angular components.

<div style="page-break-after: always;"></div>

## Angular CLI commands

> **ng** is the command to interact with the Angular CLI, like "dotnet" is the command to interact with the .NET CLI

Create new angular project wihtout strictr type checking:

```console
>> ng new <projectName> --strict false
```

Run the angular application, assuming we are in the directory of our angular project:

```console
>> ng serve
```

Generate new component for the application, without creating "spec.ts" test files for the new component. By default the new component gets generated inside a folder of its own, with the same name we gave to the component:

```console
>> ng generate component <component-name> --skip-tests
```

