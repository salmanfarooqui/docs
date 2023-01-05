# REACT



1. We split JSX over multiple lines for readability. While it isn’t required, when doing this, we also recommend wrapping it in parentheses to avoid the pitfalls of [automatic semicolon insertion](https://stackoverflow.com/q/2846283).

```js
const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```

<br><br>

2. Since JSX is closer to JavaScript than to HTML, **React DOM uses `camelCase` property naming convention instead of HTML attribute names**.

   For example, `class` becomes [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) in JSX, and `tabindex` becomes [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

<br><br>

3. **Always start component names with a capital letter.**

React treats components starting with lowercase letters as DOM tags. For example, <div /> represents an HTML div tag, but <Welcome /> represents a component and requires Welcome to be in scope.

<br><br>

4. Example

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```

​	Let’s recap what happens in this example:

- We call `root.render()` with the `<Welcome name="Sara" />` element.
- React calls the `Welcome` component with `{name: 'Sara'}` as the props.
- Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
- React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.


<br><br>

5. Whether you declare a component [as a function or a class](https://reactjs.org/docs/components-and-props.html#function-and-class-components), it must never modify its own props.

   Consider this `sum` function:

   ```js
   function sum(a, b) {
     return a + b;
   }
   ```

   Such functions are called [“pure”](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.

   In contrast, this function is impure because it changes its own input:

   ```js
   function withdraw(account, amount) {
     account.total -= amount;
   }
   ```

   React is pretty flexible but it has a single strict rule:

   **All React components must act like pure functions with respect to their props.**

<br><br>

6.  

   ```js
    componentDidMount() {
       this.timerID = setInterval(
         () => this.tick(),
         1000
       );
     }
    tick() {
       this.setState({
         date: new Date()
       });
     }
   ```

   Note how we save the timer ID right on `this` (`this.timerID`).

   While `this.props` is set up by React itself and `this.state` has a special meaning, **you are free to add additional fields to the class manually if you need to store something that doesn’t participate in the data flow** (like a timer ID).

   We will tear down the timer in the `componentWillUnmount()` lifecycle method:

   ```js
     componentWillUnmount() {
       clearInterval(this.timerID);
     }
   ```

<br><br>

7.  React may batch multiple `setState()` calls into a single update for performance.

   **Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.**

   For example, this code may fail to update the counter:

   ```js
   // Wrong
   this.setState({
     counter: this.state.counter + this.props.increment,
   });
   ```

   To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

   ```js
   // Correct
   this.setState((state, props) => ({
     counter: state.counter + props.increment
   }));
   ```

<br><br>

8. Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

   - React events are named using camelCase, rather than lowercase.

   - With JSX you pass a function as the event handler, rather than a string.

     For example, the HTML:

     ```js
     <button onclick="activateLasers()">
       Activate Lasers
     </button>
     ```

     is slightly different in React:

     ```js
     <button onClick={activateLasers}>
       Activate Lasers
     </button>
     ```

   Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly.

   <br><br>

9. When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), a common pattern is for an event handler to be a method on the class.

```js
   class Toggle extends React.Component {
     constructor(props) {
       super(props);
       this.state = {isToggleOn: true};
   
       // This binding is necessary to make `this` work in the callback
       this.handleClick = this.handleClick.bind(this);
     }
   
     handleClick() {
       this.setState(state => ({
         isToggleOn: !state.isToggleOn
       }));
     }
   
     render() {
       return (
         <button onClick={this.handleClick}>
           {this.state.isToggleOn ? 'ON' : 'OFF'}
         </button>
       );
     }
   }
```

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. **If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.**

This is not React-specific behavior; it is a part of [how functions work in JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Generally, if you refer to a method without `()` after it, such as `onClick={this.handleClick}`, you should bind that method.



- If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/).

```js
  class LoggingButton extends React.Component {
    // This syntax ensures `this` is bound within handleClick.
    // Warning: this is *experimental* syntax.
    handleClick = () => {
      console.log('this is:', this);
    }
  
    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      );
    }
  }
