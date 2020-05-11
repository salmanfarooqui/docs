# Notes

## JS Arguments

### Arguments are Passed by Value

The parameters, in a function call, are the function's arguments.
JavaScript arguments are passed by value: The function only gets to know the values, not the argument's locations.
If a function changes an argument's value, it does not change the parameter's original value.
Changes to arguments are not visible (reflected) outside the function.

### Objects are Passed by Reference

In JavaScript, object references are values.
Because of this, objects will behave like they are passed by reference:
If a function changes an object property, it changes the original value.
Changes to object properties are visible (reflected) outside the function.



## Coercion

Converting a value from one type to another by JS engine automatically.

```js
var a = 1 + 2;  // 3
var a = 'hello ' + 'world';  // hello world

// But if we pass two different types to addition operator. 
var a = 1 + '2';  // 12
```

Here first value was coerced by JavaScript into a string.



**Example -**

```js
console.log(3 < 2 < 1);
// true
```

why is this true? 

first 3 < 2 runs and resolves to false. Then, false < 1 runs. Because they are two different types, coercion happens and false gets converted to 0, so it runs 0 < 1, which is true.

> == operator coerce the types. so `3 == 3 // true` and `"3" == 3 // true`. If you don't want that use strict equality operator ===

<br>

```js
Boolean(undefined); // false
Boolean(null); //false
Boolean(""); //false
Boolean(0); //false
```



## Diff between JSON and Object Literal

Object literal may or may not have quotes around keys. Example -

```js
var something = {
name = "Salman"
}
```

or 

```js
var something = {
"name" = "Salman"
}
```

But JSON must have quotes around keys. For Example -

```js
var something = {
"name" = "Salman"
}
```

We can convert them to each other easily. To convert Object literal to Json use, JSON.stringify() and to convert JSON to object literal use JSON.parse() 



## Passed as Value vs passed as referance

All primitive types are passed as values while all objects are passed as referance.
Example value -

```js
var a = 3;
var b;
b = a; // b copies a but creates it's own space, so it's not affected by changed value of a
a= 2; // changed a but doesn't effect b
console.log(a); // 2
console.log(b); // 3, b is a copy of a but has it's own space in memory
```

Example referance - 

```js
var c = { greeting: 'hi'};
var d;
d=c;
c.greeting = 'hello';
console.log(c);    // { greeting: 'hello'};
console.log(d);   // { greeting: 'hello'}; both point to same space in memory
```

but remember that if we reassign a value, then it will create it's own space. Like,

```js
c = { greeting: 'howdy'}; 
```



## Using self for 'this'

```js
function a() {
console.log(this);
}
var b = function() {
console.log (this);
}
a(); // window (global object)
b(); // window
```

Now,

```js
var c = {
name: 'Salman',
log: function() {
console.log(this);
} 
}
c.log(); //  Object {name: "Salman", log: function}
```

##### In object methods

```js
var c = {
name: 'Salman',
log: function() {
     console.log(this); // 'this' here will point to c object, like we expect
             var setname = function(newValue) {
              this.name = newValue; // 'this' here will point to window and not c object
              }
      setname('Salman Farooqui');
      console.log(this); // Salman, value not updated
      } 
}
```

To fix this, we can use self,

```js
var c = {
name: 'Salman',
log: function() {
      var self = this; // since 'this' is object, it will be set by referance (self and 'this' will be at same location in memory)
     console.log(self); // self here will point to c object
              var setname = function(newValue) {
              self.name = newValue; // self here will also point to c object
              }
      setname('Salman Farooqui');
      console.log(self); // Salman Farooqui, value will get updated
      } 
}
```



## Closures

Closures are one of the most powerful features of JavaScript. JavaScript allows for the nesting of functions and grants the inner function full access to all the variables and functions defined inside the outer function (and all other variables and functions that the outer function has access to). However, the outer function does not have access to the variables and functions defined inside the inner function. This provides a sort of security for the variables of the inner function. Also, since the inner function has access to the scope of the outer function, the variables and functions defined in the outer function will live longer than the duration of the outer function execution, if the inner function manages to survive beyond the life of the outer function. A closure is created when the inner function is somehow made available to any scope outside the outer function.

#### What is a Closure?

A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.
To use a closure, simply define a function inside another function and expose it. To expose a function, return it or pass it to another function.
The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

The functions actual return is the true 'moment of closure' (closure occurs when the inner function returns), that is when the enviornment and all necessary variables are packaged up. Placement of return is very important.



#### Example -

