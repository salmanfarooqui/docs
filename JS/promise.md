# Promise

Promises are used to handle asynchronous operations in JavaScript. They are easy to manage when dealing with multiple asynchronous operations where callbacks can create callback hell leading to unmanageable code.

A promise is an object that may produce a single value some time in the future. A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

## Callback

JavaScript runs code sequentially in top-down order. However, there are some cases that code runs (or must run) after something else happens and also not sequentially. This is called asynchronous programming.

Callbacks make sure that a function is not going to run before a task is completed but will run right after the task has completed.

In JavaScript, the way to create a callback function is to pass it as a parameter to another function, and then to call it back right after something has happened or some task is completed.

```js
function greeting(name) {
  alert('Hello ' + name);
}

function processUserInput(callback) {
  var name = prompt('Please enter your name.');
  callback(name);
}

processUserInput(greeting);
```



We also use callback functions for event declarations. 

```js
document.queryselector("#callback-btn")
    .addEventListener("click", function() {    
      console.log("User has clicked on the button!");
});
```

The first one is its type, “click”, and the second parameter is a callback function, which logs the message when the button is clicked. We will see a message on the console only when the user clicks on the button. 



## Promise

### Methods

**Promise.reject(reason)**

Returns a new `Promise` object that is rejected with the given reason.

**Promise.resolve(value)**

Returns a new `Promise` object that is resolved with the given value. If the value is a thenable (i.e. has a `then` method), the returned promise will "follow" that thenable, adopting its eventual state; otherwise the returned promise will be fulfilled with the value.

Generally, if you don't know if a value is a promise or not, Promise.resolve(value) it instead and work with the return value as a promise.



### Chained Promise

```js
const myPromise = 
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA,handleRejectedA)
  .then(handleFulfilledB,handleRejectedB)
  .then(handleFulfilledC,handleRejectedC);

// or, perhaps better ...

const myPromise =
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);
```



### Pass value in the chain

Simply use return to pass value to next then in the chain.

```js
let x = new Promise((resolve, reject) => {
resolve(40)
});

x.then((value) => {
console.log(value);
return value+1;
}).then((value) => {
console.log(value)
})

// console
// 40
// 41
```



### Call another promise from a promise

```js
let x = new Promise((resolve, reject) => {
resolve(40)
});

let y = new Promise((resolve, reject) => {
resolve(45)
});

x.then((value) => {
console.log(value);
return y;
}).then((value) => {
console.log(value)
})

// console
// 40
// 45
```



## Async/await

Async/await functions, a new addition with ES2017 (*ES8*), allows us to write completely synchronous-looking code while performing asynchronous tasks behind the scenes.

```js
function scaryClown() {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve('🤡');
		}, 2000);
	});
}
async function msg() {
	const msg = await scaryClown();
	console.log('Message:', msg);
}
msg(); // Message: 🤡 <-- after 2 seconds
```



> `await` is a new operator used to wait for a promise to resolve or reject. It can only be used inside an async function.

<br>

***Async functions always return a promise***, so the following may not produce the result you’re after:

```js
async function hello() {
  return 'Hello Alligator!';
}

const b = hello();
console.log(b); // [object Promise] { ... }
```

Since what’s returned is a promise, you could do something like this instead:

```js
async function hello() {
  return 'Hello Alligator!';
}

const b = hello();
b.then(x => console.log(x)); // Hello Alligator!
```

<br>

#### Example -

The power of async functions becomes more evident when there are multiple steps involved -

```js
function who() {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve('🤡');
		}, 200);
	});
}
function what() {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve('lurks');
		}, 300);
	});
}
async function msg() {
	const a = await who();
	const b = await what();
	console.log(`${ a } ${ b }`);
}
msg(); // 🤡 lurks in the shadows <-- after 500ms
```

A word of caution however, in the above example ***each step is done sequentially***, with each additional step waiting for the step before to resolve or reject before continuing. If you instead want the steps to happen in parallel, you can simply use ***Promise.all*** to wait for all the promises to have fulfilled.

```js
async function msg() {
	const [a, b] = await Promise.all([who(), what()]);
	console.log(`${ a } ${ b }`);
}
msg(); // 🤡 lurks in the shadows <-- after 300ms
```