```



- If you aren’t using class fields syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:

```js
  class LoggingButton extends React.Component {
    handleClick() {
      console.log('this is:', this);
    }
  
    render() {
      // This syntax ensures `this` is bound within handleClick
      return (
        <button onClick={(e) => this.handleClick(e)}>
          Click me
        </button>
      );
    }
  }
```

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, **if this callback is passed as a prop to lower components, those components might do an extra re-rendering**.

<br><br>

10. Inside a loop, it is common to want to pass an extra parameter to an event handler. For example, if `id` is the row ID, either of the following would work:

    ```js
    <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
    <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
    ```

    The above two lines are equivalent, and use [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) and [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) respectively.

    In **both cases, the `e` argument representing the React event will be passed as a second argument after the ID**. With an arrow function, we have to pass it explicitly, but with `bind` any further arguments are automatically forwarded.

    <br><br>

11. In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

    In the example below, the `<WarningBanner />` is rendered depending on the value of the prop called `warn`. If the value of the prop is `false`, then the component does not render:

    ```js
    function WarningBanner(props) {
      if (!props.warn) {
        return null;
      }
    
      return (
        <div className="warning">
          Warning!
        </div>
      );
    }
    
    class Page extends React.Component {
      constructor(props) {
        super(props);
        this.state = {showWarning: true};
        this.handleToggleClick = this.handleToggleClick.bind(this);
      }
    
      handleToggleClick() {
        this.setState(state => ({
          showWarning: !state.showWarning
        }));
      }
    
      render() {
        return (
          <div>
            <WarningBanner warn={this.state.showWarning} />
            <button onClick={this.handleToggleClick}>
              {this.state.showWarning ? 'Hide' : 'Show'}
            </button>
          </div>
        );
      }
    }
    ```

    Returning `null` from a component’s `render` method does not affect the firing of the component’s lifecycle methods. For instance `componentDidUpdate` will still be called.

    <br><br>

12. Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```




The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys.

When you don’t have stable IDs for rendered items, you may use the item index as a key as a last resort. We don’t recommend using indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state.

**A good rule of thumb is that elements inside the `map()` call need keys.**

Keys serve as a hint to React but they don’t get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name.

<br><br>

13.   An input form element whose value is controlled by React in this way is called a “controlled component”.

 ```js
    class NameForm extends React.Component {
      constructor(props) {
        super(props);
        this.state = {value: ''};
    
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
      }
    
      handleChange(event) {
        this.setState({value: event.target.value});
      }
    
      handleSubmit(event) {
        alert('A name was submitted: ' + this.state.value);
        event.preventDefault();
      }
    
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <label>
              Name:
              <input type="text" value={this.state.value} onChange={this.handleChange} />
            </label>
            <input type="submit" value="Submit" />
          </form>
        );
      }
    }
 ```

Since the `value` attribute is set on our form element, the displayed value will always be `this.state.value`, making the React state the source of truth. Since `handleChange` runs on every keystroke to update the React state, the displayed value will update as the user types.



When you need to handle multiple controlled `input` elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

```js
handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

		<form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
```

<br><br>

14.  Some components don’t know their children ahead of time. We recommend that such components use the special `children` prop to pass children elements directly into their output:

    ```js
    function FancyBorder(props) {
      return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
          {props.children}
        </div>
      );
    }
    ```

This lets other components pass arbitrary children to them by nesting the JSX:

```js
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```



**Advanced Example -** 

`props.children` works just like any other prop in that it can pass any sort of data, not just the sorts that React knows how to render. For example, if you have a custom component, you could have it take a callback as `props.children`:

```js
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

Children passed to a custom component can be anything, as long as that component transforms them into something React can understand before rendering. This usage is not common, but it works if you want to stretch what JSX is capable of.



## Export

### Default Export

If you export as the default export like this,

`export default class Template extends React.Component {}`



Then in another file you import the default export without using the {}, like this,

`import Template from './components/templates'`

<br>

There can only be one default export per file. You're free to rename the default export as you import it,

`import TheTemplate from './components/templates'`



### Named Export

Exporting without default means it's a "named export". You can have multiple named exports in a single file.
`export class Template extends React.Component  {}`

`export class AnotherTemplate extends React.Component  {}`

then you have to import these exports using their exact names.

`import {Template, AnotherTemplate} from './components/templates'`

<br>

And you can import default and named exports at the same time,

`import Template,{AnotherTemplate} from './components/templates'`



## Context

Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or preferred language.

For example, in the code below we manually thread through a “theme” prop in order to style the Button component:

```js
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // The Toolbar component must take an extra "theme" prop
  // and pass it to the ThemedButton. This can become painful
  // if every single button in the app needs to know the theme
  // because it would have to be passed through all components.
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```



Using context, we can avoid passing props through intermediate elements:

```js
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```



**React.createContext**

```js
const MyContext = React.createContext(defaultValue);
```

Creates a Context object. When React renders a component that subscribes to this Context object it will read the current context value from the closest matching `Provider` above it in the tree.



**Context.Provider**

```js
<MyContext.Provider value={/* some value */}>
```

Every Context object comes with a Provider React component that allows consuming components to subscribe to context changes.

Accepts a `value` prop to be passed to consuming components that are descendants of this Provider. One Provider can be connected to many consumers. Providers can be nested to override values deeper within the tree.

All consumers that are descendants of a Provider will re-render whenever the Provider’s `value` prop changes.



**Class.contextType**

```js
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* perform a side-effect at mount using the value of MyContext */
  }
  render() {
    let value = this.context;
    /* render something based on the value of MyContext */
  }
}
MyClass.contextType = MyContext;
```

The `contextType` property on a class can be assigned a Context object created by [`React.createContext()`](https://reactjs.org/docs/context.html#reactcreatecontext). This lets you consume the nearest current value of that Context type using `this.context`. You can reference this in any of the lifecycle methods including the render function.  
<br />
> If you are using the experimental [public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use a **static** class field to initialize your `contextType`.

```js
class MyClass extends React.Component {
  static contextType = MyContext;
  render() {
    let value = this.context;
  }
}
```



**Context.Consumer**

```js
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```

A React component that subscribes to context changes. This lets you subscribe to a context within a [function component](https://reactjs.org/docs/components-and-props.html#function-and-class-components). Requires a [function as a child](https://reactjs.org/docs/render-props.html#using-props-other-than-render). The function receives the current context value and returns a React node. 

**Diff between contextType and Consumer**

One major difference between Consumer and static context is the fact that using contextType allows you use context outside of render method too.  

> contextType - It works with class Component. You can only subscribe to a single context using this api.

> consumer - Using this component lets you subscribe to a context within a function component. Requires a function as a child. 


**Updating Context from a Nested Component**

It is often necessary to update the context from a component that is nested somewhere deeply in the component tree. In this case you can pass a function down through the context to allow consumers to update the context:

```js
// Make sure the shape of the default value passed to
// createContext matches the shape that the consumers expect!
export const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {},
});
```

```js
import {ThemeContext} from './theme-context';

function ThemeTogglerButton() {
  // The Theme Toggler Button receives not only the theme
  // but also a toggleTheme function from the context
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button
          onClick={toggleTheme}
          style={{backgroundColor: theme.background}}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  );
}

export default ThemeTogglerButton;
```

**Consuming Multiple Contexts**

To keep context re-rendering fast, React needs to make each context consumer a separate node in the tree.

```js
// Theme context, default to light theme
const ThemeContext = React.createContext('light');

// Signed-in user context
const UserContext = React.createContext({
  name: 'Guest',
});

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // App component that provides initial context values
    return (
      <ThemeContext.Provider value={theme}>
        <UserContext.Provider value={signedInUser}>
          <Layout />
        </UserContext.Provider>
      </ThemeContext.Provider>
    );
  }
}

function Layout() {
  return (
    <div>
      <Sidebar />
      <Content />
    </div>
  );
}