```js
function assign(name, passArray) {
var torp;
for (var i=0; i<passArray.length; i++) {
if(passArray[i] == name) {
torp = function () {
alert("Ahoy" + name + "man your post at Torpedo #" + (i+1) + "!");
};
}
}
return torp;
}

var subPassengers = ["Luke", "leia", "Han", "Chewie", "Yoda", "haha", "boba", "doba", "soba"];
var giveAssignment = assign("Chewie", subPassengers);
giveAssignments();
//returns 9 instead of 4
// because by the time we returned torp, loop has already been finished with i becoming 8, i+1 becomes 9, so closure was created with that specific enviornment.
```

To fix, we can remove `var torp;` on line 2 and 

`replace torp = function () {` 

with 

`return function () {` 

// now we will get 4

// we can also do this - function assign(passArray) {

// return function (name) {

// for (var i=0; ....



## Higher Order Components

### Pure Functions

A function is considered pure if it adheres to the following properties:

1.	All the data it deals with are declared as arguments
2.	It does not mutate data it was given or any other data (these are often referred to as side effects).
3.	Given the same input, it will always return the same output.

For example, the add function below is pure:

```js
function add(x, y) {
  return x + y;
}
```


However, the function badAdd below is impure:

```js
var y = 2;
function badAdd(x) {  
  return x + y;
}
```


This function is not pure because it references data that it hasn’t directly been given. As a result, it’s possible to call this function with the same input and get different output:

```js
var y = 2;
badAdd(3) // 5
y = 3;
badAdd(3) // 6
```


A pure function is a function which:
1.	Given the same input, will always return the same output.
2.	Produces no side effects.
3.	Relies on no external mutable state.

```js
const double = x => x * 2
console.log( double(5) ); // 10
```

`double(5)` will always mean the same thing as `10` in your program, regardless of context, no matter how many times you call it or when.
But you can’t say the same thing about all functions. Some functions rely on information other than the arguments you pass in to produce results.
Consider this example:

```js
Math.random(); // => 0.4011148700956255
Math.random(); // => 0.8533405303023756
Math.random(); // => 0.3550692005082965
```

Even though we didn’t pass any arguments into any of the function calls, they all produced different output, meaning that `Math.random()` is not pure.

A pure function produces no side effects, which means that it can’t alter any external state.

<br>

### Immutability

JavaScript’s object arguments are references, which means that if a function were to mutate a property on an object or array parameter, that would mutate state that is accessible outside the function. Pure functions must not mutate external state.
Consider this mutating, impure `addToCart()` function:

```js
// impure addToCart mutates existing cart
const addToCart = (cart, item, quantity) => {
  cart.items.push({
    item,
    quantity
  });
  return cart;
};
test('addToCart()', assert => {
  const msg = 'addToCart() should add a new item to the cart.';
  const originalCart =     {
    items: []
  };
  const cart = addToCart(
    originalCart,
    {
      name: "Digital SLR Camera",
      price: '1495'
    },
    1
  );
  const expected = 1; // num items in cart
  const actual = cart.items.length;
  assert.equal(actual, expected, msg);
  assert.deepEqual(originalCart, cart, 'mutates original cart.');
  assert.end();
});
```

It works by passing in a cart, and item to add to that cart, and an item quantity. The function then returns the same cart, with the item added to it.

The problem with this is that we’ve just mutated some shared state. Other functions may be relying on that cart object state to be what it was before the function was called, and now that we’ve mutated that shared state, we have to worry about what impact it will have on the program logic if we change the order in which functions have been called. 

Now consider this version:

```js
// Pure addToCart() returns a new cart
// It does not mutate the original.
const addToCart = (cart, item, quantity) => {
  const newCart = lodash.cloneDeep(cart);

  newCart.items.push({
    item,
    quantity
  });
  return newCart;
};

test('addToCart()', assert => {
  const msg = 'addToCart() should add a new item to the cart.';
  const originalCart = {
    items: []
  };

  // deep-freeze on npm
  // throws an error if original is mutated
  deepFreeze(originalCart);
  const cart = addToCart(
    originalCart,
    {
      name: "Digital SLR Camera",
      price: '1495'
    },
    1
  );

  const expected = 1; // num items in cart
  const actual = cart.items.length;
  assert.equal(actual, expected, msg);
  assert.notDeepEqual(originalCart, cart,
    'should not mutate original cart.');
  assert.end();
});
```

In this example, we have an array nested in an object, which is why I reached for a deep clone.



## Higher Order Functions

A higher order function is a function that when called, returns another function.  Often they also take a function as an argument, but this is not required for a function to be considered higher order.