*Promise.all* returns an array with the resolved values once all the passed-in promises have resolved.

<br>

> We can also define async function expressions and async arrow functions.
>
> ```js
> const msg = async function() {
> 	const msg = await scaryClown();
> 	console.log('Message:', msg);
> }
> ```
>
> ```js
> const msg = async () => {
> 	const msg = await scaryClown();
> 	console.log('Message:', msg);
> }
> ```



### With Promise-Based APIS

```js
async function fetchUsers(endpoint) {
	const res = await fetch(endpoint);
	let data = await res.json();
	data = data.map(user => user.username);
	console.log(data);
}
fetchUsers('https://jsonplaceholder.typicode.com/users'); // ["Bret", "Antonette", "Samantha", "Karianne", "Kamren", "Leopoldo_Corkery"]
```



## Error Handling

Error handling in async function is also done completely synchronously, using good old **try…catch** statements.

```js
function yayOrNay() {
	return new Promise((resolve, reject) => {
		const val = Math.round(Math.random() * 1); // 0 or 1, at random
		val ? resolve('Lucky!!') : reject('Nope 😠');
	});
}
async function msg() {
	try {
		const msg = await yayOrNay();
		console.log(msg);
	} catch(err) {
		console.log(err);
	}
}
msg(); // Lucky!!
msg(); // Lucky!!
msg(); // Lucky!!
msg(); // Nope 😠
msg(); // Lucky!!
msg(); // Nope 😠
msg(); // Nope 😠
```

Given that async functions always return a promise, you can also deal with unhandled errors as you would normally using a catch statement:

```js
async function msg() {
  const msg = await yayOrNay();
  console.log(msg);
}

msg().catch(x => console.log(x));
```



This synchronous error handling doesn’t just work when a promise is rejected, but also when there’s an actual runtime or syntax error happening. 

```js
function caserUpper(val) {
	return new Promise((resolve, reject) => {
		resolve(val.toUpperCase());
	});
}
async function msg(x) {
	try {
		const msg = await caserUpper(x);
		console.log(msg);
	} catch(err) {
		console.log('Ohh no:', err.message);
	}
}
msg('Hello'); // HELLO
msg(34); // Ohh no: val.toUpperCase is not a function
```





## Fetch

A typical fetch request consists of two await calls 
```js
let response = await fetch(url, options); // resolves with response headers
let result = await response.json(); // read body as json
```
Or, without await
```js
fetch(url, options)
  .then(response => response.json())
  .then(result => /* process result */)
```

Here options is optional parameters: method, headers etc.
Without options, this is a simple GET request, downloading the contents of the url.
Here’s the full list of all possible fetch options with their default values (alternatives in comments):

```js
let promise = fetch(url, {
  method: "GET", // POST, PUT, DELETE, etc.
  headers: {
    // the content type header value is usually auto-set
    // depending on the request body
    "Content-Type": "text/plain;charset=UTF-8"
  },
  body: undefined, // string, FormData, Blob, BufferSource, or URLSearchParams
  referrer: "about:client", // or "" to send no Referer header,
  // or an url from the current origin
  referrerPolicy: "strict-origin-when-cross-origin", // no-referrer-when-downgrade, no-referrer, origin, same-origin...
  mode: "cors", // same-origin, no-cors
  credentials: "same-origin", // omit, include
  cache: "default", // no-store, reload, no-cache, force-cache, or only-if-cached
  redirect: "follow", // manual, error
  integrity: "", // a hash, like "sha256-abcdef1234567890"
  keepalive: false, // true
  signal: undefined, // AbortController to abort request
  window: window // null
});
```

Getting a response is usually a two-stage process.

- First, the promise, returned by fetch, resolves with an object of the built-in Response class as soon as the server responds with headers. At this stage we can check HTTP status, to see whether it is successful or not, check headers, but don’t have the body yet.

- Second, to get the response body, we need to use an additional method call. Response provides multiple promise-based methods to access the body in various formats - response.text(), response.json(), response.blob() etc.


<br>

[![Promises and Async/await ](https://img.youtube.com/vi/vn3tm0quoqE/0.jpg)](https://www.youtube.com/watch?v=vn3tm0quoqE)