// A component may consume multiple contexts
function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```


## Error Boundaries

A JavaScript error in a part of the UI shouldn’t break the whole app.  Error boundaries are React components that **catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI** instead of the component tree that crashed.

Error boundaries do not catch errors for:

1. Event handlers (learn more)
2. Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)

A class component becomes an error boundary if it defines either (or both) of the lifecycle methods [`static getDerivedStateFromError()`](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror) or [`componentDidCatch()`](https://reactjs.org/docs/react-component.html#componentdidcatch). Use `static getDerivedStateFromError()` to render a fallback UI after an error has been thrown. Use `componentDidCatch()` to log error information.

```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

Then you can use it as a regular component:

```js
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

Error boundaries work like a JavaScript `catch {}` block, but for components. Only class components can be error boundaries. In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.

Note that error boundaries only catch errors in the components below them in the tree. An error boundary can’t catch an error within itself. If an error boundary fails trying to render the error message, the error will propagate to the closest error boundary above it.

You may wrap top-level route components to display a “Something went wrong” message to the user, just like server-side frameworks often handle crashes. You may also wrap individual widgets in an error boundary to protect them from crashing the rest of the application.



## Refs

In the typical React dataflow, [props](https://reactjs.org/docs/components-and-props.html) are the only way that parent components interact with their children. To modify a child, you re-render it with new props. However, there are a few cases where you need to imperatively modify a child outside of the typical dataflow. The child to be modified could be an instance of a React component, or it could be a DOM element. For both of these cases, React provides an escape hatch.



Here are a few good use cases for refs:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

**Refs are created using `React.createRef()` and attached to React elements via the `ref` attribute.** Refs are commonly assigned to an instance property when a component is constructed so they can be referenced throughout the component.

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

**When a ref is passed to an element in `render`, a reference to the node becomes accessible at the `current` attribute of the ref.**

```js
const node = this.myRef.current;
```

The value of the ref differs depending on the type of the node:

- When the `ref` attribute is used on an HTML element, the `ref` created in the constructor with `React.createRef()` receives the underlying DOM element as its `current` property.
- When the `ref` attribute is used on a custom class component, the `ref` object receives the mounted instance of the component as its `current`.
- You may not use the ref attribute on function components because they don’t have instances.



**Example -**

This code uses a `ref` to store a reference to a DOM node

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

React will assign the `current` property with the DOM element when the component mounts, and assign it back to `null` when it unmounts. `ref` updates happen before `componentDidMount` or `componentDidUpdate` lifecycle methods.

**Adding a Ref to a Class Component -** If we wanted to wrap the `CustomTextInput` above to simulate it being clicked immediately after mounting, we could use a ref to get access to the custom input and call its `focusTextInput` method manually:

```js
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />
    );
  }
}
```



> In rare cases, you might want to have access to a child’s DOM node from a parent component. This is generally not recommended because it breaks component encapsulation, but it can occasionally be useful for triggering focus or measuring the size or position of a child DOM node.
>
> While you could [add a ref to the child component](https://reactjs.org/docs/refs-and-the-dom.html#adding-a-ref-to-a-class-component), this is not an ideal solution, as you would only get a component instance rather than a DOM node. If you use React 16.3 or higher, we recommend to use [ref forwarding](https://reactjs.org/docs/forwarding-refs.html) for these cases. Ref forwarding lets components opt into exposing any child component’s ref as their own.



## Ref forwarding

Ref forwarding is a technique for automatically passing a [ref](https://reactjs.org/docs/refs-and-the-dom.html) through a component to one of its children.

Consider a `FancyButton` component that renders the native `button` DOM element:

```js
function FancyButton(props) {
  return (
    <button className="FancyButton">
      {props.children}
    </button>
  );
}
```

**Ref forwarding is an opt-in feature that lets some components take a ref they receive, and pass it further down (in other words, “forward” it) to a child.**

In the example below, `FancyButton` uses `React.forwardRef` to obtain the `ref` passed to it, and then forward it to the DOM `button` that it renders:

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

This way, components using `FancyButton` can get a ref to the underlying `button` DOM node and access it if necessary—just like if they used a DOM `button` directly.

Here is a step-by-step explanation of what happens in the above example:

1. We create a [React ref](https://reactjs.org/docs/refs-and-the-dom.html) by calling `React.createRef` and assign it to a `ref` variable.
2. We pass our `ref` down to `<FancyButton ref={ref}>` by specifying it as a JSX attribute.
3. React passes the `ref` to the `(props, ref) => ...` function inside `forwardRef` as a second argument.
4. We forward this `ref` argument down to `<button ref={ref}>` by specifying it as a JSX attribute.
5. When the ref is attached, `ref.current` will point to the `<button>` DOM node.



## Render Props

The term "render prop"  refers to a technique for sharing code between React components using a prop whose value is a function.

A component with a render prop takes a function that returns a React element and calls it instead of implementing its own render logic.

```js
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

