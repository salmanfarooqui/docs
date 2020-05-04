# new Operator

Suppose you have this function:

```js
var Foo = function(){
  this.A = 1;
  this.B = 2;
};
```

If you call this as a standalone function like so:

```js
Foo();
```

Executing this function will add two properties to the `window` object (`A` and `B`). It adds it to the `window` because `window` is the object that called the function when you execute it like that, and `this` in a function is the object that called the function. In Javascript at least.

Now, call it like this with `new`:

```js
var bar = new Foo();
```

What happens when you add `new` to a function call is that a new object is created (just `var bar = new Object()`) and that the `this` within the function points to the new `Object` you just created, instead of to the object that called the function. So `bar` is now an object with the properties `A` and `B`. 

<br >
<br >

Constructor functions technically are regular functions. There are two conventions though:

1. They are ***named with capital letter first.***
2. They should be executed only with `"new"` operator.



The *new operator* lets developers create an instance of a user-defined object type or of one of the built-in object types that has a constructor function. 



> ```javascript
> function User(name) {
>   this.name = name;
>   this.isAdmin = false;
> }
> 
> let user = new User("Jack");
> 
> alert(user.name); // Jack
> alert(user.isAdmin); // false
> ```
>
> When a function is executed with `new`, it does the following steps:
>
> 1. ***A new empty object is created and assigned to `this`.***
> 2. Links (sets the constructor of) this object to another object;
> 3. The function body executes. Usually it modifies `this`, adds new properties to it.
> 4. ***The value of `this` is returned, if the function doesn't return its own object.***
>
> In other words, `new User(...)` does something like:
>
> ```javascript
> function User(name) {
>   // this = {};  (implicitly)
> 
>   // add properties to this
>   this.name = name;
>   this.isAdmin = false;
> 
>   // return this;  (implicitly)
> }
> ```



Usually, constructors do not have a `return` statement. Their task is to write all necessary stuff into `this`, and it automatically becomes the result.

But if there is a `return` statement, then the rule is simple:

- If `return` is called with an object, then the object is returned instead of `this`.
- If `return` is called with a primitive, it’s ignored.

<br>
<br>

JavaScript has built-in constructors for native objects:
```js
var x1 = new Object();    // A new Object object
var x2 = new String();    // A new String object
var x3 = new Number();    // A new Number object
var x4 = new Boolean();   // A new Boolean object
var x5 = new Array();     // A new Array object
var x6 = new RegExp();    // A new RegExp object
var x7 = new Function();  // A new Function object
var x8 = new Date();      // A new Date object
```
As you can see above, JavaScript has object versions of the primitive data types String, Number, and Boolean. But there is no reason to create complex objects. Primitive values are much faster.

Use object literals {} instead of new Object().
Use string literals "" instead of new String().
Use number literals 12345 instead of new Number().
Use boolean literals true / false instead of new Boolean().
Use array literals [] instead of new Array().

<br>

### Tip -

If we have many lines of code all about creation of a single complex object, we can wrap them in constructor function, like this:

```javascript
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // ...other code for user creation
  // maybe complex logic and statements
  // local variables etc
};
```

The constructor can’t be called again, because it is not saved anywhere, just created and called. So **this trick aims to encapsulate the code that constructs the single object, without future reuse**.

<br>

### Tip - 

Diff between `new Function()` and `new function()`

```js
// first
var func = new Function();
func.property = "some property";
return func;

//second
var func = new function(){
  this.property = "some property";
}
return func;
```

In the first case, you create a new object and you apply the Function constructor. Return value is a **function**.

In the second example, you create a new object and you apply an anonymous function as constructor. Return value is an **object**.