```js
function logAndReturn(func) {  
  return function() {  
    var args = Array.prototype.slice.call(arguments);
    var result = func.apply(null, args);
    console.log('Result', result);
    return result;
  }
}
```

Now we can take this function and use it to add logging to add and subtract:

```js
var addAndLog = logAndReturn(add);
addAndLog(4, 4) // 8 is returned, ‘Result 8’ is logged
```

```js
var subtractAndLog = logAndReturn(subtract);
subtractAndLog(4, 3) // 1 is returned, ‘Result 1’ is logged;
```

logAndReturn is a HOF because it takes a function as its argument and returns a new function that we can call.



## Functional, Stateless Components

React 0.14 introduced support for functional, stateless components. These are components that have the following characteristics:

- They do not have any state.
- They do not use any React lifecycle methods (such as componentWillMount()).
- They only define the render method and nothing more.

When a component adheres to the above, we can define it as a function, rather than using class App extends React.Component. For example, the two expressions below both produce the same component:

```js
var App = React.createClass({
  render: function() {
    return <p>My name is { this.props.name }</p>;
  }
});

var App = function(props) {
  return <p>My name is { props.name }</p>;
}
```

In the functional, stateless component instead of referring to this.props we’re instead passed props as an argument. 



## setState is Asynchronous

I didn’t notice this when I started out with React. The setState function is asynchronous!

If you call setState and immediately inspect this.state, chances are it will not be updated yet.

If you need to set the state and immediately act on that change, you can pass in a callback function like this:

```js
this.setState({name: 'Joe'}, function() {
	// called after state has been updated
	// and the component has been re-rendered
});
```

Another alternative is to use the componentWillUpdate or componentDidUpdate lifecycle hooks, which will be called immediately before and after rendering in response to your state change. They’ll also get called whenever props change – so if you absolutely want to respond only to state changes, use the callback approach.



## React’s Synthetic Events

Inside React event handlers, the event object is wrapped in a SyntheticEvent object. These objects are pooled, which means that the objects received at an event handler will be reused for other events to increase performance. This also means that accessing the event object’s properties asynchronously will be impossible since the event’s properties have been reset due to reuse.

The following piece of code will log null because event has been reused inside the SyntheticEvent pool:

```js
handleClick(event) {
  setTimeout(function() {
    console.log(event.target.name);
  }, 1000);
}
```

To avoid this you need to store the event’s property you are interested in inside its own binding:

```js
handleClick(event) {
  let name = event.target.name;
  setTimeout(function() {
    console.log(name);
  }, 1000);
}
```



## Always look at your bundle size

One tip to making your bundle way smaller is to import directly from the node module root path.
Do this:
import Foo from ‘foo/Foo’
instead of:
Import {Foo} from ‘foo’



## The JSX spread

JSX, the HTML-like syntax we use to define React elements, supports the spread operator for passing an object to a component as properties. For example, the code samples below achieve the same thing:

```js
var props = { a: 1, b: 2};

<Foo a={props.a} b={props.b} />
<Foo {...props} />
```

Using {...props} spreads each key in the object and passes it to Foo as an individual property.

#### How it Works

Let’s imagine in your application you have an object that contains information on the current user who is authenticated on the system. You need some of your React components to be able to access this information, but rather than blindly making it accessible for every component you want to be more strict about which components receive the information.
The way to solve this is to create a function that we can call with a React component. The function will then return a new React component that will render the given component but with an extra property which will give it access to the user information.

That sounds pretty complicated, but it’s made more straightforward with some code:

```js
function wrapWithUser(Component) {
  // information that we don’t want everything to access
  var secretUserInfo = {
    name: 'Jack Franklin',
    favouriteColour: 'blue'
  };
  // return a newly generated React component
  // using a functional, stateless component
  return function(props) {
    // pass in the user variable as a property, along with
    // all the other props that we might be given
    return <Component user={secretUserInfo} {...props} />
  }
}
```

The function takes a React component (which is easy to spot given React components have to have capital letters at the beginning) and returns a new function that will render the component it was given with an extra property of user, which is set to the secretUserInfo.

Now let’s take a component, <AppHeader>, which wants access to this information so it can display the logged in user:

```js
var AppHeader = function(props) {
  if (props.user) {
    return <p>Logged in as {props.user.name}</p>;
  } else {
    return <p>You need to login</p>;
  }
}
```

The final step is to connect this component up so it is given this.props.user. We can create a new component by passing this one into our wrapWithUser function.