Render props comes handy when we want to share the same state across components. It’s also worth noting that the name of the prop does not need to be `render`.

Render props are often compared to higher-order components since they serve a similar purpose.



We have learned in the past posts, how we can pass data to child components from parent components. The child components capture the data in the `props` object argument:

```js
class ChildComp extends React.Component {
    constructor(props) { }
    render() {
        return <div>{this.props.name}</div>
    }
}<ChildComp name="nnamdi" />
```

Objects, arrays, booleans, strings, Numbers can be passed to child components via props. Now, we can pass a function to the props object.

```js
class ChildComp extends React.Component {
    constructor(props) { }
    render() {
        return <div>{this.props.name()}</div>
    }
}<ChildComp name={() => [ "nnamdi", "chidume" ]} />
```

You see we passed a function `() => [ "nnamdi", "chidume" ]` to `ChildComponent` via `name`,then it can access it by referencing it as key in the props argument: `this.props.name`.



**Example 1 -** 

For example, let’s say we have a component that renders a blog post:

```js
class CommentList extends React.Component {
    render() {
        return <Comment comment={} />
    }
}class BlogPost extends React.Component {
    render() {
        return (
            <div>
                <TextBlock text={} />
                <CommentList />
            </div>
        )
    }
}
```

This pattern makes CommentList tightly coupled to BlogPost. Its valid this way though. If there is no strong use case of having more control over what is rendered within the BlogPost component, we can use it. What if we can move the CommentList outside the BlogPost?

Here is where render prop comes in: Instead of hard coding CommentList inside BlogPost, we can provide BlogPost with a function prop that uses it to determine what to render.

```js
class CommentList extends React.Componen {
    render() {
        return <Comment comment={} />
    }
}class BlogPost extends React.Component {
    render() {
        return (
            <div>
                <TextBlock text={} />
                {this.props.render()}
            </div>
        )
    }
}<BlogPost render={()=><CommentList/>}/>
```

This technique makes the behavior in BlogPost extremely configurable. We can render anything apart from comments.



**Example 2 -**

Lets say we have a Component that tracks the mouse position in a web app. And we have a `<Cat>` component that renders the image of a cat chasing the mouse around the screen. We can do this - 

```js
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class MouseWithCat extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          We could just swap out the <p> for a <Cat> here ... but then
          we would need to create a separate <MouseWithSomethingElse>
          component every time we need to use it, so <MouseWithCat>
          isn't really reusable yet.
        */}
        <Cat mouse={this.state} />
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <MouseWithCat />
      </div>
    );
  }
}
```

This approach will work for our specific use case, but we haven’t achieved the objective of truly encapsulating the behavior in a reusable way. Now, every time we want the mouse position for a different use case, we have to create a new component (essentially another `<MouseWithCat>`) that renders another component.

Here’s where the render prop comes in --- Instead of hard-coding a `<Cat>` inside a `<Mouse>` component, and effectively changing its rendered output, we can provide `<Mouse>` with a function prop that it uses to dynamically determine what to render–a render prop.

