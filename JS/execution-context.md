# Execution Context

Every script/code starts with an execution context called a **global execution context**. And every time we call a function, a new execution context is created and is put on top of the *execution stack*. The same pattern follows when you call the nested function which calls another nested function.



Simply put, an execution context is an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context.

**Global Execution Context —** This is the default or base execution context. The code that is not inside any function is in the global execution context. It performs two things: it creates a global object which is a window object (in the case of browsers) and sets the value of `this` to equal to the global object.

**Functional Execution Context —** Every time a function is invoked, a brand new execution context is created for that function. Each function has its own execution context, but it’s created when the function is invoked or called. Any argument you pass in will be added as a local variable in that function’s Execution Context



The execution context is created in two phases: **1) Creation Phase** and **2) Execution Phase.**

![Globals variables in the creation phase](http://docs.salmanfarooqui.com/JS/images/global-variables-in-creation-phase.png)

![Global variables in the execution phase](http://docs.salmanfarooqui.com/JS/images/global-variables-in-execution-phase.png)

During creation phase, it sets up memory space for variables and functions.
Assign variable declarations a default value of “undefined” while placing any function declarations in memory.

It’s not until the `Execution` phase where the JavaScript engine starts running your code line by line and executing it.



## Creation Phase

In the **Global** `Creation` phase, the JavaScript engine will

1. Create a global object.
2. Create an object called “this”.
3. Set up memory space for variables and functions.
4. Assign variable declarations a default value of “undefined” while placing any function declarations in memory.

While in **Function** Execution Context creation phase, the JavaScript engine will

1. ~~Create a global object.~~

1. Create an arguments object.

2. Create an object called this.

3. Set up memory space for variables and functions.

4. Assign variable declarations a default value of “undefined” while placing any function declarations in memory.



The execution context is created during the creation phase. Following things happen during the creation phase:

Lexical Environment component is created.

Variable Environment component is created.



***Each Lexical Environment has three components:***

1. ***Environment Record***
2. ***Reference to the outer environment,***
3. ***This binding.***



### Environment Record

The environment record is the place where the variable and function declarations are stored inside the lexical environment.

*Note —* For the function code, the *environment record* also contains an `arguments` object that contains the mapping between indexes and arguments passed to the function and the *length(number)* of the arguments passed into the function.

```js
function foo(a, b) {
  var c = a + b;
}
foo(2, 3);// argument object
Arguments: {0: 2, 1: 3, length: 2},
```



### Reference to the Outer Environment

The **reference to the outer environment** means it has access to its outer lexical environment. That means that the JavaScript engine can look for variables inside the outer environment if they are not found in the current lexical environment.

```js
function b() {
	console.log(myVar);  // 1
}
function a() {
	var myVar = 2;
	b();
}
var myVar = 1;
a();
```

Outer environment of function b() is the Global execution context, even though a() is directly below b() in execution stack. Function **b sits lexically** in the same level as var myVar = 1, *it's not inside a()*

JavaScript cares about the lexical environment when it comes to outer reference that every execution context gets.

In regards to creation of execution context,it doesn't matter where the function is physical written. What matters is ***when you invoke them***. But when you invoke them, the JS engine creates an outer reference for that execution context and it looks up ***where the code was physically sitting***. Syntax parser had gone through your code line by line and it knows where your function was written and it then creates the appropriate out reference based on where your function was physically written in the JS file.



But if we change the lexical environment and put *function b* inside *function a*

```js
function a() {
	function b() {
		console.log(myVar);  // 2
	}	
	var myVar = 2;
	b();
}
var myVar = 1;
a();
```

Here *function b* is sitting physically inside a so it's outer reference will be *a* 



##### Scope chain

When a variable is used in JavaScript, the JavaScript engine will try to find the variable’s value in the current scope. If it could not find the variable, it will look into the outer scope and will continue to do so until it finds the variable or reaches global scope. Scope chain is this chain of references to outer environment.

Put another way, scope is about access.

Lexical Scope literally means that ***scope is determined at the lexing time*** (generally referred to as compiling) rather than at runtime.

```js
let number = 42;
function printNumber() {
  console.log(number); // Prints 42
}
function log() {
  let number = 54;
  printNumber();
}
log();
```

Here the `console.log(number)` will always print `42` no matter from where the function `printNumber()` is called. This is different from languages with the dynamic scope where the `console.log(number)` will print different value depending on from where the function `printNumber()` is called.

*If it’s still could not find the variable, it will either implicitly declare the variable in the global scope (if not in strict mode) or return an error.*



##### Note- Block Scope

ES6 introduced **let** and **const** variables, unlike **var** variables, they can be scoped to the nearest pair of curly braces. That means, they can’t be accessed from outside that pair of curly braces. For example:

```js
{
  let greeting = 'Hello World!';
  var lang = 'English';
  console.log(greeting); // Prints 'Hello World!'
}// Prints 'English'
console.log(lang);// Uncaught ReferenceError: greeting is not defined
console.log(greeting);
```

We can see that `var` variables can be used outside the block, that is, `var` variables are not block scoped.



### This Binding

In the global execution context, the value of `this` refers to the global object. (in browsers, `this` refers to the Window Object).

In the function execution context, the value of `this` depends on how the function is called. If it is called by an object reference, then the value of `this` is set to that object, otherwise, the value of `this` is set to the global object or `undefined`(in strict mode).

```js
const person = {
  name: 'peter',
  birthYear: 1994,
  calcAge: function() {
    console.log(2018 - this.birthYear);
  }
}person.calcAge(); 
// 'this' refers to 'person', because 'calcAge' was called with //'person' object referenceconst calculateAge = person.calcAge;
calculateAge();
// 'this' refers to the global window object, because no object reference was given
```



Note - During the creation phase, the code is scanned for variable and function declarations, while the function declaration is stored in its entirety in the environment, the variables are initially set to `undefined `(in case of `var`) or remain uninitialized (in case of `let `and `const`).



## Execution Stack

When the JavaScript engine first encounters your script, it creates a global execution context and pushes it to the current execution stack. Whenever the engine finds a function invocation, it creates a new execution context for that function and pushes it to the top of the stack.

The engine executes the function whose execution context is at the top of the stack. When this function completes, its execution stack is popped off from the stack, and the control reaches to the context below it in the current stack.

```js
let a = 'Hello World!';
function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}
function second() {
  console.log('Inside second function');
}
first();
console.log('Inside Global Execution Context');
```

![img](http://docs.salmanfarooqui.com/JS/images/1_ACtBy8CIepVTOSYcVwZ34Q.png)



## Closures

We learned that variables created inside of a function are locally scoped and they can’t be (**for the most part**) accessed once the function’s Execution Context has been popped off the Execution Stack. It’s time to dive into that ”**for the most part**”. The one scenario where this isn’t true is if you have a function nested inside of another function. In this case, the child function will still have access to the outer function’s scope, even after the parent function’s Execution Context has been removed from the Execution Stack.

![Visualizes closures](http://docs.salmanfarooqui.com/JS/images/closure-scope.gif)

Notice that after the `makeAdder` Execution Context has been popped off the Execution Stack, JavaScript Visualizer creates what’s called a `Closure Scope`. Inside of that `Closure Scope` is the same variable environment that existed in the `makeAdder` Execution Context. The reason this happened is because that we have a function nested inside of another function. In our example, the `inner` function is nested inside of the `makeAdder` function, so `inner` creates a `Closure` over the `makeAdder` variable environment. Even after the `makeAdder` Execution Environment has been popped off the Execution Stack, because that `Closure Scope` was created, `inner` has access to the `x` variable (via the Scope Chain).

As you probably guessed, this concept of a child function “closing” over the variable environment of its parent function is called `Closures`.



## Final Words

When a function is invoked, an execution context is created and placed on the execution stack. This execution context does these things -

- Create a global object. // if global execution context
- Create an arguments object. // if function execution context
- Create an object called “this”.
- Set up memory space for variables and functions.
- Assign variable declarations a default value of “undefined” while placing any function declarations in memory.

