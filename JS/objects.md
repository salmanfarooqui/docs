# Objects

In JavaScript, almost "everything" is an object.

- Booleans can be objects (if defined with the `new` keyword)
- Numbers can be objects (if defined with the `new` keyword)
- Strings can be objects (if defined with the `new` keyword)
- Dates are always objects
- Maths are always objects
- Regular expressions are always objects
- Arrays are always objects
- Functions are always objects
- Objects are always objects

***All JavaScript values, except primitives, are objects.***



A primitive value is a value that has no properties or methods. JavaScript defines 5 types of primitive data types:

1. string
2. number
3. `boolean`
4. null
5. undefined



An empty object can be created using one of two syntaxes:

```javascript
let user = new Object(); // "object constructor" syntax
let user = {};  // "object literal" syntax
```



## Property

A JavaScript object is a collection of unordered properties.

The syntax for accessing the property of an object is:  `objectName.property`  or  `objectName["property"]`



### . vs [] notation in Objects

```js
let obj = {
  cat: 'meow',
  dog: 'woof'
};
```

**. notation**

`let sound = obj.cat`

When working with dot notation, property identifies can only be alphanumeric (and _ and $). Properties can’t start with a number. Dot notation is much easier to read than bracket notation and is therefor used more often. 

We can’t use variables with dot notation.

****

**[] notation**

> When working with bracket notation, property identifiers only have to be a String. They can include any characters, including spaces. Variables may also be used as long as the variable resolves to a String.

`let sound = obj[‘cat’]`

This means there are fewer limitations when working with bracket notation. We can now have ***spaces*** in our strings, and can even start strings with numbers.

Perhaps most importantly, ***we can now use variables to access properties in an object***. It’s important the variable you are using references a String.

```js
let obj = {
  cat: 'meow',
  dog: 'woof'
};
let dog = 'cat';
let sound = obj[dog];
console.log(sound); // meow
```



**Conclusion -** Try to always use Dot. And when you want to access object property with a variable, use Bracket



### Deleting Properties

The `delete` keyword deletes a property from an object:

```js
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
delete person.age;   // or delete person["age"];
```



## Methods

Methods are nothing more than properties that hold function values. This is a simple method:

```js
let rabbit = {};
rabbit.speak = function(line) {
  console.log(`The rabbit says '${line}'`);
};

rabbit.speak("I'm alive.");
// → The rabbit says 'I'm alive.'
```

Usually a method needs to do something with the object it was called on. When a function is called as a method—looked up as a property and immediately called, as in `object.method()`—***the binding called `this` in its body automatically points at the object that it was called on***.



All objects in JavaScript descend from the parent Object constructor. Object has many useful built-in methods we can use and access to make working with individual objects straightforward. Unlike Array prototype methods like sort() and reverse() that are used on the array instance, Object methods are used directly on the Object constructor, and use the object instance as a parameter. This is known as a static method.

### Object.create()

The Object.create() method is used to create a new object and link it to the prototype of an existing object.

