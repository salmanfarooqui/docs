# Event Loop

Being single-threaded, JS creates the concept of event loop to run multiple tasks asynchronously. The event loop performs the function of constant monitoring of the message queue and the execution stack. The moment the execution stack becomes empty, the event loop lines up the first callback function on the execution stack.

![img](http://docs.salmanfarooqui.com/JS/images/1*FA9NGxNB6-v1oI2qGEtlRQ.png)

The Event Loop has one simple job — to monitor the Call Stack and the Callback Queue. If the Call Stack is empty, it will take the first event from the queue and will push it to the Call Stack, which effectively runs it.

Only when execution stack(call stack) gets cleared(is empty), JavaScript looks at event queue to check if there is something to process like a click event. If there is a click event in the queue, it creates execution context for whatever function that needs to run for that click event and puts that function in execution stack.

Note - Memory Heap is where the memory allocation happens



> In Other words,
>
> JavaScript runtimes contain a message queue which stores a list of messages to be processed and their associated callback functions. These messages are queued in response to external events (such as a mouse being clicked or receiving the response to an HTTP request) **given a callback function has been provided.**
>
> If, for example a user were to click a button and no callback function was provided – no message would have been enqueued.



So, for example, when your JavaScript program makes an Ajax request to fetch some data from the server, you set up the “response” code in a function (the “callback”), and the JS Engine tells the hosting environment:
“Hey, I’m going to suspend execution for now, but whenever you finish with that network request, and you have some data, please *call* this function *back*.”

The browser is then set up to listen for the response from the network, and when it has something to return to you, it will schedule the callback function to be executed by inserting it into the *event loop*.



##### Example

```js
console.log('Hi');
setTimeout(function cb1() { 
    console.log('cb1');
}, 5000);
console.log('Bye');
```

How it runs -

![img](http://docs.salmanfarooqui.com/JS/images/1*TozSrkk92l8ho6d8JxqF_w.gif)



***What happens when you have function calls in the Call Stack that take a huge amount of time to be processed*?**

While the Call Stack has functions to execute, the browser can’t do anything else — it’s being blocked. This means that the browser can’t render, it can’t run any other code, it’s just stuck. And here comes the problem — your app UI is no longer efficient and pleasing.



You may be writing your JavaScript application in a single JS file, but your program is almost certainly comprised of several blocks, only one of which is going to execute *now*, and the rest will execute *later*. The most common block unit is the function.

The problem most developers new to JavaScript seem to have is understanding that *later* doesn’t necessarily happen strictly and immediately after *now*. In other words, tasks that cannot complete *now* are, by definition, going to complete asynchronously, which means you won’t have the above-mentioned blocking behavior as you might have subconsciously expected or hoped for.



## How setTimeout(…) works

It’s important to note that `setTimeout(…)` doesn’t automatically put your callback on the event loop queue. It sets up a timer. When the timer expires, the environment places your callback into the event loop, so that some future tick will pick it up and execute it.

That doesn’t mean that `myCallback` will be executed in 1,000 ms but rather that, in 1,000 ms, `myCallback` will be added to the queue. The queue, however, might have other events that have been added earlier — your callback will have to wait.

There are quite a few articles and tutorials on getting started with async code in JavaScript that suggest doing a setTimeout(callback, 0). Well, now you know what the Event Loop does and how setTimeout works: calling setTimeout with 0 as a second argument just defers the callback until the Call Stack is clear.



```js
const bar = () => console.log('bar')
const baz = () => console.log('baz')

const foo = () => {
  console.log('foo');
  setTimeout(bar, 0);
  baz();
}
foo();

// foo
// baz
// bar
```





## Microtasks & Macrotasks

It turns out that not all tasks are created the same. There are macrotasks and microtasks. In reality, these two types of tasks occupy different queues.

> For each loop of the ‘event loop’ one macrotask is completed out of the macrotask queue. After that macrotask is complete, the event loop visits the microtask queue. The entire microtask queue is completed before moving on.



***Each time around the loop only one task from the macrotask queue is completed, but the entire microtask queue is completed after that.*** The “event loop” does not look into the next action until the completion of the entire micro-task (Job) queue.

This is done because user actions are more important than any background task such as setTimeout task, to give a user the best user experience.



### Macrotasks

Macrotasks includes parsing HTML, generating DOM, executing main thread JS code and other events such as page loading, input, network events, timer events, etc. From the browser’s point of view, Macrotask represents some discrete and independent work. Examples - setTimeout, setInternal, setImmediate, I/O tasks, requestAnimationFrame, I/O, UI rendering



### Microtasks

Microtasks is to complete some minor tasks to update the application state, such as processing the callback of Promise and DOM modification, so that these tasks can be executed before the browser re-renders. Microtask should be executed asynchronously as soon as possible, so their cost is less than Macrotask. Examples - process.nextTick, Promises, Object.observe, MutationObserver

If Promises were treated as Macro task, the it means that a long chain of promises would actually require multiple loops through the event loop to complete. This might not be ideal. 



In short, the Microtask Queue has a higher priority, that is, after executing a Macrotask task, the entire Microtask Queue will be cleared, and if there is a new microtask added at this time, it will also be executed.



*Generally, macrotasks as referred as tasks and microtasks as job.*



At any point of time if both the queues got entries, ***JobQueue gets higher precedence than TaskQueue***.

```js
const tom = () => console.log('Tom');
const jerry = () => console.log('Jerry');

const cartoon = () => {
  console.log('Cartoon');
    
  setTimeout(tom, 0);
    
  new Promise((resolve, reject) =>
    resolve('should it be right after Tom, before Jerry?')
  ).then(resolve => console.log(resolve))

  jerry();
}

cartoon();

// Cartoon
// Jerry
// should it be right after Tom, before Jerry?
// Tom
```

The Event Loop Model, gives priority to all the Jobs in JobQueue over anything in TaskQueue. Hence the callback of the promise get to the Call Stack first, gets executed and then, the function `tom` get to the Call Stack from TaskQueue and gets executed.



### ES6 Job Queue

[ECMAScript 2015](https://flaviocopes.com/ecmascript/) introduced the concept of the Job Queue, which is used by Promises (also introduced in ES6/ES2015). It’s a way to execute the result of an async function as soon as possible, rather than being put at the end of the call stack.

Promises that resolve before the current function ends will be executed right after the current function.

```js
const bar = () => console.log('bar')
const baz = () => console.log('baz')

const foo = () => {
  console.log('foo')
  setTimeout(bar, 0)
  new Promise((resolve, reject) =>
    resolve('should be right after baz, before bar')
  ).then(resolve => console.log(resolve))
  baz()
}

foo()

// foo
// baz
// should be right after baz, before bar
// bar
```

That’s a big difference between Promises (and async/await, which is built on promises) and plain old asynchronous functions through `setTimeout()` or other platform APIs.