```js
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

One interesting thing to note about render props is that you can implement most higher-order components (HOC) using a regular component with a render prop. For example, if you would prefer to have a withMouse HOC instead of a <Mouse> component, you could easily create one using a regular <Mouse> with a render prop.

```js
// If you really want a HOC for some reason, you can easily
// create one using a regular component with a render prop!
function withMouse(Component) {
  return class extends React.Component {
    render() {
      return (
        <Mouse render={mouse => (
          <Component {...this.props} mouse={mouse} />
        )}/>
      );
    }
  }
}
```



## Higher-Order Components

A higher-order component (HOC) is an advanced technique in React for reusing component logic.  
A higher-order component **is a function that takes a component and returns a new component.**  The main purpose of a higher-order component in React is to share common functionality between components without repeating code. It transforms a component into another component and adds additional data or functionality.



***When to use?* -** When you’re repeating patterns/logic across components. Examples -

- *hooking into/subscribing to a data source -* Say you have three JSON files in your application. These files contain different data that will be loaded in your application in three different components. You want to give your users the ability to *search* the data loaded from these files. You could implement a search feature in all three of the components. A better way forward is to create a higher-order component to handle the search functionality. With it, you can wrap the other components individually in your higher-order component.
- *adding interactivity to UI (also achieved with wrapper/render props)*
- *sorting/filtering input data*




Let’s look at an example of the most simple HOC possible.

```js
 // Take in a component as argument WrappedComponent
    function simpleHOC(WrappedComponent) {
      // And return a new anonymous component
      return class extends React.Component{
        render() {
          return <WrappedComponent {...this.props}/>;
        }
      }
    }
```

This HOC takes a React component, WrappedComponent, as parameter. It returns a new React component. The returned component contains the WrappedComponent as a child.

We use the HOC to create a new component like this:

```js
    // Create a new component
    const NewComponent = simpleHOC(Hello);

    // NewComponent can be used exactly like any component
    // In this case, NewComponent is functionally the same as Hello
    <NewComponent/>
```

<br>

**Don’t modify or mutate the component -** It's for creating new ones.

**Don’t Use HOCs Inside the render Method -** Instead, apply HOCs outside the component definition so that the resulting component is created only once. Then, its identity will be consistent across renders. This is usually what you want, anyway. In those rare cases where you need to apply a HOC dynamically, you can also do it inside a component’s lifecycle methods or its constructor.

**Refs Aren’t Passed Through -** While the convention for higher-order components is to pass through all props to the wrapped component, this does not work for refs. That’s because `ref` is not really a prop — like `key`, it’s handled specially by React. If you add a ref to an element whose component is the result of a HOC, the ref refers to an instance of the outermost container component, not the wrapped component. The solution for this problem is to use the `React.forwardRef` API (introduced with React 16.3).



#### Example -

**Example 1**

For example, say you have a CommentList component that subscribes to an external data source to render a list of comments. Later, you write a component for subscribing to a single blog post, which follows a similar pattern.

CommentList and BlogPost aren’t identical — they call different methods on DataSource, and they render different output. But much of their implementation is the same:

1. On mount, add a change listener to DataSource.
2. Inside the listener, call setState whenever the data source changes.
3. On unmount, remove the change listener.

You can imagine that in a large app, this same pattern of subscribing to DataSource and calling setState will occur over and over again. We want an abstraction that allows us to define this logic in a single place and share it across many components. This is where higher-order components excel.

We can write a function that creates components, like CommentList and BlogPost, that subscribe to DataSource. The function will accept as one of its arguments a child component that receives the subscribed data as a prop. Let’s call the function withSubscription:

```js
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

The first parameter is the wrapped component. The second parameter retrieves the data we’re interested in, given a DataSource and the current props.

When CommentListWithSubscription and BlogPostWithSubscription are rendered, CommentList and BlogPost will be passed a data prop with the most current data retrieved from DataSource

```js
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
Note that a HOC doesn’t modify the input component, nor does it use inheritance to copy its behavior. Rather, a HOC composes the original component by wrapping it in a container component. *A HOC is a pure function with zero side-effects*


**Example 2**

Say we’re about to start work on our app’s user page. We know what our *User* object looks like, but we haven’t quite decided what kind of authorization we’d like to use.  We can start with a simple HOC function named `withUser`. We want this function to wrap around any component we pass to it and provide our User object as a prop.

```js
const withUser = WrappedComponent => {
  const user = sessionStorage.getItem("user");
  return props => <WrappedComponent user={user} {...props} />;
};
```

When we want to access the User object on our page, we can call `withUser` to wrap the page’s component:

```js
const UserPage = props => (
  <div class="user-container">
    <p>My name is {props.user}!</p>
  </div>
);