```js
var ConnectedAppHeader = wrapWithUser(AppHeader);
```

We now have a <ConnectedAppHeader> component that can be rendered, and will have access to the user object.
I chose to call the component ConnectedAppHeader because I think of it as being connected with some extra piece of data that not every component is given access to.



## Why are HOCs Useful

I think two attributes of HOCs make them useful.

Firstly, given an HOC wraps another component, it can:
1.	Do things before and/or after it calls that component
2.	Avoid rendering the component if certain criteria is not met
3.	Update the props passed to that component, or add new props
4.	Transform the output of rendering a component (e.g. wrap with extra DOM elements, etc.)



Secondly, because HOCs can be applied to any component, functionality can be implemented once and re-used for every component that needs it.



#### Examples

Let's assume, there is a module called featureToggles which can check if a specific toggle is on or off for the current user/request:

```js
featureToggles.js
module.exports.isOn = function(toggleName) {
   // implementation excluded
   // returns true or false
}
```

Then, a toggledOn HOC an be implemented as:

```js
const toggledOn = (toggleName, ComposedComponent) => React.createClass({
  render() {
    return featureToggles.isOn(toggleName) ? <ComposedComponent {...this.props} /> : null;
  }
});
```

As you can see, this HOC simply accepts a toggle name and a component, and only renders that component if the toggle is turned on. So it can be simply used to hide a component on the page by default and to only show it if the toggle is on for the current user/request:



## Arrow Functions 

```js
var salman = function(name, age) {
console.log('Haha');
};
```

is same as writing like this with arrow functions

```js
var salman = (name,age) => console.log('Haha');
```

if there is only 1 arguement, we can remove the parantheses around arguements. Like , `var salman = name => console.log('Haha');`

1) Function keyword gets removed all the time.

2) Paranthesis around function arguement gets removed if there is only 1 arguement. And curly braces are not used after it.

3) Curly braces can be removed if it's returning only single value. 

Like, `var salman = (name,age) => console.log('Haha');`

If you omit the braces, the arrow function has a concise body, which consists solely of a single expression whose result will implicitly become the return value of the function.
However, when using multiple lines of code, you need to add return keyword to return something.

arrow functions don't have their own `this` value so they're handy when you want to preserve the `this` value from an outer method definition.

---------------------------------------------------------------------------------

We can use parantheses for JSX. Like this,

```js
const foo = (params) => (
    <span>
		<p>Content</p>
 	</span>
);
```

In JSX, it looks like multiple "lines" but really just gets compiled to a single "element."



In JSX, it looks like multiple "lines" but really just gets compiled to a single "element."

Note that if we execute code inside parantheses, it will throw error.

```js
Like, var salman = (name,age) => (
console.log('Haha');
);
//Error because console.log is executing with a semi colon.
```

So use parantheses only for JSX.

------------------------------------------------------------------------------------------------------------------------------------------

For executing multiple lines of code, use curly braces.

#### Example -

```js
var salman = (name,age) => 
console.log('Hahax');
console.log('pto');
```

will compile to 

```js
var salman = function salman(name, age) {
  return console.log('Hahax');
};
console.log('pto');
```

but this

```js
var salman = (name,age) => {
console.log('Hahax');
console.log('pto');
}
```

will correctly compile to

```js
var salman = function salman(name, age) {
  console.log('Hahax');
  console.log('pto');
};
```

So, for multiple line of code execution, don't remove the curly braces.



--------------------------------------------------------



**Arrow functions automatically bind the 'this' keyword.**

Since arrow function has a short syntax, it's inviting to use it for a method definition. Let's take a try:

```js
var calculate = {  
  array: [1, 2, 3],
  sum: () => {
    console.log(this === window); // => true
    return this.array.reduce((result, item) => result + item);
  }
};
console.log(this === window); // => true  
// Throws "TypeError: Cannot read property 'reduce' of undefined"
calculate.sum();  
```

calculate.sum method is defined with an arrow function. But on invocation calculate.sum() throws a TypeError, because this.array is evaluated to undefined. 

When invoking the method sum() on the calculate object, the context still remains window. It happens because the arrow function binds the context lexically with the window object. 

Executing this.array is equivalent to window.array, which is undefined.

The solution is to use a function expression or shorthand syntax for method definition (available in ECMAScript 6). In such case this is determined by the invocation, but not by the enclosing context. Let's see the fixed version:

```js
var calculate = {  
  array: [1, 2, 3],
  sum() {
    console.log(this === calculate); // => true
    return this.array.reduce((result, item) => result + item);
  }
};
calculate.sum(); // => 6  


```