See more info see [Prototype section](/JS/objects#prototype)

<br>

<br>

### Object.keys()

Object.keys() creates an array containing the keys of an object.

```js
/ Initialize an object
const employees = {
    boss: 'Michael',
    secretary: 'Pam',
    sales: 'Jim',
    accountant: 'Oscar'
};

// Get the keys of the object
const keys = Object.keys(employees);

console.log(keys);
// Output["boss", "secretary", "sales", "accountant"]
```

<br>

Object.keys can be used to iterate through the keys and values of an object.

```js
// Iterate through the keys
Object.keys(employees).forEach(key => {
    let value = employees[key];
     console.log(`${key}: ${value}`);
});

// Outputboss: Michael
// secretary: Pam
// sales: Jim
// accountant: Oscar
```

<br>

Object.keys is also useful for checking the length of an object.

```js
// Get the length of the keys
const length = Object.keys(employees).length;

console.log(length);
// 4
```

<br>

### Object.values()

Object.values() creates an array containing the values of an object.

```js
const session = {
    id: 1,
    time: `26-July-2018`,
    device: 'mobile',
    browser: 'Chrome'
};

// Get all values of the object
const values = Object.values(session);

console.log(values);
// [1, "26-July-2018", "mobile", "Chrome"]
```

<br>

### Object.entries()

Object.entries() creates a nested array of the key/value pairs of an object.

```js
const operatingSystem = {
    name: 'Ubuntu',
    version: 18.04,
    license: 'Open Source'
};

// Get the object key/value pairs
const entries = Object.entries(operatingSystem);

console.log(entries);

 // [
 //   ["name", "Ubuntu"]
 //   ["version", 18.04]
 //   ["license", "Open Source"]
 // ]
```

<br>

Once we have the key/value pair arrays, we can use the `forEach()` method to loop through and work with the results.

```js
entries.forEach(entry => {
    let key = entry[0];
    let value = entry[1];

    console.log(`${key}: ${value}`);
});

// Outputname: Ubuntu
// version: 18.04
// license: Open Source
```

<br>

### Object.assign()

You can create a shallow copy i.e. a top level properties copy, using Objects.assign() method. Object.assign() is used to copy values from one object to another.

We can create two objects, and merge them with `Object.assign()`.

```js
const name = {
    firstName: 'Philip',
    lastName: 'Fry'
};

const details = {
    job: 'Delivery Boy',
    employer: 'Planet Express'
};

// Merge the objects
const character = Object.assign(name, details);

console.log(character);

// {firstName: "Philip", lastName: "Fry", job: "Delivery Boy", employer: "Planet Express"}
```

It is also possible to use the spread operator (`...`) to accomplish the same task. `const character = {...name, ...details}`

For for info see [Value vs Reference section](/JS/objects#copying-objects-and-arrays)

<br>

### Object.freeze()

Object.freeze() prevents modification to properties and values of an object, and prevents properties from being added or removed from an object.

```js
const user = {
    username: 'AzureDiamond',
    password: 'hunter2'
};

// Freeze the object
const newUser = Object.freeze(user);

newUser.password = '*******';
newUser.active = true;

console.log(newUser);

// {username: "AzureDiamond", password: "hunter2"}
```

In the example above, we tried to override the password hunter2 with *******, but the password property remained the same.  `Object.isFrozen()` is available to determine whether an object has been frozen or not, and returns a Boolean.

<br>

### Object.seal()

[`Object.seal()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal) prevents new properties from being added to an object, but allows the modification of existing properties.

<br>

### Object.getPrototypeOf()

Object.getPrototypeOf() is used to get the internal hidden [[Prototype]] of an object, also accessible through the `_proto_` property.

In this example, we can create an array, which has access to the `Array` prototype.

```js
const employees = ['Ron', 'April', 'Andy', 'Leslie'];
Object.getPrototypeOf(employees);

// [constructor: ƒ, concat: ƒ, find: ƒ, findIndex: ƒ, pop: ƒ, …]
```

We can see in the output that the prototype of the `employees` array has access to `pop`, `find`, and other Array prototype methods. We can confirm this by testing the `employees` prototype against `Array.prototype`.

```js
Object.getPrototypeOf(employees) === Array.prototype;
// true
```

This method can be useful to get more information about an object or ensure it has access to the prototype of another object.

> There is also a related `Object.setPrototypeOf()` method that will add one prototype to another object. It is recommended that you use `Object.create()` instead as it is faster and more performant.



## Prototype

```js
let empty = {};
console.log(empty.toString);
// → function toString(){…}
console.log(empty.toString());
// → [object Object]
```

I pulled a property out of an empty object. Magic!

In addition to their set of properties, most objects also have a *prototype*. A prototype is another object that is used as a fallback source of properties. ***When an object gets a request for a property that it does not have, its prototype will be searched for the property,*** then the prototype’s prototype, and so on.

So who is the prototype of that empty object? It is the great ancestral prototype, the entity behind almost all objects, `Object.prototype`. It provides a few methods that show up in all objects, such as `toString`, which converts an object to a string representation.

Many objects don’t directly have `Object.prototype` as their prototype but instead have another object that provides a different set of default properties. Functions derive from `Function.prototype`, and arrays derive from `Array.prototype`. Such a prototype object will itself have a prototype, often `Object.prototype`.



You can use `Object.create` to create an object with a specific prototype.

**Syntax -** `Object.create(proto, [propertiesObject])` where `proto` is the object which should be the prototype of the newly-created object and `propertiesObject` is the enumerable properties to be added to the newly created object.

```js
let protoRabbit = {
  speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
};
let killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!"); // → The killer rabbit says 'SKREEEE!'
```

The Object.create() method creates a new object ***using an existing object as the prototype of the newly created object.*** Object.create() is used for implementing inheritance. 

Here, killerRabbit does not have speak property so it looks it in it's prototype i.e protoRabbit and executes it. The protoRabbit acts as a container for the properties that are shared by all rabbits. An individual rabbit object, like the killerRabbit, contains properties that apply only to itself and derives shared properties from its prototype.

A ***property like `speak(line)` in an object expression is a shorthand way of defining a method.*** It creates a property called `speak` and gives it a function as its value.



#### All in One, Object.create, prototype, new

```js
function Animal (name, energy) {
	let animal = {}
	animal.name = name
	animal.energy = energy
	animal.eat = function (amount) {
		console.log(`${this.name} is eating.`)
		this.energy += amount
	}
	animal.sleep = function (length) {
		console.log(`${this.name} is sleeping.`)
		this.energy += length
	}
	animal.play = function (length) {
		console.log(`${this.name} is playing.`)
		this.energy -= length
	}
	return animal
}
const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)
```

This works great and it’s incredibly simple. But there’s no reason to re-create those methods as we’re currently doing whenever we create a new animal. We’re just wasting memory and making each animal object bigger than it needs to be.

What if instead of re-creating those methods every time we create a new animal, we move them to their own object then we can have each animal reference that object?

```js
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = {}
  animal.name = name
  animal.energy = energy
  animal.eat = animalMethods.eat
  animal.sleep = animalMethods.sleep
  animal.play = animalMethods.play

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)
```

By moving the shared methods to their own object and referencing that object inside of our `Animal` function, we’ve now solved the problem of memory waste and overly large animal objects.



**Let’s improve our example once again by using `Object.create`.** Now with `Object.create` in our tool shed, how can we use it in order to simplify our `Animal` code from earlier? Well, instead of adding all the shared methods to the animal one by one like we’re doing now, we can use Object.create to delegate to the `animalMethods` object instead. 

```js
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = Object.create(animalMethods)
  animal.name = name
  animal.energy = energy

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)