export default withUser(UserPage);
```

And that does it! Our `withUser` function takes a component as an argument and returns a higher order component. Three months from now, if we decide to change things around, we only have to edit our HOC.



## Portals

Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```js
ReactDOM.createPortal(child, container)
```

The first argument (`child`) is any [renderable React child](https://reactjs.org/docs/react-component.html#render), such as an element, string, or fragment. The second argument (`container`) is a DOM element.

A typical use case for portals is when a parent component has an `overflow: hidden` or `z-index` style, but you need the child to visually “break out” of its container. For example, dialogs, hovercards, and tooltips.

Even though a portal can be anywhere in the DOM tree, it behaves like a normal React child in every other way. Features like context work exactly the same regardless of whether the child is a portal, as the portal still exists in the *React tree* regardless of position in the *DOM tree*. This includes event bubbling. **An event fired from inside a portal will propagate to ancestors in the containing *React tree*, even if those elements are not ancestors in the *DOM tree***.

For example, if you render a `<Modal />` component, the parent can capture its events regardless of whether it’s implemented using portals.


## HOOKS
Let's you use state and other React features without writing a class

Hooks are JavaScript functions, but they impose two additional rules:
* `Only call Hooks at the top level`. Don’t call Hooks inside loops, conditions, or nested functions.
* Only call Hooks from React function components. `Don’t call Hooks from regular JavaScript functions`. (You can call Hooks from your own custom Hooks)


### useState

useState lets you use local state within a function component.

```js
import React, { useState } from 'react';

const [state, stateUpdaterFunction] = useState(initialStateValue)
```

When we declare a state variable with useState, it returns a pair — an array with two items. The first item is the current value, and the second is a function that lets us update it. Using [0] and [1] to access them is a bit confusing because they have a specific meaning. This is why we use array destructuring instead.

Equivalent to -
```js
const something = useState(initialStateValue);
var state = something[0];
var stateUpdaterFunction = something[1];
```


**Difference between this.state and state hook** 

- Unlike this.state, `the state here doesn’t have to be an object` — although it can be if you want.

- Unlike this.setState in a class, updating a state variable using hook always `replaces it instead of merging it`. To merge, you can pass a function that receives the previous state -
```js
const [user, setUser] = useState(
    { name: 'John', email: 'john@example.com', age: 25 }
  );
  setUser((prevState) => ({ ...prevState, name: 'Harry' }));
  ```


useState can be used once or multiple times within a single component.

State could also be initialised from a function

```js
const [token] = useState(() => {
    let token = window.localStorage.getItem("my-token");
    return token || "default#-token#"
  })
  ```

If the `new state is computed using the previous state, you can pass a function to setState`. The function will receive the previous value, and return an updated value.

```js
const [count, setCount] = useState(initialCount);
<button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
```


> Unlike the setState method found in class components, useState does not automatically merge update objects. You can replicate this behaviour by combining the function updater form with object spread syntax
```js
const [state, setState] = useState({});
setState(prevState => {
  // Object.assign would also work
  return {...prevState, ...updatedValues};
});
```
Another option is useReducer, which is more suited for managing state objects that contain multiple sub-values.


### useEffect

You’ve likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations “side effects” (or “effects” for short) because they can affect other components and can’t be done during rendering.

useEffect accepts a function which can perform any side effects. It serves the same purpose as componentDidMount, 
componentDidUpdate, and componentWillUnmount in React classes, but unified into a single API.

```js
useEffect(effectFunction, arrayDependencies)
```

- Without arrayDependencies, React runs the effects after every render — including the first render.

- With empty array [] as dependency, effect will run only once.

- If you want to skip applying an effect if certain values haven’t changed between re-renders
```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```

To `cleanup an effect, return a function from within the effect` function passed to useEffect

```js
  useEffect(() => {
    const clicked = () => console.log('window clicked')
    window.addEventListener('click', clicked)

    // return a clean-up function
    return () => {
      window.removeEventListener('click', clicked)
    }
  }, [])
