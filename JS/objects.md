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



### Looping through properties

`for...in` loops - This method traverses all **enumerable** properties **of an object and its prototype chain**

To find out if an object has a specific property as one of its own property, you use the hasOwnProperty method. This method is very useful because from time to time you need to enumerate an object and you want only the own properties, not the inherited ones. hasOwnProperty restricts its search to the root elements of the object in which the property is being searched

```js
var school = {schoolName:"MIT"};
console.log(school.hasOwnProperty ("schoolName"));  // true
```



##### Enumerable

If you create an object via `myObj = {foo: 'bar'}` or something thereabouts, all properties are enumerable. So what's not enumerable? Certain objects have some non-enumerable properties, for example if you call `Object.getOwnPropertyNames([])` (which returns an array of all properties, enumerable or not, on []), it will return `['length']`, which includes the non-enumerable property of an array, 'length'.

<br>

You can create your own non-enumerable properties using the `Object.defineProperty` method:

```js
  var car = {
    make: 'Honda',
    model: 'Civic',
    year: '2008',
    condition: 'bad',
    mileage: 36000
  };

  Object.defineProperty(car, 'mySecretAboutTheCar', {
    value: 'cat pee in back seat',
    enumerable: false
  });
```

Now, the fact that there is even a secret about the car is hidden. Of course they can still access the property directly and get the answer:

```js
console.log(car.mySecretAboutTheCar); // prints 'cat pee in back seat'
```

But, they would have to know that the property exists first, because if they're trying to access it through `for..in` or `Object.keys` it will remain completely secret.

```js
console.log(Object.keys(car)); //prints ['make', 'model', 'year', 'condition', 'mileage']
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

> Inheritance - One object gets access to the properties and methods of another object



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



### \__proto__ vs prototype in JavaScript

**When a function is created in JavaScript, the JavaScript engine adds a `prototype` property to the function.** This `prototype` property is an object (called a prototype object) that has a `constructor` property by default. The `constructor` property points back to the function on which `prototype` object is a property. We can access the function’s prototype property using `functionName.prototype`.

![1_15Qo3ab3NPkLfXpj5AncaQ](https://docs.salmanfarooqui.com/JS/images/1_15Qo3ab3NPkLfXpj5AncaQ.png)



In other words, `prototype` is a property of a Function object. It is the prototype of objects constructed by that function.

<br>

`__proto__` is internal property of an object, pointing to its prototype. Current standards provide an equivalent `Object.getPrototypeOf(O)` method.

<br>

> `__proto__` is the actual object that is used in the lookup chain to resolve methods, etc.
>
> `prototype` is the object that is used to build `__proto__` when you create an object with `new`

<br>

```js
var a = {};
var b = function() { };
var c = [];

a.__proto__
 // {
 // constructor: ƒ Object()
 // hasOwnProperty: ƒ hasOwnProperty()
 // isPrototypeOf: ƒ isPrototypeOf()
 // propertyIsEnumerable: ƒ propertyIsEnumerable()
 // toLocaleString: ƒ toLocaleString()
 // toString: ƒ toString()
 // valueOf: ƒ valueOf()
 // get __proto__: ƒ __proto__()
 // set __proto__: ƒ __proto__()
 // }

a.__proto__.__proto__
 // null

---------------------------------------------

b.__proto__
 // f() { [native code]}

b.__proto__.__proto__
 // {
 // constructor: ƒ Object()
 // hasOwnProperty: ƒ hasOwnProperty()
 // isPrototypeOf: ƒ isPrototypeOf()
 // propertyIsEnumerable: ƒ propertyIsEnumerable()
 // toLocaleString: ƒ toLocaleString()
 // toString: ƒ toString()
 // valueOf: ƒ valueOf()
 // get __proto__: ƒ __proto__()
 // set __proto__: ƒ __proto__()
 // }

b.__proto__.__proto__.__proto__
 // null

