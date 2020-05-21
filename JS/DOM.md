# DOM

## Events

An event is a signal that something has happened. To react on events we can assign a handler (a function that runs when an event triggers). Handlers are a way to run JavaScript code in case of user actions. 

### Ways to Assign Event

#### 1. HTML-attribute (bad practice)

A handler can be set in HTML with an attribute named `on<event>`.

```html
<input value="Click me" onclick="alert('Click!')" type="button">
```

Please note that inside `onclick` we use single quotes, because the attribute itself is in double quotes. This won't work - `onclick="alert("Click!")"`

An HTML-attribute is not a convenient place to write a lot of code, so we’d better create a JavaScript function and call it there.

```html
<script>
  function countRabbits() {
    for(let i=1; i<=3; i++) {
      alert("Rabbit number " + i);
    }
  }
</script>

<input type="button" onclick="countRabbits()" value="Count rabbits!">
```

> HTML attribute names are not case-sensitive, so `ONCLICK` works as well as `onClick` and `onCLICK`… But usually attributes are lowercased: `onclick`



The value of `this` inside a handler is the element. The one which has the handler on it.

```html
<button onclick="alert(this.innerHTML)">Click me</button> // Click me
```



#### 2. DOM property

We can assign a handler using a DOM property `on`. For instance, `elem.onclick`

```html
<input id="elem" type="button" value="Click me">
<script>
  elem.onclick = function() {
    alert('Thank you');
  };
</script>
```

If the handler is assigned using an **HTML-attribute** then the browser reads it, creates a new function from the attribute content and writes it to the DOM property. So this way is actually the same as the previous one.

> To remove a handler – assign `elem.onclick = null`

> Assign a handler to `elem.onclick`, not `elem.ONCLICK`, because DOM properties are case-sensitive.

##### **Note -**

```javascript
function sayThanks() {
  alert('Thanks!');
}

elem.onclick = sayThanks;
```

The function should be assigned as `sayThanks`, not `sayThanks()`. If we add parentheses, then `sayThanks()` becomes is a function call. So the last line actually takes the *result* of the function execution, that is `undefined`, and assigns it to `onclick`. That doesn’t work.

On the other hand, in the markup we do need the parentheses:

```html
<input type="button" id="button" onclick="sayThanks()">
```

Here, when the browser reads the attribute, it creates a handler function with body from the attribute content.

So the markup generates this property:

```js
button.onclick = function() {
  sayThanks(); // <-- the attribute content goes here
};
```

##### **Note -**

Don’t use `setAttribute` for handlers

```js
// a click on <body> will generate errors,
// because attributes are always strings, function becomes a string
document.body.setAttribute('onclick', function() { alert(1) });
```



#### 3. addEventListener

The fundamental problem of the above mentioned ways to assign handlers – we can’t assign multiple handlers to one event. Let’s say, one part of our code wants to highlight a button on click, and another one wants to show a message on the same click. If we do assign it using dom property, second one will replace the first.

```javascript
input.onclick = function() { alert(1); }
// ...
input.onclick = function() { alert(2); } // replaces the previous handler
```

To overcome this we use special methods addEventListener and removeEventListener. **1) addEventListener allows assigning multiple handlers to one event**. **2) You can also remove an event listener.**

`element.addEventListener(event, handler, [options])`

<u>event</u> - Event name, e.g. `"click"`.

<u>handler</u> - The handler function.

<u>options</u> - An additional optional object with properties:

- <u>once</u> : if `true`, then the listener is automatically removed after it triggers.
- <u>capture</u> : the phase where to handle the event. For historical reasons, `options` can also be `false/true`, that’s the same as `{capture: false/true}`
- <u>passive</u> : if `true`, then the handler will not call `preventDefault()`

<br>

We can assign not just a function, but an object as an event handler using addEventListener. When an event occurs, its `handleEvent` method is called.

```js
<button id="elem">Click me</button>

let obj = {
    handleEvent(event) {
      alert(event.type + " at " + event.currentTarget);
    }
};

elem.addEventListener('click', obj);
```

The method `handleEvent` does not have to do all the job by itself. It can call other event-specific methods instead, like this:

