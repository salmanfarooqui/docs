# Array

Arrays are a special type of objects. The typeof operator in JavaScript returns "object" for arrays.

There are two syntaxes for creating an empty array:

```js
let arr = new Array();
let arr = [];
```

Almost all the time, the second syntax is used. 

<br>

Array elements are numbered, starting with zero. We can get an element by its number in square brackets:

```javascript
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits[0] ); // Apple
alert( fruits[1] ); // Orange
```

We can replace an element:

```javascript
fruits[2] = 'Pear'; // now ["Apple", "Orange", "Pear"]
```

…Or add a new one to the array:

```javascript
fruits[3] = 'Lemon'; // now ["Apple", "Orange", "Pear", "Lemon"]
```

The total count of the elements in the array is its length:

```javascript
let fruits = ["Apple", "Orange", "Plum"];
alert( fruits.length ); // 3
```

Get last element in an array:

```js
fruits[fruits.length - 1];  // Plum
```



## Iterate

### *for..of* cycle

`for(const item of items)` cycle iterates over array items.

Let’s iterate over a list of `colors`:

```javascript
const colors = ['blue', 'green', 'white'];

for (const color of colors) {
  console.log(color);
}
// 'blue'
// 'green'
// 'white'
```

On each iteration, the variable `color` is assigned with the iterated item.

Tips:

- You can stop iterating at any time using a `break` statement.

### *for* cycle

`for(let i; i < array.length; i++)` cycle iterates over array items using an incrementing index variable.

`for` usually requires `index` variable that increments on each cycle:

```javascript
const colors = ['blue', 'green', 'white'];

for (let index = 0; index < colors.length; index++) {
  const color = colors[index];
  console.log(color);
}
// 'blue'
// 'green'
// 'white'
```

`index` variable increments from `0` until `colors.length - 1`. This variable is used to access the item by index: `colors[index]`.

Tips:

- You can stop iterating at any time using a `break` statement.

### *array.forEach()* method

`array.forEach(callback)` method iterates over array items by invoking `callback` function on every array item.

On each iteration `callback(item [, index [, array]])` is called with arguments: iterated item, index and the array itself.

Let’s iterate over `colors` array:

```javascript
const colors = ['blue', 'green', 'white'];

colors.forEach(function callback(value, index) {
  console.log(value, index);
});
// 'blue', 0
// 'green', 1
// 'white', 2
```

`array.forEach(callback)` invokes `callback` 3 times for every item in the array: `'blue'`, `'green'` and `'white'`.

Tips:

- You cannot break `array.forEach()` iterating.

## Map

### *array.map()* method

`array.map(callback)` method creates to a new array by using `callback` invocation result on each array item.

On each iteration `callback(item[, index[, array]])` is invoked with arguments: current item, index and the array itself. It should return the new item.

Let’s increment the numbers of an array:

```javascript
const numbers = [0, 2, 4];

const newNumbers = numbers.map(function increment(number) {
  return number + 1;
});

newNumbers; // => [1, 3, 5]
```

`numbers.map(increment)` creates a new array from `numbers` by incrementing each array item.

Tips:

- `array.map()` creates a new mapped array, without mutating the original one.

### *Array.from()* function

`Array.from(arrayLike[, callback])` method creates to a new array by using `callback` invocation result on each array item.

On each iteration `callback(item[, index[, array]])` is invoked with arguments: current item, index and the array itself. It should return the new item.

Let’s increment the numbers of an array:

```javascript
const numbers = [0, 2, 4];

const newNumbers = Array.from(numbers,
  function increment(number) {
    return number + 1;
  }
);

newNumbers; // => [1, 3, 5]
```

`Array.from(numbers, increment)` creates a new array from `numbers` by incrementing each array item.

Tips:

- `Array.from()` creates a new mapped array, without mutating the original one
- `Array.from()` fits better to map from an [array-like object](https://dmitripavlutin.com/javascript-array-from-applications/#2-transform-array-like-into-an-array).

## Reduce

### *array.reduce()* method

`array.reduce(callback[, initialValue])` reduces the array to a value by invoking `callback` function as a reducer.

On each iteration `callback(accumulator, item[, index[, array]])` is invoked with arguments: accumulator, current item, index and the array itself. It should return the accumulator.

The classic example is summing an array of numbers:

```javascript
const numbers = [2, 0, 4];

function summarize(accumulator, number) {
  return accumulator + number;
}

const sum = numbers.reduce(summarize, 0);

sum; // => 6
```

At first step `accumulator` is initialized with `0`. Then `summarize` function is invoked on each array item accumulating the sum of numbers.

Tips:

- The first array item becomes the initial value if you skip the `initialValue` argument.

## Concat

### *array.concat()* method

`array.concat(array1[, array2, ...])` concatenates to the original array one or more arrays.

Let’s concatenate 2 arrays of names:

```javascript
const heroes = ['Batman', 'Robin'];
const villains = ['Joker', 'Bane'];

const everyone = heroes.concat(villains);

everyone; // => ['Batman', 'Robin', 'Joker', 'Bane']
```

`heroes.concat(villains)` creates a new array by concatenating `heroes` and `villains` arrays.

Tips:

- `array.concat()` creates a new array, without mutating the original one
- `array.concat(array1[, array2, ...])` accepts multiple arrays to concat.

### Spread operator

You can use the spread operator with an array literal to concatenate arrays: `[...array1, ...array2]`.

Let’s concatenate 2 arrays of names:

```javascript
const heroes = ['Batman', 'Catwoman'];
const villains = ['Joker', 'Bane'];

const names = [...heroes, ...villains];

names; // => ['Batman', 'Catwoman', 'Joker', 'Bane']
```

`[...heroes, ...villains]` spreads `heroes` and `villains` items, then creates a new array containing all spread items.

- `[...arr1, ...arr2, ...arrN]`: you can concat as many arrays as you need using spread operator.

## Slice

### *array.slice()* method

`array.slice([fromIndex[, toIndex]])` returns a slice of the array starting `fromIndex` and ending `toIndex` (excluding `toIndex` itself). `fromIndex` optional argument defaults to `0`, `toIndex` optional argument defaults to `array.length`.

Let’s get some array slices:

```javascript
const names = ['Batman', 'Catwoman', 'Joker', 'Bane'];

const heroes = names.slice(0, 2);
const villains = names.slice(2);

heroes; // => ['Batman', 'Catwoman']
villains; // => ['Joker', 'Bane']
```

`names.slice(0, 2)` returns a slice of 2 items from `names` array.

`names.slice(2)` returns a slice of 2 items. The `end` argument defaults to `names.length`.

Tips:

- `array.slice()` creates a new array, without mutating the original one.

## Clone

### Spread operator

An easy way to clone an array is to use the spread operator: `const clone = [...array]`;

Let’s clone an array of `colors`:

```javascript
const colors = ['white', 'black', 'gray'];

const clone = [...colors];

clone; // => ['white', 'black', 'gray']
colors === clone; // => false
```

`[...colors]` creates a clone of `colors` array.

Tips:

- `[...array]` creates a shallow copy.

### *array.concat()* method

`[].concat(array)` is yet another approach on how to clone `array`.

```javascript
const colors = ['white', 'black', 'gray'];

const clone = [].concat(colors);

clone; // => ['white', 'black', 'gray']
colors === clone; // => false
```

`[].concat(colors)` creates a clone of `colors` array.

Tips:

- `[].concat(array)` creates a shallow copy.

### *array.slice()* method

`array.slice()` is another approach on how to clone `array`.

```javascript
const colors = ['white', 'black', 'gray'];

const clone = colors.slice();

clone; // => ['white', 'black', 'gray']
colors === clone; // => false
```

`colors.slice()` creates a clone of `colors` array.

Tips:

- `colors.slice()` creates a shallow copy.

## Search

### *array.includes()* method

`array.includes(itemToSearch[, fromIndex])` returns a boolean whether `array` contains `itemToSearch`. The optional argument `fromIndex`, defaulting to `0`, indicates the index to start searching.

Let’s determine if `2` and `99` exist in an array of numbers:

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.includes(2);  // => true
numbers.includes(99); // => false
```

`numbers.includes(2)` returns `true` because `2` exists in `numbers` array.

`numbers.includes(99)` is, however, `false` because `numbers` doesn’t contain `99`.

### *array.find()* method

`array.find(predicate)` method returns the first array item that satisfies the `predicate` function.

On each iteration `predicate(item[, index[, array]])` function is invoked with the arguments: iterated item, index and the array itself.

For example, let’s find the first even number:

```javascript
const numbers = [1, 2, 3, 4, 5];

function isEven(number) {
  return number % 2 === 0;
}

const evenNumber = numbers.find(isEven);

evenNumber; // => 2
```

`numbers.find(isEven)` returns the first even number inside `numbers`, which is `2`.

Tips:

- `array.find()` returns `undefined` if no item has satisfied the predicate.

### *array.indexOf()* method

`array.indexOf(itemToSearch[, fromIndex])` returns the index of the first appearance `itemToSearch` in `array`. The optional argument `fromIndex`, defaulting to `0`, is the index to start searching.

Let’s find the index of `'Joker'`:

```javascript
const names = ['Batman', 'Catwoman', 'Joker', 'Bane'];

const index = names.indexOf('Joker');

index; // => 2
```

The index of `'Joker'` inside `names` is `2`.

Tips:

- `array.indexOf(itemToSearch)` returns `-1` if the item hasn’t been found
- `array.findIndex(predicate)` is an alternative to find the index using a predicate function.

## Query

### *array.every()* method

`array.every(predicate)` method returns `true` if every item passes `predicate` check.

On each iteration `predicate(item[, index[, array]])` predicate function is invoked with the arguments: iterated item, index and the array itself.

Let’s determine whether arrays `evens` and `mix` contain only even numbers:

```javascript
const evens = [0, 2, 4, 6];
const numbers = [0, 1, 4, 6];

function isEven(number) {
  return number % 2 === 0;
}

evens.every(isEven); // => true
numbers.every(isEven); // => false
```

`evens.every(isEven)` is `true` because *all* numbers in `evens` are even.

However, `numbers.every(isEven)` evaluates to `false` because `numbers` contains an odd number `1`.

### *array.some()* method

`array.some(predicate)` method returns `true` if at least one item passes `predicate` check.
On each iteration `predicate(item[, index[, array]])` function is invoked with the arguments: iterated item, index and the array itself.

Let’s determine whether the arrays contain at least one even number:

```javascript
const numbers = [1, 5, 7, 10];
const odds = [1, 3, 3, 3];

function isEven(number) {
  return number % 2 === 0;
}

numbers.some(isEven); // => true
odds.some(isEven);   // => false
```

`numbers.some(isEven)` is `true` because at least one even number, `10`, exists in `numbers`.

But `odds.some(isEven)` is `false` because `odds` contains only odd numbers.

## Filter

### *array.filter()*

`array.filter(predicate)` method returns a new array with items that have passed `predicate` check.

On each iteration `predicate(item[, index[, array]])` function is invoked with the arguments: iterated item, index and the array itself.

Let’s filter an array to have only even numbers:

```javascript
const numbers = [1, 5, 7, 10];

function isEven(number) {
  return number % 2 === 0;
}

const evens = numbers.filter(isEven);

evens; // => [10]
```

`numbers.filter(isEven)` creates a new array `evens` by filtering `numbers` to contain only even numbers.

Tips:

- `array.filter()` creates a new array, without mutating the original one.

## Insert

### *array.push()* method

`array.push(item1[..., itemN])` method appends one or more items to the end of an array, returning the new length.

Let’s append `'Joker'` at the end of `names` array:

```javascript
const names = ['Batman'];

names.push('Joker');

names; // ['Batman', 'Joker']
```

`names.push('Joker')` inserts a new item `'Joker'` at the end of the `names` array.

Tips:

- `array.push()` mutates the array in place
- `array.push(item1, item2, ..., itemN)` can push multiple items.

### *array.unshift()* method

`array.unshift(item1[..., itemN])` method appends one or more items to the beginning of an array, returning the new length of the array.

Let’s append `'Catwoman'` at the beginning of `names` array:

```javascript
const names = ['Batman'];

names.unshift('Catwoman');

names; // ['Catwoman', 'Batman']
```

`names.unshift('Catwoman')` inserts a new item `'Catwoman'` at the beginning of `names` array.

Tips:

- `array.unshift()` mutates the array in place.
- `array.unshift(item1, item2, ..., itemN)` can insert multiple items.

### Spread operator

You can insert items in an array in an immutable manner by combining the spread operator with the array literal.

Appending an item at the *end of an array*:

```javascript
const names = ['Joker', 'Bane'];

const names2 = [
  ...names,
  'Batman',
];

names2; // => ['Joker', 'Bane', 'Batman'];
```

Appending an item at the *beginning of an array*:

```javascript
const names = ['Joker', 'Bane'];

const names2 = [
  'Batman',
  ...names
];

names2; // => ['Batman', 'Joker', 'Bane'];
```

Inserting an item *at any index*:

```javascript
const names = ['Joker', 'Bane'];
const indexToInsert = 1;

const names2 = [
  ...names.slice(0, indexToInsert),
  'Batman',
  ...names.slice(indexToInsert)
];

names2; // => ['Joker', 'Batman', 'Bane'];
```

## Remove

### *array.pop()* method

`array.pop()` method removes the last item from an array, then returns that item.

For example, let’s remove the last element of `colors` array:

```javascript
const colors = ['blue', 'green', 'black'];

const lastColor = colors.pop();

lastColor; // => 'black'
colors; // => ['blue', 'green']
```

`colors.pop()` removes the last element of `colors` and returns it.

Tips:

- `array.pop()` mutates the array in place.

### *array.shift()* method

`array.shift()` method removes the first item from an array, then returns that item.

For example, let’s remove the first element of `colors` array:

```javascript
const colors = ['blue', 'green', 'black'];

const firstColor = colors.shift();

firstColor; // => 'blue'
colors; // => ['green', 'black']
```

`colors.shift()` removes the first element `'blue'` of `colors` and returns it.

Tips:

- `array.shift()` mutates the array in place
- `array.shift()` has O(n) complexity.

### *array.splice()* method

`array.splice(fromIndex[, removeCount[, item1[, item2[, ...]]]])` removes items from an array and inserts new items instead.

For example, let’s remove 2 items from index `1`:

```javascript
const names = ['Batman', 'Catwoman', 'Joker', 'Bane'];

names.splice(1, 2);

names; // => ['Batman', 'Bane']
```

`names.splice(1, 2)` removes the elements `'Catwoman'` and `'Joker'`.

`names.splice()` can insert new items instead of removed ones. Let’s replace 2 items from index `1`, and insert a new item `'Alfred'` instead:

```javascript
const names = ['Batman', 'Catwoman', 'Joker', 'Bane'];

names.splice(1, 2, 'Alfred');

names; // => ['Batman', 'Alfred' ,'Bane']
```

Tips:

- `array.splice()` mutates the array in place.

### Spread operator

You can remove items from an array in an immutable manner by combining the spread operator with the array literal.

Let’s remove a few items:

```javascript
const names = ['Batman', 'Catwoman', 'Joker', 'Bane'];
const fromIndex = 1;
const removeCount = 2;

const newNames = [
  ...names.slice(0, fromIndex),
  ...names.slice(fromIndex + removeCount)
];

newNames; // => ['Batman', 'Bane']
```

`newNames` contains the items of `names`, but without 2 that were removed.

## Empty

### *array.length* property

`array.length` is a property that holds the array length. More than that, `array.length` is writable.

If you write a smaller than current length `array.length = newLength`, the extra elements are removed from the array.

Let’s use `array.length = 0` to remove all the items of an array:

```javascript
const colors = ['blue', 'green', 'black'];

colors.length = 0;

colors; // []
```

`colors.length = 0` removes all items from `colors` array.

### *array.splice()* method

`array.splice(fromIndex[, removeCount[, item1[, item2[, ...]]]])` removes items from an array and inserts new items instead.

If `removeCount` argument is omitted, then `array.splice()` removes all elements of the array starting `fromIndex`.

Let’s use this to remove all elements of an array:

```javascript
const colors = ['blue', 'green', 'black'];

colors.splice(0);

colors; // []
```

`colors.splice(0)` removes all elements of `colors` array.

## Fill

### *array.fill()* method

`array.fill(value[, fromIndex[, toIndex]])` fills the array with `value` starting `fromIndex` until `toIndex` (excluding `toIndex` itself). `fromIndex` optional argument defaults to `0`, `toIndex` optional argument defaults to `array.length`.

For example, let’s fill an array with zero values:

```javascript
const numbers = [1, 2, 3, 4];

numbers.fill(0);

numbers; // => [0, 0, 0, 0]
```

`numbers.fill(0)` fills the array with zeros.

More than that, you can initialize arrays of specific length and initial value using `Array(length).fill(initial)`:

```javascript
const length = 3;
const zeros = Array(length).fill(0);

zeros; // [0, 0, 0]
```

`Array(length).fill(0)` creates an array of 3 zeros.

Tips:

- `array.fill()` mutates the array in place.

### *Array.from()* function

`Array.from()` can be useful to initialize an array of certain length with objects:

```javascript
const length = 4;
const emptyObjects = Array.from(Array(length), function() {
  return {};
});

emptyObjects; // [{}, {}, {}, {}]
```

`emptyObjects` is an array initialized with different instances of empty objects.

## Flatten

### *array.flat()* method

`array.flat([depth])` method creates a new array by recursively flatting the items that are arrays, until certain `depth`. `depth` optional argument defaults to `1`.

Let’s flatten an array of arrays:

```javascript
const arrays = [0, [1, 3, 5], [2, 4, 6]];

const flatArray = arrays.flat();

flatArray; // [0, 1, 3, 5, 2, 4, 6]
```

`arrays` contains a mix of numbers and arrays of numbers. `arrays.flat()` flats the array so that it contains only numbers.

Tips:

- `array.flat()` creates a new array, without mutating the original one.

## Sort

### *array.sort()* method

`array.sort([compare])` method sorts the items of the array. When the compare function is omitted, the method converts the items to strings, then orders them ascending by UTF-16 code units values.

The optional argument `compare(item1, item2)` is a callback that customizes the order of items. If `compare(item1, item2)` returns:

- `-1` then `item1` will follow by `item2` in the sorted array
- `1` then `item2` will follow by `item1` in the sorted array
- `0` then the position of items doesn’t change

Let’s sort an array of letters:

```javascript
const letters = ['B', 'C', 'A'];

letters.sort();

letters; // => ['A', 'B', 'C']
```

`letters.sort()` sorts the letters in ascending order.

Let’s use the compare function and make even numbers followed by odd ones:

```javascript
const numbers = [4, 3, 1, 2];

function compare(n1, n2) {
  if (n1 % 2 === 0 && n2 % 2 !== 0) {
    return -1;
  }
  if (n1 % 2 !== 0 && n2 % 2 === 0) {
    return -1;
  }
  return 0;
}

numbers.sort(compare);

numbers; // => [4, 2, 3, 1]
```

`numbers.sort(compare)` uses the custom compare function that orders even numbers first.

Tips:

- `array.sort()` mutates the array in place.



## map vs forEach

The important difference between them is that **map accumulates all of the results into a collection**, whereas **foreach returns nothing**. map is usually used when you want to transform a collection of elements with a function, whereas foreach simply executes an action for each element. In short, foreach is for applying an operation on each element of a collection of elements, whereas map is for transforming one collection into another.

foreach has no conceptual restrictions on the operation it applies, other than perhaps accept an element as argument. That is, the operation may do nothing, may have a side-effect, may return a value or may not return a value. All foreach cares about is to iterate over a collection of elements, and apply the operation on each element.

map, on the other hand, does have a restriction on the operation: it expects the operation to return an element.



## CheatSheet



```js
list = [a,b,c,d,e]
list[1]                 // → b
list.indexOf(b)         // → 1
```

#### **Adding items**

```js
// Mutative
list.push(X)            // list == [_,_,_,_,_,X]
list.unshift(X)         // list == [X,_,_,_,_,_]
list.splice(2, 0, X)    // list == [_,_,X,_,_,_]

// Immutable
list.concat([X,Y])      // → [_,_,_,_,_,X,Y]
```

#### **Replace items**

```js
list.splice(2, 1, X)    // list == [a,b,X,d,e]
```

#### **Iterables**

```js
.filter(n => ...) => array
        
.find(n => ...)  // es6
.findIndex(...)  // es6
           
.every(n => ...) => Boolean // ie9+
.some(n => ..) => Boolean   // ie9+

.map(n => ...)   // ie9+
.reduce((total, n) => total) // ie9+
.reduceRight(...)
```

#### **Subsets**

```js
// Immutable
list.slice(0,1)         // → [a        ]
list.slice(1)           // → [  b,c,d,e]
list.slice(1,2)         // → [  b      ]

// Mutative
re = list.splice(1)     // re = [b,c,d,e]  list == [a]
re = list.splice(1,2)   // re = [b,c]      list == [a,d,e]
```

#### **Inserting**

```js
// after -- [_,_,REF,NEW,_,_]
list.splice(list.indexOf(REF)+1, 0, NEW))

// before -- [_,_,NEW,REF,_,_]
list.splice(list.indexOf(REF), 0, NEW))
```

#### **Removing items**

```js
list.pop()              // → e    list == [a,b,c,d]
list.shift()            // → a    list == [b,c,d,e]
list.splice(2, 1)       // → [c]  list == [a,b,d,e]
```