Because sum is a regular function, this on invocation of calculate.sum() is the calculate object. this.array is the array reference, therefore the sum of elements is calculated correctly: 6.



------------------------------------------------------------



Starting with ECMAScript 2015, a shorter syntax for method definitions on objects initializers is introduced.

In objects,

```js
var person = {
first: "Salman",
second: function() {
console.log('Haha');
}
}
```

becomes

```js
var person = {
first: "Salman",
second() {
console.log('Haha');
}
}
```



## Const

...constant cannot change through re-assignment

...constant cannot be re-declared

When you're adding to an array or object you're not re-assigning or re-declaring the constant, it's already declared and assigned, you're just adding to the "list" that the constant points to.

So this works fine

```js
const x = {};
x.foo = 'bar';
console.log(x); // {foo : 'bar'}
```


and this

```js
const y = [];
y.push('foo');
console.log(y); // ['foo'];
```


but neither of these

```js
const x = {};
x = {foo: 'bar'}; // error - re-assigning

const y = ['foo'];
const y = ['bar']; // error - re-declaring

const foo = 'bar'; 
foo = 'bar2';       // error - can not re-assign
var foo = 'bar3';   // error - already declared
function foo() {};  // error - already declared
```

The const declaration creates a read-only reference to a value. It does not mean the value it holds is immutable, just that the variable identifier cannot be reassigned. For instance, in the case where the content is an object, this means the object's contents (e.g., its parameters) can be altered.

**Const != immutable**

It’s very important to understand const. It doesn’t imply immutability.

A variable is like a pointer to a value (it’s a pointer for objects, it’s an assigned value for primitives). const prevents the variable to be assigned to another value. We could say it makes the pointer immutable, but it doesn’t make the value immutable too!

So beware that arrays and objects assigned to const variables can be mutated. However numbers, booleans and strings are immutable per se, so they cannot be mutated. Not because you are using const but just because they are intrinsically immutable.

it is very frequent to assign the returned value of a function to it, but maybe the function returns null or undefined and in that case you want to use a default value. In that case you will probably be tempted to use an if but then you cannot use const since you will reassign the variable value if the condition is met, and we cannot do that, and we don’t want to do that with const. But, in JavaScript there’s a nice solution to that.



❌ Don't do this

```js
let foo = something()
if (!foo) {
  foo = defaultValue
}
```

✅ This is better

```js
const foo = something() || defaultValue
```

Beware that defaultValue will be used for any falsy value returned by something().



## Push

The method that I was most familiar with was push, as that's one of the first ways of manipulating an array that I learned. Push is exceptionally useful as it lets you add one or more elements, that were input arguments, to the array that invoked the method. e.g.

```js
var testArray = [1, 2, 3]; // [1, 2, 3]  
var pushResult = testArray.push(4, 5, 6);  
console.log(pushResult); // 6  
console.log(testArray); // [1, 2, 3, 4, 5, 6]  
```

Wait what is that on the third line? 6? What's the deal with that result? Well the one thing was not immediately apparent to me when I initially started to use push a while back, was that it returned the length of the array after adding the new values, not the modified array. This is something that you learn quickly after the first few rounds of unexpected results.



## Concat

While push alters the array that invoked it, concat returns a new array with the original array joined with the array/s or value/s that were provided as arguments. There are a couple of the things to note about concat and how it creates the returned array. Both strings and numbers are copied into the array, which means that they if the original value is changed, the value in the new array will be unaffected. This is not true for objects. Instead of copying objects into the new array, the references are copied instead. This means that if the values of objects change in one array, they will also be changed in the other array, as they are references to the objects not unique copies.

```js
var test = [1, 2, 3]; // [1, 2, 3]  
var example = [{ test: 'test value'}, 'a', 'b', 4, 5];  
var concatExample = test.concat(example); // [1, 2, 3, { test: 'test value'}, 'a', 'b', 4, 5]  
example[0].test = 'a changed value';  
console.log(concatExample[3].test); // Object { test: "a changed value"}  
example[1] = 'dog';  
console.log(concatExample[4]); // 'a'  
```



## Callback Functions

In JavaScript, functions are first-class objects; that is, functions are of the type Object and they can be used in a first-class manner like any other object (String, Array, Number, etc.) since they are in fact objects themselves. They can be “stored in variables, passed as arguments to functions, created within functions, and returned from functions”.

A callback function, also known as a higher-order function, is a function that is passed to another function (let’s call this other function “otherFunction”) as a parameter, and the callback function is called (or executed) inside the otherFunction.

