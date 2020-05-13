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



### Optional Arguments

The following code is allowed and executes without any problem.

```js
function square(x) { return x * x; }
console.log(square(4, true, "hedgehog"));
// → 16
```

We defined `square` with only one parameter. Yet when we call it with three, the language doesn’t complain. It ignores the extra arguments and computes the square of the first one.

If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters get assigned the value `undefined`.



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



## Classes

Classes are "special functions". 

```javascript
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// proof: User is a function
alert(typeof User); // function
```

What `class User {...}` construct really does is:

1. Creates a function named `User`, that becomes the result of the class declaration. The function code is taken from the `constructor` method (assumed empty if we don’t write such method).
2. Stores class methods, such as `sayHi`, in `User.prototype`.

After `new User` object is created, when we call its method, it’s taken from the prototype. So the object has access to class methods. We can illustrate the result of `class User` declaration as -

![class01](https://docs.salmanfarooqui.com/JS/images/class01.png)

```javascript
alert(User === User.prototype.constructor); // true
alert(User.prototype.sayHi); // alert(this.name);
```

Sometimes `class` is called “syntactic sugar” (syntax that is designed to make things easier to read, but doesn’t introduce anything new), because we could actually declare the same without `class` keyword at all:

```javascript
// rewriting class User in pure functions

// 1. Create constructor function
function User(name) {
  this.name = name;
}
// a function prototype has "constructor" property by default,
// so we don't need to create it

// 2. Add the method to prototype
User.prototype.sayHi = function() {
  alert(this.name);
};

// Usage:
let user = new User("John");
user.sayHi();
```

The result of this definition is about the same. Still there are many difference like -

- Class methods are non-enumerable. A class definition sets `enumerable` flag to `false` for all methods in the `"prototype"`. That’s good, because if we `for..in` over an object, we usually don’t want its class methods.
- Classes always `use strict`. All code inside the class construct is automatically in strict mode.
- Class declarations are not `hoisted` which means, you can’t use a class before it is declared, it will return `not defined` error.  

```js
const p = new Rectangle(); // ReferenceError
class Rectangle {}
```

- Classes doesn’t allow the property value assignments like constructor functions or object literals. You can only have functions or getters / setters. So no `property:value` assignments directly in the class. We can use [class fields](/JS/functions#class-fields).

<br>

Just as you can define function expressions and function declarations, the class syntax has two components: class expressions and class declarations.

### Class declaration

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
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

That’s the place where you set the initial values for the fields, or do any kind of object setup. Inside the constructor `this` value equals to the newly created instance. The arguments used to instantiate the class become the parameters of the constructor.

```javascript
class User {
  constructor(name) {
    name; // => 'Jon Snow'    
    this.name = name;
  }
}

const user = new User('Jon Snow');
```

If you don’t define a constructor for the class, a default one is created. The default constructor is an empty function, which doesn’t modify the instance.

The `constructor` method is a special method **for creating and initializing** an object created with a `class`. The constructor method is called each time the class object is initialized. A constructor can use the `super` keyword to call the constructor of the super class.

**`constructor` requires `new` keyword to work.** It means constructor will only be called when we do following.

```js
let car = new Vehicle("Toyota", "Corolla", "Black");
```



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



### Class Fields

Previously, our classes only had methods. “Class fields” is a syntax that allows to add any properties. For instance, let’s add `name` property to `class User`

```javascript
class User {
  name = "John";
  sayHi() {
    alert(`Hello, ${this.name}!`);
  }
}

new User().sayHi(); // Hello, John!
```

The important difference of class fields is that they are set on individual objects, not `User.prototype`

```javascript
class User {
  name = "John";
}

let user = new User();
alert(user.name); // John
alert(User.prototype.name); // undefined
```

Technically, they are processed after the constructor has done it’s job.  Class fields are a recent addition to the language so older browsers may need a polyfill.

<br>

A good way to hide internal data of an object is to use the private fields. Prefix the field name with the special symbol `#` to make it private. 

```javascript
class User {
  #name;
  constructor(name) {
    this.#name = name;
  }
  getName() {
    return this.#name;
  }
}

const user = new User('Jon Snow');
user.getName(); // => 'Jon Snow'
user.#name;     // SyntaxError is thrown
```

`#name` is a private field. You can access and modify `#name` within the body of the `User`. The method `getName()` can access the private field `#name`. But if you try to access the private field `#name` outside of `User` class body, a syntax error is thrown.



### this

functions in JavaScript have a dynamic `this`. It depends on the context of the call. So if an object method is passed around and called in another context, `this` won’t be a reference to its object any more.

```javascript
class Button {
  constructor(value) {
    this.value = value;
  }
  click() {
    alert(this.value);
  }
}

let button = new Button("hello");
setTimeout(button.click, 1000); // undefined
```

The problem is called "losing `this`".

There are two approaches to fixing it -

1. Pass a wrapper-function, such as `setTimeout(() => button.click(), 1000)`.
2. Bind the method to object, e.g. in the constructor.

```javascript
class Button {
  constructor(value) {
    this.value = value;
    this.click = this.click.bind(this);
  }
  click() {
    alert(this.value);
  }
}

let button = new Button("hello");
setTimeout(button.click, 1000); // hello
```

Class fields provide a more elegant syntax for the latter solution.

```javascript
class Button {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    alert(this.value);
  }
}

let button = new Button("hello");
setTimeout(button.click, 1000); // hello
```

The class field `click = () => {...}` creates an independent function on each `Button` object, with `this` bound to the object. Then we can pass `button.click` around anywhere, and it will be called with the right `this`.



### Explaination

Let's say we have constructor function like this -

```js
function Vehicle(make, model, color) {
        this.make = make,
        this.model = model,
        this.color = color,
        this.getName = function () {
            return this.make + " " + this.model;
        }
}

// can create as many objects of type Vehicle as we want by adding 1 line of code
let car = new Vehicle("Toyota", "Corolla", "Black");
let car2 = new Vehicle("Honda", "Civic", "White");
```

There are a few problems with this technique.

- When we write `new Vehicle()`, what Javascript engine does under the hood is that it makes a copy of our `Vehicle` constructor function for each of our objects, each and every property and method is copied to the new instance of the `Vehicle`. The problem is that we don’t want the member functions (methods) of our constructor function to be repeated in every object. That is redundant.

- Another problem is that we can’t add a new property or method to an existing object like this:

 ```js
  car2.year = "2012"
 ```

 To add this year property you will have to add it to the constructor function itself:

 ```js
  function Vehicle(make, model, color, year) {
          this.make = make,
          this.model = model,
          this.color = color,
          this.year = year,
          this.getName = function () {
              return this.make + " " + this.model;
          }
  }
 ```

<br>

Whenever a new function is created in javascript, Javascript engine by default adds a `prototype` property to it, this property is an object and we call it “prototype object”. By default this prototype object has a constructor property which points back to our function, and another property `__proto__` which is an object.

```js
console.log(Vehicle.prototype);

// {
// constructor: ƒ Vehicle(make, model, color)
// __proto__: Object
// }

// Now, this prototype object can be used to add new properties and methods to the constructor function using the following syntax and they will be available to all the instances of the constructor function

car.prototype.year = "2016";
```

Whenever a new instance of the constructor function is created this property is also copied to the instance along with other properties and methods.

- Prototype is cool, but there are a few things you need to be careful while using prototype approach. Prototype properties and methods are shared between all the instances of the constructor function, but when any one of the instances of a constructor function makes any change in any primitive property, it will only be reflected in that instance and not among all the instances.
- Another thing is that reference type properties are always shared among all the instances, for example, a property of type array, if modified by one instance of the constructor function will be modified for all the instances

<br>

We understand the constructor functions and prototype, now its easy to understand the class, why? because javascript classes are nothing but just a new way of writing the constructor functions by utilizing the power of prototype. The class becomes useful when you create an *instance* of the class. An instance is an object containing data and behavior described by the class.

```js
class Vehicle {
    constructor(make, model, color) {
        this.make = make;
        this.model = model;
        this.color = color;
    }

    getName() {
        return this.make + " " + this.model;
    }
}
```

And in order to create a new instance of class `Vehicle`, we do this:

```js
let car = new Vehicle("Toyota", "Corolla", "Black");
```
By writing the above code, we have actually created a variable named `Vehicle` which references to function constructor defined in the class, also we have added a method to the prototype of the variable `Vehicle`, same as bellow:

```js
function Vehicle(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
}

Vehicle.prototype.getName()= function () {
    return this.make + " " + this.model;
}

let car = new Vehicle("Toyota", "Corolla", "Black");
```

So, this proves that class is a new way of doing the constructor functions.



## Recursion

It is perfectly okay for a function to call itself, as long as it doesn’t do it so often that it overflows the stack. **A function that calls itself** is called *recursive*.

```js
function factorial(x) {
  if (x < 0) return;
  if (x === 0) return 1;
  return x * factorial(x - 1);
}
factorial(3);
// 6
```

All recursive functions should have two key features -

1. **A Termination Condition** - Simply put, `if(something bad happened){ STOP };` The Termination Condition is our recursion fail-safe. Think of it like your emergency brake. It’s put there in case of bad input to prevent the recursion from ever running.
2. **A Base Case** - Simply put, `if(this happens) { Yay! We're done };` The Base Case is similar to our termination condition in that it also stops our recursion. But remember, the termination condition is a catch-all for bad data. Whereas the base case is *the goal* of our recursive function. In the factorial example, `if (x === 0) return 1;` is our base case.

```js
function factorial(x) {
  // TERMINATION
  if (x < 0) return;
    
  // BASE
  if (x === 0) return 1;
    
  // RECURSION
  return x * factorial(x - 1);
}
```

Unlike loops, the recursive version doesn't need extra variables to track its progress. Each call to `factorial` adds to the stack, feeding it `x - 1` (updated parameters).



> Recursive functions let you perform a unit of work multiple times. This is exactly what `for/while` loops let us accomplish! Sometimes, however, recursive solutions are a more elegant approach to solving a problem. Running through a simple loop is generally cheaper than calling a function multiple times.



## Currying

Currying is a way of constructing functions that allows partial application of a function’s arguments. What this means is that you can pass a subset of those arguments and get a function back that’s waiting for the rest of the arguments. 

The curried effect is achieved by binding some of the arguments to the first function invoke, so that those values are fixed for the next invocation. 

Currying is when you break down a function that takes multiple arguments into a series of functions that take part of the arguments.  This allows functions of several arguments to have some of their initial arguments partially applied. 



Here's an example in JavaScript:

```js
function add (a, b) {
  return a + b;
}

add(3, 4); // returns 7
```

This is a function that takes two arguments, a and b, and returns their sum. We will now curry this function:

```js
function add (a) {
  return function (b) {
    return a + b;
  }
}
```

This is a function that takes one argument, a, and returns a function that takes another argument, b, and that function returns their sum.

```js
add(3)(4);
var add3 = add(3);
add3(4);
```

The first statement returns 7, like the add(3, 4) statement. The second statement defines a new function called add3 that will add 3 to its argument. This is what some people may call a closure. The third statement uses the add3 operation to add 3 to 4, again producing 7 as a result.

<br>

### Example -

Simple greeting in js -

```js
var greet = function(greeting, name) {
  console.log(greeting + ", " + name);
};
greet("Hello", "Heidi"); //"Hello, Heidi"
```

Using currying -

```js
var greetCurried = function(greeting) {
  return function(name) {
    console.log(greeting + ", " + name);
  };
};
```

This tiny adjustment to the way we wrote the function lets us create a new function for any type of greeting, and pass that new function the name of the person that we want to greet:

```js
var greetHello = greetCurried("Hello");
greetHello("Heidi"); //"Hello, Heidi"
greetHello("Eddie"); //"Hello, Eddie"
```

We can also call the original curried function directly, just by passing each of the parameters in a separate set of parentheses, one right after the other:
`greetCurried("Hi there")("Howard"); //"Hi there, Howard"`

<br>

Now that we have learned how to modify our traditional function to use this approach for dealing with arguments, we can do this with as many arguments as we want:

```js
var greetDeeplyCurried = function(greeting) {
  return function(separator) {
    return function(emphasis) {
      return function(name) {
        console.log(greeting + separator + name + emphasis);
      };
    };
  };
};
```

No matter how far the nesting goes, we can create new custom functions to greet as many people as we choose in as many ways as suits our purposes:

```js
var greetAwkwardly = greetDeeplyCurried("Hello")("...")("?");
greetAwkwardly("Heidi"); //"Hello...Heidi?"
greetAwkwardly("Eddie"); //"Hello...Eddie?"
```

What’s more, we can pass as many parameters as we like when creating custom variations on our original curried function, creating new functions that are able to take the appropriate number of additional parameters, each passed separately in its own set of parentheses:

```js
var sayHello = greetDeeplyCurried("Hello")(", ");
sayHello(".")("Heidi"); //"Hello, Heidi."
sayHello(".")("Eddie"); //"Hello, Eddie."
```


<br>

<br>



Arrow functions

pure functions

higher order functions



## Video

[![Functions ](https://img.youtube.com/vi/gigtS_5KOqo/0.jpg)](https://www.youtube.com/watch?v=gigtS_5KOqo)



JavaScript Function - What's your Function? | fireship.io