leo.eat(10)
snoop.play(5)
```

 So now when we call `leo.eat`, JavaScript will look for the `eat` method on the `leo` object. That lookup will fail, then, because of Object.create, it’ll delegate to the `animalMethods` object which is where it’ll find `eat`. 

It seems just a tad “hacky” to have to manage a separate object (`animalMethods`) in order to share methods across instances. That seems like a common feature that you’d want to be implemented into the language itself. **This is where `prototype` comes in.** It allows us to share methods across all instances of a function.

```js
function Animal (name, energy) {
  	let animal = Object.create(Animal.prototype)
  	animal.name = name
	animal.energy = energy
	return animal
}
Animal.prototype.eat = function (amount) {
  	console.log(`${this.name} is eating.`)
  	this.energy += amount
}

Animal.prototype.sleep = function (length) {
  	console.log(`${this.name} is sleeping.`)
  	this.energy += length
}

Animal.prototype.play = function (length) {
  	console.log(`${this.name} is playing.`)
  	this.energy -= length
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)
leo.eat(10)
snoop.play(5)
```



**What’s nice about the slow, methodical approach we took to get here is you’ll now have a deep understanding of exactly what the `new` keyword in JavaScript is doing under the hood.**

Looking back at our `Animal` constructor, the two most important parts were creating the object and returning it. Without creating the object with `Object.create`, we wouldn’t be able to delegate to the function’s prototype on failed lookups. Without the `return` statement, we wouldn’t ever get back the created object.

Here’s the cool thing about `new` - when you invoke a function using the `new` keyword, those two lines are done for you implicitly (“under the hood”) and the object that is created is called `this`.

Using comments to show what happens under the hood and assuming the `Animal` constructor is called with the `new` keyword, it can be re-written as this.

```js
function Animal (name, energy) {
  // const this = Object.create(Animal.prototype)
  this.name = name
  this.energy = energy
  // return this
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
```



In 2015, EcmaScript (the official JavaScript specification) **6 was released with support for Classes and the `class` keyword.** Let’s see how our `Animal` constructor function above would look like with the new class syntax.

```js
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
```

Class allows you to create a blueprint for an object. Then whenever you create an instance of that Class, you get an object with the properties and methods defined in the blueprint.



## Value vs Reference

### By Value

JavaScript has 5 data types that are passed by value: `Boolean`, `null`, `undefined`, `String`, and `Number`. We’ll call these primitive types. If a primitive type is assigned to a variable, we can think of that variable as *containing* the primitive value. Changing one does not change the other.



### By Reference

JavaScript has 3 data types that are passed by reference: `Array`, `Function`, and `Object`.

Variables that are assigned a non-primitive value are given a *reference* to that value. ***That reference points to the object’s location in memory.*** 

Objects are created at some location in your computer’s memory. When we write `arr = []`, we’ve created an array in memory. What the variable `arr` receives is the address, the location, of that array.

When we use `arr` to do something, such as pushing a value `arr.push(1)`, the Javascript engine goes to the location of `arr` in memory and works with the information stored there.

When a reference type value, an object, is copied to another variable using `=`, the address of that value is what’s actually copied over `var ref = [1]; var refCopy = ref;` Each variable now contains a reference to the *same array*. That means that if we alter `ref`, `refCopy` will see those changes:

```js
ref.push(2);
console.log(ref, refCopy); // -> [1, 2], [1, 2]
```



##### Copying Objects and Arrays

Since a simple assignment is not enough to produce a copy of an object, that can be achieved by other approaches:



<u>**Arrays**</u>

**slice()**

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals.slice(2)); // ["camel", "duck", "elephant"]
```

**concat()**

```js
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
console.log(array1.concat(array2)); // ["a", "b", "c", "d", "e", "f"]
```

**spread (ES6)**

```js
const arr = [1, 2, 3];
const arr2 = [...arr]; // like arr.slice()
```



**<u>Objects</u>**

**assign()**

```js
let human = Object.assign({}, person, { age: 20 });
console.log(person, human);
>>{  person: { name: 'Marina', age: 29 },  human: { name: 'Marina', age: 20 }}
```



Until now we assumed that all properties of user are primitive. But properties can be references to other objects. What to do with them?

Like this:

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

Now it’s not enough to copy clone.sizes = user.sizes, because the user.sizes is an object, it will be copied by reference. So clone and user will share the same sizes:

Like this:

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, same object

// user and clone share sizes
user.sizes.width++;       // change a property from one place
alert(clone.sizes.width); // 51, see the result from the other one
```

To fix that, we should use the cloning loop that examines each value of user[key] and, if it’s an object, then replicate its structure as well. That is called a “deep cloning”.

There’s a standard algorithm for deep cloning that handles the case above and more complex cases, called the Structured cloning algorithm.

We can use recursion to implement it. Or, not to reinvent the wheel, take an existing implementation, for instance _.cloneDeep(obj) from the JavaScript library lodash.




##### Passing Parameters through Functions - 

We refer to functions that don’t affect anything in the outside scope as ***pure functions***. As long as a function only takes primitive values as parameters and doesn’t use any variables in its surrounding scope, it is automatically pure, as it can’t affect anything in the outside scope. All variables created inside are garbage-collected as soon as the function returns.

If a function takes in an array reference and alters the array that it points to, perhaps by pushing to it, variables in the surrounding scope that reference that array see that change. After the function returns, the changes it makes persist in the outer scope. This can cause undesired side effects that can be difficult to track down.

```js
function changeAgeImpure(person) {
    person.age = 25;
    return person;
}
var alex = {
    name: 'Alex',
    age: 30
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // -> { name: 'Alex', age: 25 }
console.log(changedAlex); // -> { name: 'Alex', age: 25 }
```

Now let’s look at a pure function.

```js
function changeAgePure(person) {
    var newPersonObj = JSON.parse(JSON.stringify(person));
    newPersonObj.age = 25;
    return newPersonObj;
}
var alex = {
    name: 'Alex',
    age: 30
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }
```

In this function, we use `JSON.stringify` to transform the object we’re passed into a string, and then parse it back into an object with `JSON.parse`. By performing this transformation and storing the result in a new variable, we’ve created a new object. The new object has the same properties as the original but it is a distinctly separate object in memory. When we change the `age` property on this new object, the original is unaffected. This function is now pure. It can’t affect any object outside its own scope, not even the object that was passed in.

Many native array functions, including `Array.map` and `Array.filter`, are therefore written as pure functions. They take in an array reference and internally, they copy the array and work with the copy instead of the original. This makes it so the original is untouched, the outer scope is unaffected, and we’re returned a reference to a brand new array.



##### Tip - 

When the equality operators, `==` and `===`, are used on reference-type variables, they check the reference. If the variables contain a reference to the same item, the comparison will result in `true`.

```js
var arrRef = [’Hi!’];
var arrRef2 = arrRef;
console.log(arrRef === arrRef2); // -> true
```

If they’re distinct objects, even if they contain identical properties, the comparison will result in `false`.

```js
var arr1 = ['Hi!'];
var arr2 = ['Hi!'];
console.log(arr1 === arr2); // -> false
```



