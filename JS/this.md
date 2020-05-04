# this

**the “this” keyword allows you to decide which object should be focal when invoking a function or a method.**



The first thing we’ll look at is how to tell what the `this` keyword is referencing. The first and most important question you need to ask yourself when you’re trying to answer this question is ”**Where is this function being invoked?**”. The **only** way you can tell what the `this` keyword is referencing is by looking at where the function using the `this` keyword was invoked.

 Say we had a `greet` function that took in a name an alerted a welcome message.

```js
function greet (name) {
  alert(`Hello, my name is ${name}`)
}
```

If I were to ask you exactly what `greet` was going to alert, what would your answer be? Given only the function definition, it’s impossible to know. To know what `name` is, you’d have to look at the function invocation of `greet`



Now that you know the first step to figuring out what the `this` keyword is referencing is to look at where the function is being invoked, what’s next? To help us with the next step, we’re going to establish 5 rules or guidelines.

1. Implicit Binding
2. Explicit Binding
3. new Binding
4. Lexical Binding
5. window Binding



## Implicit Binding

the goal here is to be able to look at a function definition using the `this` keyword and tell what `this` is referencing. The first and most common rule for doing that is called the `Implicit Binding`.

```js
const user = {
  name: 'Tyler',
  age: 27,
  greet() {
    alert(`Hello, my name is ${this.name}`)
  }
}
```

Now, if you were to invoke the `greet` method on the `user` object, you’d do so be using dot notation.

```js
user.greet()
```

This brings us to the main keypoint of the implicit binding rule. In order to figure out what the `this` keyword is referencing, first, ***look to the left of the dot when the function is invoked\***. If there is a “dot”, look to the left of that dot to find the object that the `this` keyword is referencing.

In the example above, `user` is to “the left of the dot” which means the `this` keyword is referencing the `user` object.



```js
const user = {
  name: 'Tyler',
  age: 27,
  greet() {
    alert(`Hello, my name is ${this.name}`)
  },
  mother: {
    name: 'Stacey',
    greet() {
      alert(`Hello, my name is ${this.name}`)
    }
  }
}
```

Now the question becomes, what is each invocation below going to alert?

```js
user.greet()
user.mother.greet()
```

 In the first invocation, `user` is to the left of the dot which means `this` is going to reference `user`. In the second invocation, `mother` is to the left of the dot which means `this` is going to reference `mother`. But, what if there is no dot?



## Explicit Binding

```js
function greet () {
  alert(`Hello, my name is ${this.name}`)
}

const user = {
  name: 'Tyler',
  age: 27,
}
```

### call

We know that in order to tell what the `this` keyword is referencing we first have to look at where the function is being invoked. Now, this brings up the question, how can we invoke `greet` but have it be invoked with the `this` keyword referencing the `user` object. We can’t just do `user.greet()` like we did before because `user` doesn’t have a `greet` method. In JavaScript, every function contains a method which allows you to do exactly this and that method is named `call`.

*“call” is a method on every function that allows you to invoke the function specifying in what context the function will be invoked.*

With that in mind, we can invoke `greet` in the context of `user` with the following code -

```js
greet.call(user)
```

The first argument you pass to call will be what the `this` keyword inside that function is referencing. 

What if we also wanted to pass in some arguments? Say along with their name, we also wanted to alert what languages they know. Something like this

```js
function greet (l1, l2, l3) {
  alert(
    `Hello, my name is ${this.name} and I know ${l1}, ${l2}, and ${l3}`
  )
}
```

Now in order to pass arguments to a function being invoked with `.call`, you pass them in one by one after you specify the first argument which is the context.

```js
function greet (l1, l2, l3) {
  alert(
    `Hello, my name is ${this.name} and I know ${l1}, ${l2}, and ${l3}`
  )
}

const user = {
  name: 'Tyler',
  age: 27,
}

const languages = ['JavaScript', 'Ruby', 'Python']

greet.call(user, languages[0], languages[1], languages[2])
```

### apply

It would be nice if we could just pass in the whole array as the second argument and JavaScript would spread those out for us. Well good news for us, this is exactly what `.apply` does. `.apply` is the exact same thing as `.call`, but instead of passing in arguments one by one, you can pass in a single array and it will spread each element in the array out for you as arguments to the function.

So now using `.apply`, our code can change into this (below) with everything else staying the same.

```js
const languages = ['JavaScript', 'Ruby', 'Python']

// greet.call(user, languages[0], languages[1], languages[2])
greet.apply(user, languages)
```

### bind

`.bind` is the exact same as `.call` but instead of immediately invoking the function, it’ll return a new function that you can invoke at a later time. So if we look at our code from earlier, using `.bind`, it’ll look like this

```js
function greet (l1, l2, l3) {
  alert(
    `Hello, my name is ${this.name} and I know ${l1}, ${l2}, and ${l3}`
  )
}

const user = {
  name: 'Tyler',
  age: 27,
}

const languages = ['JavaScript', 'Ruby', 'Python']

const newFn = greet.bind(user, languages[0], languages[1], languages[2])
newFn() // alerts "Hello, my name is Tyler and I know JavaScript, Ruby, and Python"
```



## new Binding