b.prototype
 // {
 // constructor: ƒ ()
 /// __proto__: Object
 // }

b.prototype.constructor
 // ƒ () {}
b.prototype.__proto__
 // {
 // constructor: ƒ Object()
 // hasOwnProperty: ƒ hasOwnProperty()
 // isPrototypeOf: ƒ isPrototypeOf()
 // propertyIsEnumerable: ƒ propertyIsEnumerable()
 // toLocaleString: ƒ toLocaleString()
 // toString: ƒ toString()
 // valueOf: ƒ valueOf()
 // get __proto__: ƒ __proto__()
 // set __proto__: ƒ __proto__()
 // }

--------------------------------------------

c.__proto__
 // [
 // concat: ƒ concat()
 // constructor: ƒ Array()
 // copyWithin: ƒ copyWithin()
 // entries: ƒ entries()
 // every: ƒ every()
 // find: ƒ find()
 /// length: 0
 // map: ƒ map()
 // push: ƒ push()
 // reduce: ƒ reduce()
 // ...
 // values: ƒ values()
 // Symbol(Symbol.iterator): ƒ values()
 // __proto__: Object
 // ]

c.__proto__.__proto__
 // {
 // constructor: ƒ Object()
 // hasOwnProperty: ƒ hasOwnProperty()
 // isPrototypeOf: ƒ isPrototypeOf()
 // propertyIsEnumerable: ƒ propertyIsEnumerable()
 // toLocaleString: ƒ toLocaleString()
 // toString: ƒ toString()
 // valueOf: ƒ valueOf()
 // get __proto__: ƒ __proto__()
 // set __proto__: ƒ __proto__()
 // }

c.__proto__.__proto__.__proto__
 // null
```

The methods that we use like toString are not on our function but rather they are on the prototype. When JS Engine couldn't find it in our function it looks up on it's prototype and if it's not there too it looks on it's prototype's prototype and so on. 

<br>

When creating a function, a property object called prototype is being created automatically (you didn't create it yourself) and is being attached to the function object (the constructor)

```js
function Foo () {
    this.name = 'John Doe';
}
Foo.prototype.myName = function () {
    return 'My name is ' + this.name;
}
```

If you will create a new object out of Foo using the new keyword, you basically creating (among other things) a new object that has an internal link to the function's prototype Foo we discussed earlier:

```js
var b = new Foo();
b.__proto__ === Foo.prototype // true
```

In other words, Instances have \__proto__, classes have prototype.

```js
function Person(name){
    this.name = name
 }; 
var eve = new Person("Eve");
eve.__proto__ == Person.prototype //true
eve.prototype  //undefined
```

**To explain let us create a function**

```js
function a (name) {
  this.name = name;
 }
