# Functions

Functions are the main “building blocks” of the program. They allow the code to be called many times without repetition.

## function Declarations vs. Expressions

A **declared function** is “saved for later use”, and will be executed later, when it is invoked (called).

```js
function bar() {
 return 3;
}
```

A **function expression** can be stored in a variable:

```js
var x = function (a, b) {return a * b};
```

After a function expression has been stored in a variable, the variable can be used as a function. Functions stored in variables do not need function names. However, a name *can* be provided.

<br>

They more-or-less do the same example thing, but there’s one subtle yet important difference between them i.e Hoisting. **Function declarations are hoisted but function expressions are not.**

All of the functions written with function declarations are “known” before any code is run. This allows you to call a function before you declare. *Function expressions*, however, do **not** hoist. If you try to run a function before you’ve expressed it, you’ll get an error.

```js
substract(7, 4); // returns Uncaught TypeError: subtract is not a function
var subtract = function (num1, num2) {
	return num1 - num2;
};
```

> In short, use function declarations when you want to create a function on the global scope and make it available throughout your code. Use function expressions to limit where the function is available, keep your global scope light, and maintain clean syntax. Instead of your program being aware of many different functions, when you keep them anonymous, they are used and forgotten immediately.



## parameters and arguments

- *Parameters* are variables listed as a part of the function definition.
- *Arguments* are values passed to the function when it is invoked.



```js
function argumentVar(parameter1, parameter2, parameter3){
  console.log(arguments.length); // Logs the number of arguments passed.
  console.log(arguments[3]); // Logs the 4th argument. 
  console.log(parameter2);
}

argumentVar(1,2,3,4,5); // 12345 are arguments
// 5
// 4
// 2
```
The `arguments` parameter is implicitly passed just like the `this` parameter. It is a local variable accessible within all functions and contains an entry for each argument passed to that function. The `arguments `object is an array-like construct which can be used to access arguments passed to the function even if a matching parameter isn’t explicitly defined.

> One interesting feature of the `arguments` object is that it aliases function parameters in non strict mode. What this means is that changing `arguments[0]` would change `parameter1`.



### Rest Parameter

Rest Parameter is an ES6 addition to JavaScript. To create a rest parameter prefix the last parameter in a function definition with ellipsis(…)

```js
function restParam(...restArgs){
  console.log(restArgs.length); // Logs the number of arguments passed
  console.log(restArgs[3]); // Logs the 4th argument
}

restParam(1,2,3,4,5);
// 5
// 4
```

In the code snippet above `…restArgs` creates an array `restArgs` that holds all the arguments passed to the function in an array. The rest parameter has to be the last parameter otherwise it throws a `Syntax error : parameter after rest parameter`.



Two major differences between the `arguments` object and `rest parameters`:

- Rest Parameters is a real array and methods like forEach and sort can be applied. Even though the arguments object has the length method, it is not a real array and using array methods like sort would only bring us misery and sorrow.
- Rest Parameters contain only the arguments that have no corresponding parameter while arguments object contains all the arguments passed to the function.

Code snippet #3 could also be written as follows.

```js
function restParam(parameter1, ...restArgs){
  console.log(restArgs.length); // Logs the number of arguments that do not have a corresponding parameter
  console.log(restArgs[2]); // Logs the 3rd argument after the number of arguments that do not have a corresponding parameter
}

restParam(1,2,3,4,5);
// 4
// 4
```



### Default Parameters

In JavaScript, a **parameter has a default value of undefined**. It means that if you don’t pass the arguments into the function, its parameters will have the default values of undefined.

```javascript
function say(message) {
    console.log(message);
}
say(); // undefined
```

If you want to give the `message` parameter a default value. A typical way for achieving this is to test parameter value and assign a default value if it is `undefined`:

```javascript
function say(message) {
    message = typeof message !== 'undefined' ? message : 'Hi';
    console.log(message);
}
say(); // 'Hi'
```

ES6 provides an easier way to set the default values for the parameters of a function as shown in the following syntax:

```js
function fn(param1=default1,param2=default2,..) {
}

function say(message='Hi') {
    console.log(message);
}
say(); // 'Hi'
say(undefined); // 'Hi'
say('Hello'); // 'Hello'
```

You can use a return value of a function as a default value for a parameter.

```js
let taxRate = () => 0.1;
let getPrice = function( price, tax = price * taxRate() ) {
    return price + tax;
}
let fullPrice = getPrice(100);
console.log(fullPrice); // 110
```

> Function expressions are convenient when passing a function as an argument to another function.
>

## return statement

The return statement ends function execution and specifies a value to be returned to the function caller.

```js
return [[expression]]; 
```

The expression whose value is to be returned. If omitted, `undefined` is returned instead.

The return statement is affected by automatic semicolon insertion (ASI). No line terminator is allowed between the return keyword and the expression.

```js
return
a + b;
```

is transformed by ASI into:

```js
return; 
a + b;
```

To avoid this problem (to prevent ASI), you could use parentheses:

```js
return (
  a + b
);
```

> function() itself returns last evaluated value even without including return. But it's a good practice to use return statement.

## Arrow functions

pure functions

higher order functions

this,closure,recursion



## Video

[![Functions ](https://img.youtube.com/vi/gigtS_5KOqo/0.jpg)](https://www.youtube.com/watch?v=gigtS_5KOqo)



JavaScript Function - What's your Function? | fireship.io