Some functions produce a value and some don’t whose only result is a side effect. A return statement determines the value the function returns. A return keyword without an expression after it will cause the function to return undefined. Functions that don’t have a return statement at all similarly return undefined.

#### Simple examples -

```js
//Note that the item in the click method's parameter is a function, not a variable.
$("#btn_1").click(function() {
  alert("Btn 1 Clicked");
});


var friends = ["Mike", "Stacy", "Andy", "Rick"];
friends.forEach(function (eachName, index){
console.log(index + 1 + ". " + eachName); // 1. Mike, 2. Stacy, 3. Andy, 4. Rick
});


// global variable
var allUserData = [];
// generic logStuff function that prints to console
function logStuff (userData) {
    if ( typeof userData === "string")
    {
        console.log(userData);
    }
    else if ( typeof userData === "object")
    {
        for (var item in userData) {
            console.log(item + ": " + userData[item]);
        }
    }
}
// A function that takes two parameters, the last one a callback function
function getInput (options, callback) {
    allUserData.push (options);
    callback (options);
}
// When we call the getInput function, we pass logStuff as a parameter.
// So logStuff will be the function that will called back (or executed) inside the getInput function
getInput ({name:"Rich", speciality:"JavaScript"}, logStuff);
//  name: Rich
// speciality: JavaScript
```

When we pass a callback function as an argument to another function, we are only passing the function definition. We are not executing the function in the parameter. The callback is executed at some point inside the containing function’s body just as if the callback were defined in the containing function. This means the callback is a closure. As we know, closures have access to the containing function’s scope, so the callback function can access the containing functions’ variables. It can also access the variables from the global scope.

<br>


Another popular pattern is to declare a named function and pass the name of that function to the parameter. 

```js
// global variable
var allUserData = [];
// generic logStuff function that prints to console
function logStuff (userData) {
    if ( typeof userData === "string")
    {
        console.log(userData);
    }
    else if ( typeof userData === "object")
    {
        for (var item in userData) {
            console.log(item + ": " + userData[item]);
        }
    }
}
// A function that takes two parameters, the last one a callback function
function getInput (options, callback) {
    allUserData.push (options);
    callback (options);
}
// When we call the getInput function, we pass logStuff as a parameter.
// So logStuff will be the function that will called back (or executed) inside the getInput function
getInput ({name:"Rich", speciality:"JavaScript"}, logStuff);
//  name: Rich
// speciality: JavaScript


```

Since the callback function is just a normal function when it is executed, we can pass parameters to it. We can pass any of the containing function’s properties (or global properties) as parameters to the callback function. In the preceding example, we pass options as a parameter to the callback function. Let’s pass a global variable and a local variable:

```js
//Global variable
var generalLastName = "Clinton";

function getInput (options, callback) {
    allUserData.push (options);
// Pass the global variable generalLastName to the callback function
    callback (generalLastName, options);
}
```

<br>

#### Problem -

When the callback function is a method that uses the ***this*** object, we have to modify how we execute the callback function to preserve the ***this*** object context.
Or else the ***this*** object will either point to the global window object (in the browser), if callback was passed to a global function. Or it will point to the object of the containing method.

```js
var clientData = {
    id: 094545,
    fullName: "Not Set",
    // setUserName is a method on the clientData object
    setUserName: function (firstName, lastName)  {
        // this refers to the fullName property in this object
      this.fullName = firstName + " " + lastName;
    }
}

function getUserInput(firstName, lastName, callback)  {
// Do other stuff to validate firstName/lastName here
    // Now save the names
    callback (firstName, lastName);
}
```

Now, when clientData.setUserName is executed, this.fullName will not set the fullName property on the clientData object. Instead, it will set fullName on the window object, since getUserInput is a global function. This happens because the this object in the global function points to the window object.

```js
getUserInput ("Barack", "Obama", clientData.setUserName);
console.log (clientData.fullName);// Not Set

// The fullName property was initialized on the window object
console.log (window.fullName); // Barack Obama
```

<br>

#### Fix -

We can fix the preceding problem by using the Call or Apply function. For now, know that every function in JavaScript has two methods: Call and Apply. And these methods are used to set the this object inside the function and to pass arguments to the functions.

```js
//Note that we have added an extra parameter for the callback object, called "callbackObj"
function getUserInput(firstName, lastName, callback, callbackObj)  {
    // Do other stuff to validate name here
    // The use of the Apply function below will set the this object to be callbackObj
    callback.apply (callbackObj, [firstName, lastName]);
}

// We pass the clientData.setUserName method and the clientData object as parameters. The clientData object will be used by the Apply function to set the this object

getUserInput ("Barack", "Obama", clientData.setUserName, clientData);

// the fullName property on the clientData was correctly set
console.log (clientData.fullName); // Barack Obama
```



