# Closure

A closure is a function that has access to its outer function scope even after the outer function has returned. This means a closure can remember and access variables and arguments of its outer function even after the function has finished.

## Lexical Scope

A lexical scope or static scope in JavaScript refers to the accessibility of the variables, functions, and objects based on their physical location in the source code.



#### Example 1

```js
function person() {
  let name = 'Peter';
  
  return function displayName() {
    console.log(name);
  };
}let peter = person();
peter(); // prints 'Peter'
```

In this code, we are calling `person` function which returns inner function `displayName` and stores that inner function in `peter` variable. When we call `peter` function (which is actually referencing the `displayName` function), the name ‘Peter’ is printed to the console.

But we don’t have any variable named `name` in `displayName` function, so this function can somehow access the variable of its outer function `person` even after that function has returned. So the `displayName` function is actually a closure.



#### Example 2

```js
function getCounter() {
  let counter = 0;
  return function() {
    return counter++;
  }
}let count = getCounter();console.log(count());  // 0
console.log(count());  // 1
console.log(count());  // 2
```

Notice that the value of the `counter` is not resetting to `0` on each `count` function call as it usually should.

That’s because, at each call of `count()`, a new scope for the function is created, but there is only single scope created for `getCounter` function, because the `counter` variable is defined in the scope of `getCounter()`, it would get incremented on each `count` function call instead of resetting to `0`.



To really understand how closures work in JavaScript, we have to understand the two most important concepts in JavaScript, that is, 

**1) Execution Context**

**2) Lexical Environment**



## Execution Context

An execution context is an abstract environment where the JavaScript code is evaluated and executed.

There can only be one currently running execution context (Because JavaScript is single threaded language), which is managed by a stack data structure known as Execution Stack or Call Stack.

The currently running execution context will be always on the top of the stack, and when the function which is currently running completes, its execution context is popped off from the stack and the control reaches to the execution context below it in the stack.

![img](https://miro.medium.com/max/1282/1*fYq9aQ9OMhO-THHrr7N54w.png)



## Lexical Environment

Every time the JavaScript engine creates an execution context to execute the function or global code, it also creates a new lexical environment to store the variable defined in that function during the execution of that function.

A *lexical environment* is a data structure that holds **identifier-variable mapping**. (here **identifier** refers to the name of variables/functions, and the **variable** is the reference to actual object [including function type object] or primitive value).

A Lexical Environment has two components:

1. The **environment record** is the actual place where the variable and function declarations are stored.
2. The **reference to the outer environment** means it has access to its outer (parent) lexical environment. This component is the most important in order to understand how closures work.

A lexical environment conceptually looks like this:

```js
lexicalEnvironment = {
  environmentRecord: {
    <identifier> : <value>,
    <identifier> : <value>
  }
  outer: < Reference to the parent lexical environment>
}
```



So let’s again take a look at above code snippet:

```js
let a = 'Hello World!';function first() {
  let b = 25;  
  console.log('Inside first function');
}
first();
console.log('Inside global execution context');
```

So the lexical environment for the global scope will look like this:

```js
globalLexicalEnvironment = {
  environmentRecord: {
      a     : 'Hello World!',
      first : < reference to function object >
  }
  outer: null
}
```

The lexical environment of the function will look like this:

```js
functionLexicalEnvironment = {
  environmentRecord: {
      b    : 25,
  }
  outer: <globalLexicalEnvironment>
}
```



**Note —** When a function completes, its execution context is removed from the stack, but its lexical environment may or may not be removed from the memory depending on if that lexical environment is referenced by any other lexical environments in their outer lexical environment property.

**Note —** Don’t confuse lexical scope with the lexical environment, ***lexical scope is a scope that is determined at compile time and a lexical environment is a place where variables are stored during the program execution.***



#### Lets look at Example1 again

```js
function person() {
  let name = 'Peter';
  
  return function displayName() {
    console.log(name);
  };
}let peter = person();
peter(); // prints 'Peter'
```

When the `person` function is executed, the JavaScript engine creates a new execution context and lexical environment for the function.

So its lexical environment will look like this:

```js
personLexicalEnvironment = {
  environmentRecord: {
    name : 'Peter',
    displayName: < displayName function reference>
  }
  outer: <globalLexicalEnvironment>
}
```

When the `person` function finishes, its execution context is removed from the stack. But its lexical environment is still in the memory because its lexical environment is referenced by the lexical environment of its inner `displayName` function. So its variables are still available in the memory.

When the `peter` function is executed (which is actually a reference to the `displayName` function), the JavaScript engine creates a new execution context and lexical environment for that function.

So its lexical environment looks like this:

```js
displayNameLexicalEnvironment = {
  environmentRecord: {
    
  }
  outer: <personLexicalEnvironment>
}
```

As there’s no variable in `displayName` function, its environment record will be empty. During the execution of this function, the JavaScript engine will try to find the variable `name` in the function’s lexical environment.

As there are no variables in the lexical environment of `displayName` function, it will look into the outer lexical environment, that is, the lexical environment of the `person` function which still there in the memory. The JavaScript engine finds the variable and `name` is printed to the console.



#### Lets look at Example2 again

```js
function getCounter() {
  let counter = 0;
  return function() {
    return counter++;
  }
}
let count = getCounter();
console.log(count());  // 0
console.log(count());  // 1
console.log(count());  // 2
```

When the `count` function is called, the JavaScript engine will look into the lexical environment of this function for the `counter` variable. Again as its environment record is empty, the engine will look into the outer lexical environment of the function.

The engine finds the variable, prints it to the console and will increment the counter variable in the `getCounter` function lexical environment.

So the lexical environment for the `getCounter` function after first call `count` function will look like this:

```
getCounterLexicalEnvironment = {
  environmentRecord: {
    counter: 1,
    <anonymous function> : < reference to function>
  }
  outer: <globalLexicalEnvironment>
}
```