```

Every effect may return a function that cleans up after it. This lets us keep the logic for adding and removing subscriptions close to each other.
Cleans up effects from the previous render before running the effects next time.


**Skipping Effects**

In some cases, cleaning up or applying the effect after every render might create a performance problem. In class components, we can solve this by writing an extra comparison with prevProps or prevState inside componentDidUpdate:

```js
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
```

You can tell React to skip applying an effect if certain values haven’t changed between re-renders. To do so, pass an array as an optional second argument to useEffect

```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```

If you want to run an effect only once, you can pass an empty array ([]) as a second argument. This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run. Without an array dependency, the effect function will be run after every single render.


**Use Multiple Effects to Separate Concerns**
class lifecycle methods often contain unrelated logic. Logic gets split between componentDidMount, componentDidUpdate and others.
Hooks let us split the code based on what it is doing rather than a lifecycle method name.


> Unlike componentDidMount or componentDidUpdate, effects scheduled with useEffect `don’t block the browser from updating the screen`. This makes your app feel more responsive. The majority of effects don’t need to happen synchronously. In the uncommon cases where they do (such as measuring the layout), there is a separate useLayoutEffect Hook with an API identical to useEffect.


### Custom Hooks

Sometimes, we want to reuse some stateful logic between components. Traditionally, there were two popular solutions to this problem: *higher-order components* and *render props*. Custom Hooks let you do this, but without adding more components to your tree.

```js
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

Now we can use it from both components:

```js
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);
  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);
return (
)
}
```

The state of each component is completely independent. Hooks are a way to reuse stateful logic, not state itself. In fact, `each call to a Hook has a completely isolated state` — so you can even use the same custom Hook twice in one component.

Custom Hooks are more of a convention than a feature. If a function’s name starts with ”use” and it calls other Hooks, we say it is a custom Hook.

Since Hooks are functions, `we can pass information between them`.



### Hooks Tips

**How to get the previous props or state?**

Sometimes, you need previous props to clean up an effect. For example, you might have an effect that subscribes to a socket based on the userId prop. If the userId prop changes, you want to unsubscribe from the previous userId and subscribe to the next one. 

```js
useEffect(() => {
  ChatAPI.subscribeToSocket(props.userId);
  return () => ChatAPI.unsubscribeFromSocket(props.userId);
}, [props.userId]);
```

In the above example, if userId changes from 3 to 4, ChatAPI.unsubscribeFromSocket(3) will run first, and then ChatAPI.subscribeToSocket(4) will run. There is no need to get “previous” userId because the cleanup function will capture it in a closure.


**Can I run an effect only on updates?**
If we want to restrict useEffect to run only when the component mounts, we can add second parameter of useEffect with [].
```js
useEffect(() => {
  // ...
}, []);
```

If you want the useEffect to run only on updates and not on initial mount

you can make `use of useRef to keep track of initialMount` with useEffect without the second parameter.

```js
const isInitialMount = useRef(true);
useEffect(() => {
  if (isInitialMount.current) {
     isInitialMount.current = false;
  } else {
      // Your useEffect code here to be run on update
  }
});
```


### Hook Lifecycle Comparison
How do lifecycle methods correspond to Hooks?
* constructor: Function components don’t need a constructor. You can initialize the state in the useState call. If computing the initial state is expensive, you can pass a function to useState.
* getDerivedStateFromProps: Schedule an update while rendering instead.
* shouldComponentUpdate: See React.memo below.
* render: This is the function component body itself.
* componentDidMount, componentDidUpdate, componentWillUnmount: The useEffect Hook can express all combinations of these (including less common cases).
* getSnapshotBeforeUpdate, componentDidCatch and getDerivedStateFromError: There are no Hook equivalents for these methods yet, but they will be added soon.



## React lifecycle Methods

Cheatsheet

![React lifecycle methods diagram](https://docs.salmanfarooqui.com/REACT/images/lifecycle-cheatsheet.png)

...