```


When JavaScript executes this code, it adds prototype property to a, prototype property is an object with two properties to it:

1. constructor

2. \_proto_

   

   So when we do a.prototype
    it returns

   ```js
    constructor: a()
   __proto__: Object
   ```

   


Now as you can see constructor is nothing but the function a itself and \__proto__ points to the root level Object of JavaScript.

Let us see what happens when we use a function with new key word.

`var b = new a ('JavaScript');`

When JavaScript executes this code it does 4 things:

1.	It creates a new object, an empty object // {}
2.	It creates \_proto_ on b and makes it point to a.prototype so b.\__proto__ === a.prototype
3.	It executes a.prototype.constructor (which is definition of function a ) with the newly created object (created in step#1) as its context (this), hence the name property passed as 'JavaScript' (which is added to this) gets added to newly created object.
4.	It returns newly created object in (created in step#1) so var b gets assigned to newly created object.
   Now if we add a.prototype.car = "BMW" and do  b.car, the output "BMW" appears.

this is because when JavaScript executed this code it searched for car property on b, it did not find then JavaScript used b.\__proto__ (which was made to point to 'a.prototype' in step#2) and finds car property so return "BMW".

<br>



> The `__proto__` property of an object and the `Object.getPrototypeOf()` method are both ways to access the prototype of an object. 
>
> Because `__proto__` is simply a property of an object and was put there back in the day to access the prototype of an object. `__proto__` is now deprecated and there is a chance that certain JS engines don't support this property anymore. `Object.getPrototypeOf()` and `Object.setPrototypeOf()` are the function which now should be used in order to retrieve a prototype.
>
> ```js
> // DON'T: Old method using __proto__ deprecated!
> console.log(dog.__proto__);
> 
> // DO: Using the newer getPrototypeOf function
> console.log(Object.getPrototypeOf(dog));
> ```
> **One difference is** that `__proto__` can be changed (a bad design practice though) while `getPrototypeOf` is a read only function.




More info - [Prototypes in JavaScript](https://medium.com/better-programming/prototypes-in-javascript-5bba2990e04b) on medium



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



## Creation Patterns

Object creation patterns.

### Factory Functions

With the factory pattern, we create a factory that creates specified objects and returns their reference. Every time we call the factory we get a new instance.

```javascript
//Factory pattern for object Creation
var computerFactory = function(ram, hardDisk) {
  var computer = {}; //creating a new temporary object
  //Create class properties
  computer.ram = ram;
  computer.hardDisk = hardDisk;
  //Create class methods
  computer.AvailableMemory = function() {
    console.log('Hard-disk : ' + this.hardDisk);
    console.log('Ram : ' + this.ram);
  };
  return computer;
};
```
Now we create an object by calling the factory, like this 

```js
//Creating new object(s) for computer class by using computerFactory
var computer1 = computerFactory(4,512);
var computer2 = computerFactory(16,1024);

//Accessing class methods using objects
computer1.AvailableMemory();
computer2.AvailableMemory();

//Output
// Hard-disk : 512
// Ram : 4
// Hard-disk : 1024
// Ram : 16
```



### Constructor Pattern

With the constructor pattern we don’t return the instance from the function — instead, **we use the new operator along with the function name**.




```js
var computer = function(ram, hardDisk) {
  //Create class properties
  this.ram = ram;
  this.hardDisk = hardDisk;
  //Create class methods
  this.AvailableMemory = function() {
    console.log('Hard-disk : ' + this.hardDisk);
    console.log('Ram : ' + this.ram);
  };
};
```

Note: we’re not returning the object from the computer constructor.

```js
//Creating new object(s) for computer class by using constructor pattern
var computer1 = new computer(4,512);
var computer2 = new computer(16,1024);
//Accessing object's properties
computer1.AvailableMemory();
computer2.AvailableMemory();

//Output
// Hard-disk : 512
// Ram : 4
// Hard-disk : 1024
// Ram : 16
```



### Prototype Pattern

In a prototype pattern, we create a blank object and assign properties (and functions) to its prototype, with some default values. Then we create a blank object and assign the actual values for the properties.

```js
var computer = function() {};
computer.prototype.ram = NaN;
computer.prototype.hardDisk = NaN;
computer.prototype.AvailableMemory = function() {
  console.log('Hard-disk : ' + this.hardDisk);
  console.log('Ram : ' + this.ram);
};
//Creating new object(s) for computer class
// by using Prototype Pattern
var computer1 = new computer(); 
//Empty object created with default values
computer1.ram = 4; //Assigning the actual value for object property
computer1.hardDisk = 512;
var computer2 = new computer();
computer2.ram = 16;
computer2.hardDisk = 1024;
//Accessing class methods using objects
computer1.AvailableMemory();
computer2.AvailableMemory();

//Output
// Hard-disk : 512
// Ram : 4
// Hard-disk : 1024
// Ram : 16
```

**Note -** Every function has the "prototype" property even if we don’t supply it. The default "prototype" is an object with the only property constructor that points back to the function itself.

```javascript
function Rabbit() {}

/* default prototype
Rabbit.prototype = { constructor: Rabbit };
*/
```

