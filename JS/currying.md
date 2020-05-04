# Currying

Currying is a way of constructing functions that allows partial application of a function’s arguments. What this means is that you can pass a subset of those arguments and get a function back that’s waiting for the rest of the arguments. 

The curried effect is achieved by binding some of the arguments to the first function invoke, so that those values are fixed for the next invocation. 

Currying is when you break down a function that takes multiple arguments into a series of functions that take part of the arguments.  This allows functions of several arguments to have some of their initial arguments partially applied. Here's an example in JavaScript:

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
