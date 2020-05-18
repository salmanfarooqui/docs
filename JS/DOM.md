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

Here, event will not bubble up(travel up) from target element to the top element of DOM.

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
>      // bind this.onClick to always run with the context of the Menu object. 
>      // will run the onClick method with "this" = Menu object created at the time it was bound
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




### Load event



### Timers

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