## Function Binding

In Ruby, for example, if you are working on the ***foo*** method inside the ***Bar*** class, ***self*** will always point to the object which is the instance of the Bar class.
JavaScript works quite surprisingly here. Because in JavaScript function context is defined while calling the function, not while defining it! 



There are four patterns of invoking functions that defines the context of the function being called:

- function invocation pattern
- method invocation pattern
- constructor invocation pattern
- apply invocation pattern




### Function Invocation Pattern

1. If there are no dots in your function call, your context is likely to be window.

```js
var func = function() {
// ...
};
func()
```

Calling the function this way will set its context (this) to a global variable of an environment on which your JavaScript operates. In browsers it is a window global variable.

```js
var unicorns = {
  func: function() { // ... }
};
var fun = unicorns.func;
fun();
```


The object.func function is a method of the unicorns object. Since JavaScript treats functions like every other value, it can be assigned to the variable. Usually people think that such assignment cannot change the context of the function - it’ll still be unicorns, right? Wrong. Since context binding is determined by the way you call this function, the context will change to window global variable.

### Method Invocation Pattern

2. If there are dots in your function call, your function context will be the right-most element of your dots chain.

```js
var frog = {
  RUN_SOUND: "POP!!",
  run: function() {
    return this.RUN_SOUND;
  }
};
frog.run(); // returns "POP!!" since this points to the `frog` object.
var runningFun = frog.run;
runningFun(); // returns "undefined" since this points to the window
```

### Constructor invocation pattern

3. When calling a function from an object nested within another object, the most nested object will be set as a context of the function call.

   ```js
   var foo = {
   bar: {
   func: function() { ... }
    }
    };
    foo.bar.func();  // this will point to `bar
   ```

   

4. Every time you see a new followed by a function name, your this will point to a newly created empty object.

    ```js
    function Wizard() {
    this.castSpell = function() { return "KABOOM"; };
    }
    var merlin = new Wizard(); // this is set to an empty object {}. Returns `this` implicitly.
    merlin.castSpell() // returns "KABOOM";
    ```

This is exactly how a constructor invocation pattern looks like. Function called with the new operator will cause two things:

Function will have a this context pointing to an empty object.

If you do not specify return, or this function will return a non-object value, this will get returned from such function.

### Apply invocation pattern

When you have a reference of the function in hand, you can call the function with the context provided by you. It can be done by using two methods that functions provide:

call - it takes a context as the first argument. The rest of arguments are arguments passed to the function being called this way.

apply - it takes a context as the first argument and an array of arguments for the function being called as the second argument.

```js
function addAndSetX(a, b) {
  this.x += a + b;
}
var obj1 = { x: 1, y: 2 };

addAndSetX.call(obj1, 1, 1); // this = obj1, obj1 after call = { x: 3, y : 2 }
// It is the same as:
// addAndSetX.apply(obj1, [1, 1]);
```

This is very handy if you need to call function being passed from somewhere else (for example, as an argument to your function) with a certain context. It is not very usable with async callback though - since binding is coupled with a function call. To set proper context with callbacks, you can create a **bounded function**.

Bounded function in JavaScript is a function that is bounded to a given context. That means no matter how you call it, the context of the call will stay the same. The only exception is the new operator which always return a new context. To create a bounded function out of the regular function, the bind method is used. bind method take context to which you want to bind your function as a first argument. The rest of arguments are arguments that will be always passed to such function.

```js
function add(x, y) {
  this.result += x + y;      // result+= x+y  (means)  result = result  + x +y
}
var computation1 = { result: 0 };
var boundedAdd = add.bind(computation1);

boundedAdd(1, 2);    // `this` is set to `computation1`. //  computation1 after call -  { result: 3 }    			// this.result =  0 + 1 + 2  where this is  referencing computation1  
			// console.log(computation1.result); will give 3

var boundedAddPlusTwo = add.bind(computation1, 2); // compuation1 is this refererce, 2 is  x
boundedAddPlusTwo(4); // 4 is y // `this` is set to `computation1`. which now is {result: 3}
                      // computation1 after call: { result: 9 } 
```


Bounded function can’t be changed even by manually calling call and apply! See these examples:

```js
var obj = { boundedPlusTwo: boundedAddPlusTwo };
obj.boundedPlusTwo(4); // `this` is set to `computation1`.
                       // even though method is called on `obj`.
                       // computation1 after call: { result: 15 }

