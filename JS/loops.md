# Loops



## for Loop

```js
for (initialize; condition; increment) single statement;
for (initialize; condition; increment) { multiple; statements; }
```

A for loop declares looping instructions, with three important pieces of information separated by semicolons ;:

- The *initialization* defines where to begin the loop by declaring (or referencing) the iterator variable
- The stopping condition determines when to stop looping (when the expression evaluates to false)
- The *iteration statement* updates the iterator each time the loop is completed

```js
for (let i = 0; i < 4; i += 1) {  
console.log(i); 
}; 
// Output: 0, 1, 2, 3
```



#### Reverse Loop

A for loop can iterate “in reverse” by initializing the loop variable to the starting value, testing for when the variable hits the ending value, and decrementing (subtracting from) the loop variable at each iteration.

```js
const items = ['apricot', 'banana', 'cherry'];

for (let i = items.length - 1; i >= 0; i -= 1) {
  console.log(`${i}. ${items[i]}`);
}

// Prints: 2. cherry
// Prints: 1. banana
// Prints: 0. apricot
```



#### Looping Through Arrays

An array’s length can be evaluated with the .length property. This is extremely helpful for looping through arrays, as the .length of the array can be used as the stopping condition in the loop.

```js
for (let i = 0; i < array.length; i++){
  console.log(array[i]);
}
// Output: Every item in the array
```



#### Breaking Early

You can break out of a for loop by using the **break** keyword:

```js
for (let i = 0;; i++) { 
    console.log("loop"); 
    break; 
};
```

Note the condition statement was skipped here. The loop will break by an explicit command from the statement itself.

In this case the for loop will print loop to the console only once. The break keyword simply exits the loop whenever it’s encountered.



#### Skipping Steps

You can skip an iteration step by using the **continue** keyword:

```js
for (let i = 0; i < 3; i++) { 
    if (i == 1) continue; 
    console.log( i ); 
}
// 0
// 2
```

The continue keyword tells code flow to go to the next step without executing any next statements in this for-loop’s scope during the current iteration step.



#### Multiple Statements

*Multiple statements* can be separated by commas. In the following example, the **inc()** function is used to increment the value of a global variable **counter.** Note the combination of the the two statements: **i++, inc()**:

```js
let counter = 0;
function inc() { counter++; }

for (let i = 0; i < 10; i++, inc());
console.log(counter); // 10
```

Same result as  let counter = 0; for (let i = 0; i < 10; i++) counter++;



> Bracket-less for-loop syntax is not good friends with the **let** keyword. The following code will generate an error: `for (var i = 0; i < 10; i++) let x = i;`
>
> 
>
> A let variable cannot be defined without scope brackets. All variables defined using the let keyword *require* their own local scope. This is fixed by: `for (var i = 0; i < 10; i++) { let x = i; }`
>



## for...of

Use for…of to iterate over the values in an **iterable, like an array**. We can iterate through it without having to create index variables. 

```js
let array = [0, 1, 2];
for (let value of array)
   console.log( value );

// 0
// 1
// 2
```

**Strings are also an iterable type**, so you can use for…of on strings.

```js
let string = 'text';

for (let value of string)
  console.log( value );
// 't'
// 'e'
// 'x'
// 't'
```

You can also iterate over maps, sets, generators, DOM node collections and the arguments object available inside a functions.  An **object is not an iterable so for...of won't work** with objects.



## for...in

Use for…in **to iterate over the properties of an object**. It will display only *enumerable* object properties.

```js
let object = { a: 1, b: 2, method: () => { } };

for (let key in object)
    
console.log(key);
// a
// b
// method

console.log(object[key]);
// 1
// 2
// () => { }
```

You can **also use for…in to iterate over the index values of an iterable like an array or a string:**

```js
let str = 'Turn the page';

for (let index in str) {
  console.log(`Index of ${str[index]}: ${index}`);
}

// Index of T: 0
// Index of u: 1
// and so on.. Index of e: 12
```




## While Loop

The while loop creates a loop that is **executed as long as a specified condition evaluates to true**. The loop will continue to run until the condition evaluates to false.

```js
let i = 0;

while (i < 5) {        
  console.log(i);
  i++;
}
```

A secondary condition can be tested within the loop. This **makes it possible to break from the loop earlier if needed**:

```js
while (condition_1) {
    if (condition_2)
        break;
}
```



## Do…While Statement

A do...while statement creates a loop that executes a block of code once, checks if a condition is true, and then repeats the loop as long as the condition is true. They are used when you want the code to always execute at least once. The loop ends when the condition evaluates to false.

```js
x = 0
i = 0

do {
  x = x + i;
  console.log(x)
  i++;
} while (i < 5);

// Prints: 0 1 3 6 10
```



## Break

Within a loop, the break keyword may be **used to exit the loop immediately**. Here, the break keyword is used to exit the loop when i is greater than 5.

```js
for (let i = 0; i < 99; i += 1) {
  if (i > 5) {
     break;
  }
  console.log(i)
}

// Output: 0 1 2 3 4 5
```

<br>

The break statement, **without a label reference**, can only be used to jump out of a loop or a switch. **With a label reference**, the break statement can be used to jump out of any code block:

```js
var cars = ["BMW", "Volvo", "Saab", "Ford"];
var text = "";

list: {
  text += cars[0]; 
  text += cars[1]; 
  break list;
  text += cars[2]; 
  text += cars[3]; 
}
console.log(text)

// BMWVolvo
```





## Continue

The **continue statement** terminates execution of the statements in the current iteration of the loop, and continues execution of the loop with the next iteration. The **continue keyword can be used to skip steps.**

```js
let text = '';

for (let i = 0; i < 10; i++) {
  if (i === 3) {
    continue;
  }
  text = text + i;
}

console.log(text);
//  "012456789"
```



```js
let c = 0;
while (c++ < 1000) {
    if (c > 1)
        continue;
    console.log(c);
}
// 1
```

The continue statement (with or without a label reference) can only be used to skip one loop iteration.