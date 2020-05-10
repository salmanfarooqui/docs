# JS Debugger

In `hello.js`, click at line number `4` and `8`.  Congratulations! You’ve set a breakpoint. 

![debugging01](C:\Users\Salman Farooqui\Documents\Docs\JS\images\debugging01.png)

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

![debugger02](C:\Users\Salman Farooqui\Documents\Docs\JS\images\debugger02.png)



1. **`Watch` – shows current values for any expressions.**

   You can click the plus `+` and input an expression. The debugger will show its value at any moment, automatically recalculating it in the process of execution.

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

![debugger03](C:\Users\Salman Farooqui\Documents\Docs\JS\images\debugger03.png)



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

### || 

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