var computation2 = { result: 0 };
boundedAdd.call(computation2, 1, 2); // `this` is set to `computation1`.
                                     // even though context passed to call is
                                     // `computation2`
                                     // computation1 after call: { result: 18 }
```

**In React** 

In the oldest component class syntax called React.createClass binding problem is non-existent. That is because React.createClass performs auto-binding under the hood. All methods you define in an object passed to React.createClass will be automatically bound to the component instance.

 That means you can always use setState, access props and state and so on from these methods.
This may seem cool, but that means under the hood something magical happened which is not under your control. While it may be perfectly acceptable in 99% of cases, it limits your ability to arbitrary set context - which can be a big issue in more sophisticated codebases.
In ECMAScript 2015 classes you need to bind your methods manually. 

That means by default you can’t access properties, state and component methods like setState from event handlers. To do so, you need to bind them explicitly. The best place to bind your event handlers is your constructor: 

```js
class InputExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { text: '' };
    this.change = this.change.bind(this);
  }
  change(ev) {
    this.setState({ text: ev.target.value });
  }
  render() {
    let { text } = this.state;
    return (<input type="text" value={text} onChange={this.change} />);
  }
}
```


This way your event handler has its context bound to the component instance. You can access props and state and call setState or forceUpdate from such bound handler.

**With anonymous arrow functions**

This creates a new function on every render potentially invalidating any efforts to memorise in Button.

```js
class AwesomeProject extends Component {
  constructor() {
    super();
    this.state = {n: 0};
  }
  handleOnPress() {
    this.setState({n: this.state.n + 1});
  }
  render() {
    return <Button onPress={() => this.handleOnPress()} />;  
// same as onPress={this.handleonPress.bind(this)}
  }
}
```

**With bind()**

The same function is reused on every render this is the ES6 best practice.

```js
class AwesomeProject extends Component {
  constructor() {
    super();
    this.state = {n: 0};
    this.handleOnPress = this.handleOnPress.bind(this);
  }
  handleOnPress() {
    this.setState({n: this.state.n + 1});
  }
  render() {
    return <Button onPress={this.handleOnPress} />;
  }
}
```

**With Class properties**

To use this approach, you must enable transform-class-properties or enable stage-2 in Babel.

We remove the need for a constructor and avoid manually binding the handler. 

Initializing state inside of the constructor comes with an overhead of calling super and remembering about props which are React’s abstraction which leaks here a little. We can initialize state directly as a class property.

Due to the fact that the state is defined on the instance, we still have access to this during state initialization. This makes it possible to use props to initialize the state. Let’s, for example, specify the number from which counter starts.

```js
class Counter extends Component {
  state = { counter: this.props.startsWith };
  ...
}
```

It is common for event handlers to modify state. The common source of the performance problems is passing event handlers with a bounded state with arrow function or calling .bind(this) on the event handler inside the render() call. Each of those techniques causes creation of new function inside the render killing PureComponent/PureRenderMixin optimizations. The right way to approach this problem is binding your event handler inside of the component’s constructor.

We can benefit from the fact that arrow function preserves context in which was defined and set handler directly as a class field.

```js
class Counter extends Component {
  state = { counter: 0 };
  onIncrementClick = () => {
    this.setState(this.increment);
  }
```


We get, clean and simple,

```js
class Counter extends Component {
  state = { counter: 0 };
  onIncrementClick = () => {
    this.setState(this.increment);
  }
  increment(state) {
    return { ...state, counter: state.counter + 1 };
  }
  render() {
    return <button onClick={this.onIncrementClick}>{this.state.counter}</button>;
  }
}
```



## JS Links

[Re-Introduction to JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) - basic JS overview from mozilla developer network(mdn)

[33 JS Concepts](https://github.com/leonardomso/33-js-concepts) - important js concepts everyone should know

[JS Cheatsheet](http://overapi.com/javascript) - all api's in one place

<br>

[ES6 Features list](http://es6-features.org/#Constants) - conatins examples with comparison to es5

[ES6 Cheatsheet](https://devhints.io/es6) - by devhints.io

[ES6](https://www.freecodecamp.org/news/write-less-do-more-with-javascript-es6-5fd4a8e50ee2/) - important es6 concepts explained by freecodecamp

[ES6](https://github.com/DrkSephy/es6-cheatsheet) - cheatsheet containing tips, tricks, best practices from DrkSephy/es6-cheatsheet on github

