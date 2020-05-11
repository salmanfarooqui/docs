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



## IIFE

IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined.

```javascript
function generateMagicNumber() {
    return Math.floor(Math.random() * 100) + 900;
}
console.log("magic no: " + generateMagicNumber());
var favoriteNumber = 5;
```

If we load other JavaScript codes in our browser, they also gain access to generateMagicNumber() and favoriteNumber. To prevent them from using or editing them

```javascript
(function () {
    function generateMagicNumber() {
        return Math.floor(Math.random() * 100) + 900;
    }
    console.log("magic no: " + generateMagicNumber());
    var favoriteNumber = 5;
})();
```

It runs the same, but now generateMagicNumber() and favoriteNumber are only accessible within. It won't create any global variable and **your global namespace will not be polluted**. 


### Asynchronous Functions in Loops

JavaScript's behavior surprises many when callbacks are executed in loops. For example, let's count from 1 to 5 in JavaScript, putting a 1 second gap between every time we log a message. A naïve implementation would be -

```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function () {
        console.log('I reached step ' + i);
    }, 1000 * i);
}
// I reached step 6
// I reached step 6
// I reached step 6
// I reached step 6
// I reached step 6
```

While the output would be printed 1 second after the other, each line prints that they reached step 6. Why?

When JavaScript encounters asynchronous code, it defers execution of the callback until the asynchronous task completes. In this example, the `console.log()` statement will run only after the timeout has elapsed.

JavaScript also created a **closure** for our callback.  With closures, our callback can access the variable `i` even though the `for` loop has already finished executing. However, our callback only has access to the value of `i` **at the time of its execution**. As the code within the `setTimeout()` function were all deferred, the `for` loop was finished with `i` being equal to 6.



*This problem can be solved with an IIFE -* 

```javascript
for (var i = 1; i <= 5; i++) {
    (function (step) {
        setTimeout(function() {
            console.log('I reached step ' + step);
        }, 1000 * i);
    })(i);
}
// I reached step 1
// I reached step 2
// I reached step 3
// I reached step 4
// I reached step 5
```

By using an IIFE, we create a new scope for our callback function. Our IIFE takes a parameter `step`. Every time our IIFE is called, we give it the current value of `i` as its argument. Now, when the callback is ready to be executed, its closure will have the correct value of `step`.



> If you have 2 libraries that export an object with the same name, you can use IIFEs to ensure they don't conflict in your code. For example, the [jQuery](https://jquery.com/) and [Cash](https://kenwheeler.github.io/cash/) JavaScript libraries both export `$` as their main object. Let's say, we want to ensure that `$` refers to the `jQuery` object, and not the `cash` alternative. 
>
> ```javascript
> (function($) {
>     // Code that runs in your function
> })(jQuery);
> ```



## classes

Classes are in fact "special functions", and just as you can define function expressions and function declarations, the class syntax has two components: class expressions and class declarations.

### Class declaration

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

An important difference between function declarations and class declarations is that **function declarations are hoisted and class declarations are not**. You first need to declare your class and then access it, otherwise code like the following will throw a ReferenceError:

```js
const p = new Rectangle(); // ReferenceError
class Rectangle {}
```



### Class expression

Class expressions can be named or unnamed. The name given to a named class expression is local to the class's body. (it can be retrieved through the class's (not an instance's) [`name`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name) property, though).

```js
// unnamed
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle"

// named
let Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle2"
```



### Constructor

The `constructor` method is a special method **for creating and initializing** an object created with a `class`. In other words, the properties are created and initialized inside a `constructor()` method. There can only be one special method with the name "constructor" in a class. The constructor method is called each time the class object is initialized. A constructor can use the `super` keyword to call the constructor of the super class.



### Extends

The `extends` keyword is used in *class declarations* or *class expressions* to create a class as a child of another class.

```js
class Animal { 
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }
  speak() {
    console.log(`${this.name} barks.`);
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```

If there is a constructor present in the subclass, it needs to **first call super() before using "this"**.



<br>

<br>

Arrow functions

pure functions

higher order functions

this,closure,recursion



## Video

[![Functions ](https://img.youtube.com/vi/gigtS_5KOqo/0.jpg)](https://www.youtube.com/watch?v=gigtS_5KOqo)



JavaScript Function - What's your Function? | fireship.io

