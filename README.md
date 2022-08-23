# React Coding Challenges

## Table of Contents

### Challenge 1 : Convert some HTML to JSX

> **Note**
> Review: What is JSX
>
> JSX is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file.
>
> Rules:
>
> 1. **Return a single root element**
>
> JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array.
>
> 2. **Close all the tags**
>
> JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`
>
> 3. **CamelCase most of the things!**

```javascript
export default function Bio() {
  return (
    <div class="intro">
      <h1>Welcome to my website!</h1>
    </div>
    <p class="summary">
      You can find my thoughts here.
      <br><br>
      <b>And <i>pictures</b></i> of scientists!
    </p>
  );
}
```

#### Solution

- Add fragment
- Adjust tags

```javascript
export default function Bio() {
  return (
    <>
      <div class="intro">
        <h1>Welcome to my website!</h1>
      </div>
      <p class="summary">
        You can find my thoughts here.
        <br />
        <br />
        <b>
          {" "}
          And <i>pictures</i>
        </b> of scientists!
      </p>
    </>
  );
}
```

### Challenge 2: Display array of users to browser

- First to create a file called `users.js`
- [CodeSandbox](https://codesandbox.io/s/display-an-array-ntpnb0)

```javascript
export const users = [
  {
    name: "John",
    age: 12,
  },
  {
    name: "Alex",
    age: 32,
  },
];
```

- Second to import `users` in `App.js`

```javascript
import "./styles.css";
import { users } from "./users";

export default function App() {
  return (
    <div className="App">
      <h1>User Profiles</h1>
      {users.map((user) => (
        <li key={user.name}>
          {user.name}: {user.age} years old
        </li>
      ))}
    </div>
  );
}
```

> **Note**
>
> If we did not provide an unique key props to `li`, it will show warning like this `Warning: Each child in a list should have a unique "key" prop.`
>
> **Rules of keys**
>
> 1. Must be unique among siblings. However, it’s okay to use the same keys for JSX nodes in different arrays.
>
> 2. Must not change.Don’t generate them while rendering.

### Challenge 3: Disable Button

- Set initial state as we need to monitor if there's any value.
- use `useState()` to set an initial value.
- [CodeSandbox](https://codesandbox.io/s/disable-submit-btn-9fste3?file=/src/App.js)

```javascript
import { useState } from "react";
export default function App() {
  // set value
  const [inputTxt, setInputTxt] = useState("");

  const handleChange = (e) => {
    e.preventDefault();
    setInputtxt(e.target.value);
  };

  return (
    <div>
      <input type="text" value={inputTxt} onChange={handleChange} />
      <button disable={inputTxt === ""}>Submit</button>
    </div>
  );
}
```

> **Note**
>
> Review: `useState()`
>
> `useState` is a React Hook that lets you add a state variable to your component.

> **Warning**
>
> Calling the set function does not change the current state in the already executing code, It only affects what useState will return starting from the next render.
>
> [React Docs - useState](https://beta.reactjs.org/apis/react/useState)

### Challenge 4: Show text after typing

- `useState()`
- `useEffect()`
- [CodeSandbox](https://codesandbox.io/s/show-value-after-typing-gsbvys)

```javascript
import { useState, useEffect } from "react";

export default function App() {
  // initial 2 states
  const [text, setText] = useState("");
  const [showText, setShowText] = useState("");

  // use useEffect() and pass text as dependency
  useEffect(() => {
    const timeoutId = setTimeOut(() => {
      setShowText(text);
    }, 300);

    return () => {
      clearTimeout(timeoutId);
    };
  }, [text]);

  return (
    <div className="App">
      <input
        type="text"
        value={text}
        onChange={(e) => setTxt(e.target.value)}
      />
      <p>{showText}</p>
    </div>
  );
}
```

### Challenge 5: Show/hide

- `useState()` to initial state as `true`.
- If `showContent` is `true`, then show content, otherwise, show a smiley face.
- [CodeSandbox](https://codesandbox.io/s/show-hide-2n6ej7?file=/src/App.js)

```javascript
import { useState } from "react";

export default function App() {
  const [showContent, setShowContent] = useState(true);

  const handleClick = () => {
    setShowContent(!showContent);
  };

  return (
    <div>
      <button onClick={handleClick}>
        {showContent ? "Hide Content" : "Show Content"}
      </button>
      {showContent ? <p>Hello, I am here</p> : <p> :) </p>}
    </div>
  );
}
```

### Challenge 6: Adding to an array

- First to initialize state for text.
- Second to initialize an empty array as we will be adding text inside it.
- Use speard operator instead of `push()` to add a todo.

```javascript
import { useState } from "react";

export default function App() {
  const [todo, setTodo] = useState("");
  const [todoLists, setTodoLists] = useState([]);

  const handleChange = (e) => {
    setTodo(e.target.value);
  };

  const handleClick = () => {
    // set id = 0
    let id = 0;
    // clear input after adding
    setTodo("");
    setTodoLists([...todoLists, { id: id++, todo: todo }]);
  };

  render(
    <div>
      <input type="text" value={todo} onChange={handleChange} />
      <button onClick={handleClick}>Add</button>
      <ul>
        {todoLists.map((todoList) => (
          <li key={todoList.id}>{todoList.todo}</li>
        ))}
      </ul>
    </div>
  );
}
```

> **Note**
>
> - Even an array is mutable, we better treat them as immutable when we store in state.
> - **Treat array in React as read-only**, meaning that we shouldn't assign an item inside an array or use methods like `pop()` or `push()`.

### Challenge 7: Deleting item from an array