The third rule for figuring out what the `this` keyword is referencing is called the `new` binding. If you’re unfamiliar with the `new` keyword in JavaScript, whenever you invoke a function with the `new` keyword, under the hood, the JavaScript interpretor will create a brand new object for you and call it `this`. So, naturally, if a function was called with `new`, the `this` keyword is referencing that new object that the interpretor created.

```js
function User (name, age) {
  /*
    Under the hood, JavaScript creates a new object called `this`
    which delegates to the User's prototype on failed lookups. If a
    function is called with the new keyword, then it's this new object
    that interpretor created that the this keyword is referencing.
  */

  this.name = name
  this.age = age
}

const me = new User('Tyler', 27)
```



## Lexical Binding

 Unlike normal functions, arrow functions don’t have their own `this`. Instead, `this` is determined `lexically`. 

Now, instead of having `languages` and `greet` as separate from the object, let’s combine them.

```js
const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {}
}
```

Earlier we assumed that the `languages` array would always have a length of 3. By doing so, we were able to use hardcoded variables like `l1`, `l2`, and `l3`. Let’s make `greet` a little more intelligent now and assume that `languages` can be of any length. To do this, we’ll use `.reduce` in order to create our string.

```js
const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {
    const hello = `Hello, my name is ${this.name} and I know`

    const langs = this.languages.reduce(function (str, lang, i) {
      if (i === this.languages.length - 1) {
        return `${str} and ${lang}.`
      }

      return `${str} ${lang},`
    }, "")

    alert(hello + langs)
  }
}
```

When we invoke `user.greet()`, we expect to see `Hello, my name is Tyler and I know JavaScript, Ruby, and Python.`. Sadly, there’s an error. You’ll notice it’s throwing the error `Uncaught TypeError: Cannot read property 'length' of undefined`. Gross. The only place we’re using `.length` is on line 9, so we know our error is there.

```js
if (i === this.languages.length - 1) {}
```

According to our error, `this.langauges` is undefined. First, we need to look at where the function is being invoked. Wait? Where is the function being invoked? The function is being passed to `.reduce` so we have no idea. We never actually see the invocation of our anonymous function since JavaScript does that itself in the implementation of `.reduce`. That’s the problem. We need to specify that we want the anonymous function we pass to `.reduce` to be invoked in the context of `user`. That way `this.languages` will reference `user.languages`. As we learned above, we can use `.bind`.

```js
const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {
    const hello = `Hello, my name is ${this.name} and I know`

    const langs = this.languages.reduce(function (str, lang, i) {
      if (i === this.languages.length - 1) {
        return `${str} and ${lang}.`
      }

      return `${str} ${lang},`
    }.bind(this), "")

    alert(hello + langs)
  }
}
```

So we’ve seen how `.bind` solves the issue. Earlier it's said that with arrow functions ”`this` is determined `lexically`. That’s a fancy way of saying `this` is determined how you’d expect, following the normal variable lookup rules.”



If we re-write the code above and do nothing but use an anonymous arrow function instead of an anonymous function declaration, everything “just works”.

```js
const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {
    const hello = `Hello, my name is ${this.name} and I know`

    const langs = this.languages.reduce((str, lang, i) => {
      if (i === this.languages.length - 1) {
        return `${str} and ${lang}.`
      }

      return `${str} ${lang},`
    }, "")

    alert(hello + langs)
  }
}
```

Again the reason for this because with arrow functions, `this` is determined “lexically”. Arrow functions don’t have their own `this`. Instead, just like with variable lookups, the JavaScript interpretor will look to the enclosing (parent) scope to determine what `this` is referencing.

In arrow function, *this* is based on **enclosing function's execution context**.

```js
const jeff = {
  phone: '34682',
  whois: function() {
		console.log(this) // the object jeff
},
  butwho: () => console.log(this) //global object
}
```

It doesn't have it's own binding to *this*, so it looks up to it's parent enclosing object and uses that *this* value.



## window Binding

Finally is the “catch-all” case - the window binding. Let’s say we had the following code

```js
function sayAge () {
  console.log(`My age is ${this.age}`)
}

const user = {
  name: 'Tyler',
  age: 27
}
```

As we covered earlier, if you wanted to invoke `sayAge` in the context of `user`, you could use `.call`, `.apply`, or `.bind`. What would happen if we didn’t use any of those and instead just invoked `sayAge` as you normally would

```js
sayAge() // My age is undefined
```

What you’d get is, unsurprisingly, `My age is undefined` because `this.age` would be undefined. JavaScript is defaulting `this` to reference the `window` object. What that means is if we add an `age` property to the `window` object, then when we invoke our `sayAge` function again, `this.age` will no longer be undefined but instead, it’ll be whatever the `age` property is on the window object.

```js
window.age = 27

function sayAge () {
  console.log(`My age is ${this.age}`)
}
```



## Method Chaining

```js
function Horse(name) 
	this.name = name;
	this.poop = function() {
		console.log('poop'); // poop poop poop
		return this; // to keep a referance to original object, return this. 					Then you can chanin infinite number of method calls
	}
}

const myHorse = new Horse('Secretariat');
myHorse.poop().poop().poop();
```

