# Understanding React key

> Keys give the react elements a stable `identity`  

## warning in array without key

```jsx
import React from 'react';

function App() {
  const persons = [‘mike’, ‘jason’, ‘sharky’];
  return <ul>
    {persons.map(p => {
      return <li>{p}</li>
    })}
  </ul>
}
```

React will throw a warning with the code above:
`Warning: Each child in a list should have a unique "key" prop.`

## What keys to use? 

Keys should be `unique` and `stable`

### Index ? Maybe, but only `under some circumstances`

Array index is unique across an array.

```jsx
import React, { useState } from 'react';

function App() {
  const persons = [‘mike’, ‘jason’, ‘sharky’];
  return <ul>
    {persons.map((p, index) => {
      return <li key={index}>{p}</li>
    })}
  </ul>
}
```

However,  if the order/number of items may change, array index is NOT `stable`.

```jsx
import React, { useState } from 'react';

function App() {
	const [persons, setPersons] = useState(
    ['mike', 'jason', 'sharky']
  );
  
  const onAdd = () => {
    setPersons(['fishman', ...persons])
  }
  return <>
    <button onClick={onAdd}> Add fishman on top </button>
    <ul>
      {persons.map((p, index) => {
        return <li key={index}><input type="checkbox"/>{p}</li>
        })}
      </ul>
  </>
}
```

[array-index-as-key demo](https://codesandbox.io/s/staging-resonance-vy7eq)

Ticking `mike` and then clicking the add button will result in the loss of check status of `mike`

### `Stable` key

Usually we will have a unique and stable `id` like property for each record from backend/database.

```jsx
import React, { useState } from ‘react’;

export default function App() {
  const [persons, setPersons] = useState(
    [
      {id: 'uywoejk', name: 'mike'},
      {id: 'woeioqj', name: 'jason'},
      {id: 'eljlkqd', name: 'sharky'}
    ]
  );
  
  const onAdd = () => {
    setPersons([{
      id: 'wuioeioe', name: 'fishman'
    }, ...persons])
  }
  return <>
    <button onClick={onAdd}> Add fishman on top </button>
    <ul>
      {
        persons.map((p, index) => {
          return <li key={p.id}><input type="checkbox"/>{p.name}</li>
        })
      }
    </ul>
  </>
}
```

[stable-key demo](https://codesandbox.io/s/gallant-visvesvaraya-0b3lo)

## Use key to unmount component

Use case: I have a select and input component. Each the select option changes, reset the input to default state,

```jsx
import React, { useState } from “react”;

export default function App() {
  const [option, setOption] = useState("");
  return (
    <>
      <select onChange={e => setOption(e.target.value)}>
        <option key="a">A</option>
        <option key="b">B</option>
      </select>
      <input defaultValue="this is default"/>
      <input key={option} defaultValue="this is default" />
    </>
  );
}
```

[react-key demo](https://codesandbox.io/s/react-key-yth0y?file=/src/App.js:0-97)

## Conclusion

* `key` is an unique identifier for react element
* `key` should be `unique` and `stable`. A react element with different `key` in different render phases will be considered as different elements. The old one will be unmounted and a new one is created.
* Only use array indices as keys when the number/order of array items are unchanged across the whole app lifecycle.
* Always prefer to use backend/database unique identifier id like property as `key`

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