```js
<button id="elem">Click me</button>

 class Menu {
    handleEvent(event) {
      // mousedown -> onMousedown
      let method = 'on' + event.type[0].toUpperCase() + event.type.slice(1);
      this[method](event);
    }
     
    onMousedown() {
      elem.innerHTML = "Mouse button pressed";
    }
     
    onMouseup() {
      elem.innerHTML += "...and released.";
    }
  }

  let menu = new Menu();
  elem.addEventListener('mousedown', menu);
  elem.addEventListener('mouseup', menu);
```

<br>

**Removing a handler** - `element.removeEventListener(event, handler, [options])`

To remove a handler we should pass exactly the same function as was assigned. This doesn’t work -

```javascript
elem.addEventListener( "click" , () => alert('Thanks!'));
// ....
elem.removeEventListener( "click", () => alert('Thanks!'));
```

The handler won’t be removed, because `removeEventListener` gets another function – with the same code, but that doesn’t matter, as it’s a different function object.

Here’s the right way -

```js
function handler() {
  alert( 'Thanks!' );
}

input.addEventListener("click", handler);
// ....
input.removeEventListener("click", handler);
```

If we don’t store the function in a variable, then we can’t remove it.



### Event object

When an event happens, the browser creates an *event object*, puts details into it and passes it as an argument to the handler.

```js
<input type="button" value="Click me" id="elem">
    
  elem.onclick = function(event) {
    alert(event.type + " at " + event.currentTarget);
    alert("Coordinates: " + event.clientX + ":" + event.clientY);
  };
```

Some properties of `event` object:

- <u>event.type</u> - Event type, here it’s `"click"`.
- <u>event.target</u> - A reference to the target to which the event was originally dispatched.
- <u>event.currentTarget</u> - Element that handled the event. That’s exactly the same as `this`, unless the handler is an arrow function, or its `this` is bound to something else, then we can get the element from event.currentTarget

`target` is the element that triggered the event (e.g., the user clicked on) 

`currentTarget` is the element that the event listener is attached to.

Some methods:

- <u>event.preventDefault()</u> - Cancels the event (if it is cancelable). Tells that its default action should not be taken as it normally would be. The event continues to propagate as usual. If the handler is assigned using `on<event>` (not by addEventListener), then returning `false` also works the same. The value returned by an event handler is usually ignored, `return false` is an exception.
- <u>event.stopPropagation()</u> - called to stop propagating the event in the DOM. First handler is run but the event doesn't bubble any further up the chain.
- <u>stopImmediatePropagation()</u> - prevents other listeners of the same event from being called. Let's say several listeners are attached to the same element for the same event type, if stopImmediatePropagation() is invoked during one such call, no remaining listeners will be called.

`stopPropagation` will prevent any **parent** handlers from being executed 

`stopImmediatePropagation` will prevent any parent handlers **and also** any **other** handlers from executing

