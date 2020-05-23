# JS Debugger

In `hello.js`, click at line number `4` and `8`.  Congratulations! You’ve set a breakpoint. 

![debugging01](https://docs.salmanfarooqui.com/JS/images/debugging01.png)

A *breakpoint* is a point of code where the debugger will automatically pause the JavaScript execution.

While the code is paused, we can examine current variables, execute commands in the console etc. In other words, we can debug it.

We can also pause the code by using the `debugger` command in it, like this:

```javascript
function hello(name) {
  let phrase = `Hello, ${name}!`;
  debugger;  // <-- the debugger stops her
  say(phrase);
}
```

That’s very convenient when we are in a code editor and don’t want to switch to the browser and look up the script in developer tools to set the breakpoint.

<br>

`hello()` is called during the page load, so the easiest way to activate the debugger (after we’ve set the breakpoints) is to reload the page. As the breakpoint is set, the execution pauses at the 4th line.

![debugger02](https://docs.salmanfarooqui.com/JS/images/debugger02.png)



1. **`Watch` – shows current values for any expressions.**

   The **Watch Expressions** tab lets you monitor the values of variables over time. Watch Expressions aren't just limited to variables. You can store any valid JavaScript expression in a Watch Expression. 

   1. Click the **Watch** tab.
   2. Click **Add Expression** ![Add Expression](https://developers.google.com/web/tools/chrome-devtools/javascript/imgs/add-expression.png).
   3. Type `typeof sum`.
   4. Press Enter. DevTools shows `typeof sum: "string"`. The value to the right of the colon is the result of your Watch Expression.

   

   ![debugger02](https://docs.salmanfarooqui.com/JS/images/debugger04.png)

   

2. **`Call Stack` – shows the nested calls chain.**

   At the current moment the debugger is inside `hello()` call, called by a script in `index.html` (no function there, so it’s called “anonymous”).

   If you click on a stack item (e.g. “anonymous”), the debugger jumps to the corresponding code, and all its variables can be examined as well.

3. **`Scope` – current variables.**

   `Local` shows local function variables. You can also see their values highlighted right over the source.

   `Global` has global variables (out of any functions).

   There’s also `this` keyword there that we didn’t study yet, but we’ll do that soon.

<br>

Now it’s time to *trace* the script.

### Resume

continue the execution, hotkey F8.

Resumes the execution. If there are no additional breakpoints, then the execution just continues and the debugger loses control.

![debugger03](https://docs.salmanfarooqui.com/JS/images/debugger03.png)



### Step

run the next command, hotkey F9.

Run the next statement. If we click it now, `alert` will be shown.

Clicking this again and again will step through all script statements one by one.



### Step over

run the next command, but *don’t go into a function*, hotkey F10.

Similar to the previous the “Step” command, but behaves differently if the next statement is a function call. That is: not a built-in, like `alert`, but a function of our own.

The “Step” command goes into it and pauses the execution at its first line, while “Step over” executes the nested function call invisibly, skipping the function internals.

The execution is then paused immediately after that function.

That’s good if we’re not interested to see what happens inside the function call.



### Step into 

hotkey F11.

That’s similar to “Step”, but behaves differently in case of asynchronous function calls. “Step” command ignores async actions, such as `setTimeout` (scheduled function call), that execute later. The “Step into” goes into their code, waiting for them if necessary. 



### Step out

continue the execution till the end of the current function, hotkey Shift+F11.

Continue the execution and stop it at the very last line of the current function. That’s handy when we accidentally entered a nested call using , but it does not interest us, and we want to continue to its end as soon as possible.



### enable/disable

That button does not move the execution. Just a mass on/off for breakpoints. 

### || (pause on exception)

enable/disable automatic pause in case of an error.

When enabled, and the developer tools is open, a script error automatically pauses the execution. Then we can analyze variables to see what went wrong. So if our script dies with an error, we can open debugger, enable this option and reload the page to see where it dies and what’s the context at that moment.

<br>

> As we can see, there are three main ways to pause a script:
>
> 1. A breakpoint.
> 2. The `debugger` statements.
> 3. An error (if dev tools are open and the `||` button is “on”).
>
> When paused, we can debug – examine variables and trace the code to see where the execution goes wrong.



## performance

Measure code performance

```js
// Take a timestamp at the beginning.
var start = performance.now();

// Execute your code here 

// Take a final timestamp.
var end = performance.now();

// Calculate the time taken and output the result in the console
console.log(`${end - start} miliseconds to run`);
```



## try...catch

The **`try...catch`** statement marks a block of statements *to try* and specifies a response should an exception be thrown. It allows us to “catch” errors so the script can, instead of dying, do something more reasonable.

```javascript
try {
  // code...
} catch (err) {
  // error handling
}
```

It works like this -

1. First, the code in `try {...}` is executed.
2. If there were **no errors, then `catch(err)` is ignored**.
3. If an **error occurs, then the `try` execution is stopped(rest of try is ignored), and control flows to the beginning of `catch(err)`**. The `err` variable will contain an error object with details about what happened.

```javascript
try {
  alert('Start of try runs');  
  lalala; // error, variable is not defined!
  alert('End of try (never reached)'); 
} catch(err) {
  alert(`Error has occurred!`); 
}
// Start of try runs
// Error has occured!
```



### try...catch works synchronously

If an exception happens in “scheduled” code, like in `setTimeout`, then `try..catch` won’t catch it.

```javascript
try {
  setTimeout(function() {
    noSuchVariable; // script will die here
  }, 1000);
} catch (e) {
  alert( "won't work" );
}
```

That’s because the function itself is executed later, when the engine has already left the try..catch construct. To catch an exception inside a scheduled function, try..catch must be inside that function.

```javascript
setTimeout(function() {
  try {
    noSuchVariable; // try..catch handles the error!
  } catch {
    alert( "error is caught here!" );
  }
}, 1000);
```

> If we don’t need error details, `catch` may omit it (recent addition. Older browser may need polyfill)
>
> ```javascript
>try {
> // ...
> } catch { // <-- without (err)
>   // ...
> }
>   ```



### Throwing error

The throw operator generates an error. `throw <error object>`

Technically, we can use anything as an error object. Even a primitive, like a number or a string, but it’s better to use objects, preferably with `name` and `message` properties (to stay somewhat compatible with built-in errors).

JavaScript has many built-in constructors for standard errors: `Error`, `SyntaxError`, `ReferenceError`, `TypeError` and others. We can use them to create error objects as well.

```javascript
let error = new Error(message);
// or
let error = new SyntaxError(message);
let error = new ReferenceError(message);
```

For built-in errors, the `name` property is exactly the name of the constructor. And `message` is taken from the argument.

```javascript
let error = new Error("Things happen o_O");
alert(error.name); // Error
alert(error.message); // Things happen o_O
```

**Example**

```javascript
let json = '{ "age": 30 }'; // incomplete data

try {
  let user = JSON.parse(json); // <-- no errors
  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }
  alert( user.name );
} catch(e) {
  alert( "JSON Error: " + e.message ); // JSON Error: Incomplete data: no name
}
```

The `throw` operator generates a `SyntaxError` with the given `message`, the same way as JavaScript would generate it itself. The execution of `try` immediately stops and the control flow jumps into `catch`.



### Rethowing

In previous example, try..catch is placed to catch “incorrect data” errors. But by its nature, `catch` gets *all* errors from `try`, like if we forgot to put let before user, `user = JSON.parse(json);`. Here it gets an unexpected error, but still shows the same `"JSON Error"` message. That’s wrong and also makes the code more difficult to debug. 

To avoid such problems, we can employ the “rethrowing” technique. **Catch should only process errors that it knows and “rethrow” all others.** Basically, in the catch block we analyze the error object `err` and if we don't know how to handle it we do `throw err`

In the code below, we use rethrowing so that `catch` only handles `SyntaxError`

```javascript
let json = '{ "age": 30 }'; // incomplete data
try {
  let user = JSON.parse(json);
  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }
  blabla(); // unexpected error
  alert( user.name );
} catch(e) {
  if (e instanceof SyntaxError) { 
    alert( "JSON Error: " + e.message );
  } else {
    throw e; // rethrow (*)
  }
}
```

**We can check the error type using the `instanceof` operator.** 

The error throwing on line `(*)` from inside catch block “falls out” of try..catch and can be either caught by an outer `try..catch` construct (if it exists), or it kills the script.

So the `catch` block actually handles only errors that it knows how to deal with and “skips” all others.

The example below demonstrates how such errors can be caught by one more level of `try..catch`

```javascript
function readData() {
  let json = '{ "age": 30 }';
  try {
    // ...
    blabla(); // error!
  } catch (e) {
    // ...
    if (!(e instanceof SyntaxError)) {
      throw e; // rethrow (don't know how to deal with it)
    }
  }
}
try {
  readData();
} catch (e) {
  alert( "External catch got: " + e ); // caught it!
}
```

Here `readData` only knows how to handle `SyntaxError`, while the outer `try..catch` knows how to handle everything.



### finally

The try..catch construct may have one more code clause: finally. If it exists, **it runs in all cases**.

```javascript
try {
   ... try to execute the code ...
} catch(e) {
   ... handle errors ...
} finally {
   ... execute always ...
}
```

The finally clause is often used when we start doing something and want to finalize it in any case of outcome. The `finally` clause works for *any* exit from `try..catch`. That includes an explicit `return`