There are more [properties and methods](https://developer.mozilla.org/en-US/docs/Web/API/Event). Many of them depend on the event type: keyboard, pointer, etc.



### Event Lifecycle

When Button is clicked, an event is dispatched into the event flow. The event journey starts at the Document, moves down to the HTML element, moves to the Body element, and finally gets to the Button element. The event then bubbles back up to Document, moving again through the Body element and HTML element on its way up.

The typical DOM event flow is conceptually divided into three phases:

- The **capture phase** comprises all the DOM elements on the trip from the Document to the parent of the target element on which an event was triggered. In other words, everything from the Document to the target, not including the target itself. It is rarely used in real code, but sometimes can be useful.
- The **target phase** occurs when the event reaches the target. Then event is fired on the target, before reversing and retracing its steps, propagating back to the outermost Document. The target phase comprises the time spent at the Button.
- The **bubbling phase** comprises all the DOM elements encountered on the return trip from the target back to the Document. Bubbling gives the freedom of handling an event on any element by its parent elements.

```js
<body>
	<div id="parent">
    	<button id="child">Child</button>
	</div>
</body>

var parent = document.querySelector('#parent);
var child = document.querySelector('#child);

parent.addEventListener('click', function() {
	console.log("Parent Clicked");
});
child.addEventListener('click', function() {
	console.log("Child Clicked");
});

// Child Clicked
// Parent Clicked
```

When we click on the button first run the function which is attached on button , after that `onclick()` function of div runs. This is due to Event bubbling. First run the event which is attached with event target then its parents on the way to `window` object.

![event-lifecycle](https://docs.salmanfarooqui.com/JS/images/event-lifecycle.png)

If you want to stop the **event bubbling**, this can be achieved by the use of the `event.stopPropagation()` method. 

```js
child.addEventListener('click', function(e) {
    e.stopPropagation();
	console.log("Child Clicked");
});
// Child Clicked
```



> By default, **all event handlers are triggered only during the target and bubbling phase**. We can handle events during the capture phase by setting useCapture, the third argument of addEventListener, to true.
>
> ```js
> <body>
> 	<div id="parent">
>     	<button id="child">Child</button>
> 	</div>
> </body>
> 
> var parent = document.querySelector('#parent);
> var child = document.querySelector('#child);
> 
> parent.addEventListener('click', function() {
> 	console.log("Parent Clicked");
> }, true);
> child.addEventListener('click', function() {
> 	console.log("Child Clicked");
> });
> 
> // Parent Clicked
> // Child Clicked
> ```

> Bubbling also allows us to take advantage of **event delegation** — this concept relies on the fact that if you want some code to run when you click on any one of a large number of child elements, you can set the event listener on their parent and have events that happen on them bubble up to their parent rather than having to set the event listener on every child individually. Another example is If we have a single handler `form.onclick`, then it can “catch” all clicks inside the form. No matter where the click happened, it bubbles up to `<form>` and runs the handler.
>
> There are other uses for event delegation.
>
> ```js
> <div id="menu">
> <button data-action="save">Save</button>
> <button data-action="load">Load</button>
> </div>
> 
> class Menu {
>  constructor(elem) {
>    elem.onclick = this.onClick.bind(this); 
>      // elem is menu dom node, 'this' is empty Menu object with __proto__ containing save, load and onClick methods
>      // onClick is bound to Menu object, inside onClick 'this' will referance Menu Object
>  }
> 
>  save() {
>    alert('saving');
>  }
>  load() {
>    alert('loading');
>  }
> 
>  onClick(event) {
>    let action = event.target.dataset.action;
>    if (action) {
>      this[action]();
>    }
>  };
> }
> 
> new Menu(menu);
> ```
> The first idea may be to assign a separate handler to each button. But this is a more elegant solution.  We added a handler for the whole menu and `data-action` attributes for buttons.
>
> Please note that `this.onClick` is bound to `this`. That’s important, because otherwise `this` inside it would reference the DOM element (`elem`), not the `Menu` object



### Dispatch Events

We can not only assign handlers, but also generate events from JavaScript.

We can create `Event` objects like this:

```javascript
let event = new Event(type[, options]);
```

- <u>type</u> – event type, a string like `"click"` or our own like `"my-event"`.

- <u>options</u> – the object with two optional properties:

  - <u>bubbles: true/false</u> – if `true`, then the event bubbles.
  - <u>cancelable: true/false</u> – if `true`, then the “default action” may be prevented. Later we’ll see what it means for custom events.

  By default both are false: `{bubbles: false, cancelable: false}`.

After an event object is created, we should “run” it on an element using the call `elem.dispatchEvent(event)`. Then handlers react on it as if it were a regular browser event. If the event was created with the `bubbles` flag, then it bubbles.

In the example below the `click` event is initiated in JavaScript. The handler works same way as if the button was clicked -

```js
<button id="elem" onclick="alert('Click!');">Autoclick</button>

let event = new Event("click");
elem.dispatchEvent(event);
```

**Example -** 

We can create a bubbling event with the name `"hello"` and catch it on `document`.

```js
<h1 id="elem">Hello from the script!</h1>

 // catch on document...
 document.addEventListener("hello", function(event) { // (1)
    alert("Hello from " + event.target.tagName); // Hello from H1
 });

 // ...dispatch on elem!
 let event = new Event("hello", {bubbles: true}); // (2)
 elem.dispatchEvent(event);

 // the handler on document will activate and display the message.
```

1. We should use `addEventListener` for our custom events, because `on` only exists for built-in events, `document.onhello` doesn’t work.
2. Must set `bubbles:true`, otherwise the event won’t bubble up.

> There is a way to tell a “real” user event from a script-generated one. The property `event.isTrusted` is `true` for events that come from real user actions and `false` for script-generated events.



**Event Types**

Here’s a short list of classes for UI Events from the UI Event specification:

- `UIEvent`
- `FocusEvent`
- `MouseEvent`
- `WheelEvent`
- `KeyboardEvent`

We should use them instead of `new Event` if we want to create such events. For instance, `new MouseEvent("click")`. The right constructor allows to specify standard properties for that type of event.

Like `clientX/clientY` for a mouse event -

```javascript
let event = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 100,
  clientY: 100
});

alert(event.clientX); // 100
// the generic Event constructor does not allow clientX, clientY
// Technically, we can work around that by assigning directly event.clientX=100 after creation
```

**Custom Events**

completely new events types like `"hello"` we should use `new CustomEvent`. Technically [CustomEvent](https://dom.spec.whatwg.org/#customevent) is the same as `Event`, with one exception. In the second argument (object) we can add an additional property `detail` for any custom information that we want to pass with the event.

```js
<h1 id="elem">Hello for John!</h1>

  // additional details come with the event to the handler
  elem.addEventListener("hello", function(event) {
    alert(event.detail.name);
  });

  elem.dispatchEvent(new CustomEvent("hello", {
    detail: { name: "John" }
  }));
```

The `detail` property can have any data. Technically we could live without, because we can assign any properties into a regular `new Event` object after its creation. But `CustomEvent` provides the special `detail` field for it to evade conflicts with other event properties.



**event.preventDefault()**

Many browser events have a “default action”, such as nagivating to a link, starting a selection, and so on.

For new, custom events, there are definitely no default browser actions, but a code that dispatches such event may have its own plans what to do after triggering the event. By calling `event.preventDefault()`, an event handler may send a signal that those actions should be canceled. In that case the call to `elem.dispatchEvent(event)` returns `false`. And the code that dispatched it knows that it shouldn’t continue.



```js
<button onclick="hide()">Hide()</button>

  // hide() will be called automatically in 2 seconds
  function hide() {
    let event = new CustomEvent("hide", {
      cancelable: true // without that flag preventDefault doesn't work
    });
    if (!rabbit.dispatchEvent(event)) {
      alert('The action was prevented by a handler');
    } else {
      rabbit.hidden = true;
    }
  }

  rabbit.addEventListener('hide', function(event) {
    if (confirm("Call preventDefault?")) {
      event.preventDefault();
    }
  });
```



**Events-in-events are synchronous**

Usually events are processed in a queue. That is: if the browser is processing `onclick` and a new event occurs, then it’s handing is queued up and will be called after `onclick` processing is finished.

Except when one event is initiated from within another one, e.g. using `dispatchEvent`. Such events are processed immediately: the new event handlers are called, and then the current event handling is resumed.

```js
<button id="menu">Menu (click me)</button>

menu.onclick = function() {
alert(1);

menu.dispatchEvent(new CustomEvent("menu-open", {
bubbles: true
}));

alert(2);
};

// triggers between 1 and 2
document.addEventListener('menu-open', () => alert('nested'));
```

*Output - 1 → nested → 2*

It’s processed immediately, without waiting for onlick handler to end. 

Please note that the nested event `menu-open` is caught on the `document`. The propagation and handling of the nested event is finished before the processing gets back to the outer code (`onclick`). That’s not only about `dispatchEvent`, there are other cases. If an event handler calls methods that trigger to other events – they are too processed synchronously, in a nested fashion.

Let's say we want `onclick` to be fully processed first, independently from `menu-open`. Then we can either put the `dispatchEvent` (or another event-triggering call) at the end of `onclick` or, maybe better, wrap it in the zero-delay `setTimeout`:

```js
setTimeout(() => menu.dispatchEvent(new CustomEvent("menu-open", {
   bubbles: true
})));
```

Now `dispatchEvent` runs asynchronously after the current code execution is finished, including `mouse.onclick`, so event handlers are totally separate. *The output order becomes 1 → 2 → nested.*





### Load event

The lifecycle of an HTML page has three important events:

- <u>DOMContentLoaded</u> – the browser fully loaded HTML, and the DOM tree is built, but external resources like pictures (img tags) and stylesheets may be not yet loaded. DOM is ready, so the handler can lookup DOM nodes, initialize the interface. The `DOMContentLoaded` event happens on the `document` object.

  We must use `addEventListener` to catch it:

  ```javascript
  document.addEventListener("DOMContentLoaded", ready);
  // not "document.onDOMContentLoaded = ..."
  ```

- <u>load</u> – not only HTML is loaded, but also all the external resources: images, styles etc. External resources are loaded, so styles are applied, image sizes are known etc.

- <u>beforeunload/unload</u> – the user is leaving the page. 1) `beforeunload` – the user is leaving: we can check if the user saved the changes and ask them whether they really want to leave. 2) `unload` – the user almost left, but we still can initiate some operations, such as sending out statistics.



**DOMContentLoaded**

When the browser processes an HTML-document and comes across a `<script>` tag, it needs to execute before continuing building the DOM. As scripts may want to modify DOM, and even `document.write` into it, so `DOMContentLoaded` has to wait. So DOMContentLoaded definitely happens after such scripts.

```html
<script>
  document.addEventListener("DOMContentLoaded", () => {
    alert("DOM ready!");
  });
</script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.3.0/lodash.js"></script>

<script>
  alert("Library loaded, inline script executed");
</script>

// Library loaded, inline script executed
// DOM ready!
```

There are two exceptions from this rule:

1. Scripts with the `async` attribute, don’t block `DOMContentLoaded`.
2. Scripts that are generated dynamically with `document.createElement('script')` and then added to the webpage also don’t block this event.

External style sheets don’t affect DOM, so `DOMContentLoaded` does not wait for them. But there’s a pitfall. If we have a script after the style, then that script must wait until the stylesheet loads.

```html
<link type="text/css" rel="stylesheet" href="style.css">
<script>
  // the script doesn't not execute until the stylesheet is loaded
  alert(getComputedStyle(document.body).marginTop);
</script>
```

The reason for this is that the script may want to get coordinates and other style-dependent properties of elements, like in the example above. Naturally, it has to wait for styles to load. As `DOMContentLoaded` waits for scripts, it now waits for styles before them as well.



**window.onload**

The `load` event on the `window` object triggers when the whole page is loaded including styles, images and other resources. This event is available via the `onload` property.

```html
<script>
  window.onload = function() { // same as window.addEventListener('load', (event) => {
    alert('Page loaded');
    // image is loaded at this time
    alert(`Image size: ${img.offsetWidth}x${img.offsetHeight}`);
  };
</script>

<img id="img" src="https://en.js.cx/clipart/train.gif?speed=1&cache=0">
```



**window.onunload**

When a visitor leaves the page, the `unload` event triggers on `window`. We can do something there that doesn’t involve a delay, like closing related popup windows.

```javascript
window.addEventListener("unload", function() {
  navigator.sendBeacon("/analytics", JSON.stringify(analyticsData));
};
```



**window.onbeforeunload**

If a visitor initiated navigation away from the page or tries to close the window, the `beforeunload` handler asks for additional confirmation.

```javascript
window.onbeforeunload = function() {
  return false;
};
// if we run this and then reload page, a popup will appear asking for confirmation
```



**readyState**

There are cases when we are not sure whether the document is ready or not. We’d like our function to execute when the DOM is loaded, be it now or later.

The `document.readyState` property tells us about the current loading state. There are 3 possible values.

- <u>"loading"</u> – the document is loading.
- <u>"interactive"</u> – the document was fully read.
- <u>"complete"</u> – the document was fully read and all resources (like images) are loaded too.

So we can check `document.readyState` and setup a handler or execute the code immediately if it’s ready.

```javascript
function work() { /*...*/ }

if (document.readyState == 'loading') {
  // loading yet, wait for the event
  document.addEventListener('DOMContentLoaded', work);
} else {
  // DOM is ready!
  work();
}
```

There’s also the `readystatechange` event that triggers when the state changes.

```javascript
document.addEventListener('readystatechange', () => console.log(document.readyState));
// print state changes
```

The readystatechange event is an alternative mechanics of tracking the document loading state, it appeared long ago. Nowadays, it is rarely used.



### Timers

We may decide to execute a function not right now, but at a certain time later. That’s called “scheduling a call”.

There are two methods for it:  `setTimeout` and `setInterval`.  These methods are not a part of JavaScript specification. But most environments have the internal scheduler and provide these methods.



**setTimeout**

```javascript
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

<u>func|code</u> - Usually Function but a string of code is allowed too although not recommended.

<u>delay</u> - The delay before run, in milliseconds. By default 0

<u>arg1, arg2…</u> - Arguments for the function (not supported in IE9-)

```javascript
function sayHi(phrase, who) {
  alert( phrase + ', ' + who );
}

setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John
```

**There’s a special use case: `setTimeout(func, 0)`, or just `setTimeout(func)`**

This schedules the execution of `func` as soon as possible. But the scheduler will invoke it only after the currently executing script is complete. So the function is scheduled to run “right after” the current script.

```javascript
setTimeout(() => alert("World"));
alert("Hello");
// outputs “Hello”, then immediately “World”
```



**setInterval**

The setInterval method has the same syntax as setTimeout. But unlike setTimeout it runs the function not only once, but regularly after the given interval of time.

There are two ways of running something regularly.. One is `setInterval`. The other one is a `nested setTimeout`

```javascript
// instead of: let timerId = setInterval(() => alert('tick'), 2000);

let timerId = setTimeout(function tick() {
  alert('tick');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
```

The setTimeout above schedules the next call right at the end of the current one `(*)`.

The **nested setTimeout is a more flexible method than setInterval**. This way the next call may be scheduled differently, depending on the results of the current one. For instance, we need to write a service that sends a request to the server every 5 seconds asking for data, but in case the server is overloaded, it should increase the interval to 10, 20, 40 seconds…

```javascript
let delay = 5000;
let timerId = setTimeout(function request() {
  ...send request...
  if (request failed due to server overload) {
    delay *= 2;   // increase the interval to the next run
  }
  timerId = setTimeout(request, delay);
}, delay);
```

Also, **Nested setTimeout allows to set the delay between the executions more precisely than setInterval**

```javascript
// using setInterval
let i = 1;
setInterval(function() {
  func(i++);
}, 100);

//using nested setTimeout
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
```

![Scheduling_setTimeout_and_setInterval](https://docs.salmanfarooqui.com/JS/images/Scheduling_setTimeout_and_setInterval.jpg)

The delay in setInterval code is normal, because the time taken by `func`'s execution “consumes” a part of the interval. **The nested setTimeout guarantees the fixed delay (here 100ms).**That’s because a new call is planned at the end of the previous one.



**Cancelling** 

We know setTimeout schedules another function to be called later, after a given number of milliseconds. Sometimes you need to cancel a function you have scheduled. This is done by storing the value returned by `setTimeout` and calling `clearTimeout` on it.



```js
let bombTimer = setTimeout(() => {
  console.log("BOOM!");
}, 500);

if (Math.random() < 0.5) { // 50% chance
  console.log("Defused.");
  clearTimeout(bombTimer);
}
```

A similar set of functions, `setInterval` and `clearInterval`, are used to set timers that should *repeat* every *X* milliseconds.

> Please note that all scheduling methods do not *guarantee* the exact delay.
>
> For example, the in-browser timer may slow down for a lot of reasons: The CPU is overloaded. The browser tab is in the background mode. The laptop is on battery.



### Debouncing

Some types of events have the potential to fire rapidly, many times in a row (the `"mousemove"` and `"scroll"` events, for example). When handling such events, you must be careful not to do anything too time-consuming or your handler will take up so much time that interaction with the document starts to feel slow.

If you do need to do something nontrivial in such a handler, you can use `setTimeout` to make sure you are not doing it too often. This is usually called *debouncing* the event.

```js
let textarea = document.querySelector("textarea");
  let timeout;
  textarea.addEventListener("input", () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => console.log("Typed!"), 500);
  });
```

We want to react when the user has typed something, but we don’t want to do it immediately for every input event. When they are typing quickly, we just want to wait until a pause occurs. Instead of immediately performing an action in the event handler, we set a timeout. We also clear the previous timeout (if any) so that when events occur close together (closer than our timeout delay), the timeout from the previous event will be canceled.

Giving an undefined value to `clearTimeout` or calling it on a timeout that has already fired has no effect. Thus, we don’t have to be careful about when to call it, and we simply do so for every event.



## Forms

