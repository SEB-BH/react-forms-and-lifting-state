<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Create the Tnitial State</span>
</h1>


## Create the initial state

Outside the component, create an object called `initialState`.

```jsx
const initialState = {
  name: '',
  email: '',
  phone: '',
}
```

Each property represents one input in the form.

The empty strings represent the starting values of the inputs.

```jsx
import { useState } from 'react'

const initialState = {
  name: '',
  email: '',
  phone: '',
}

const App = () => {
  return (
    <div>
      <h1>Contact List</h1>

      <h2>Add a Contact</h2>

      <form>
        Name:
        <input type="text" name="name" />

        Email:
        <input type="email" name="email" />

        Phone:
        <input type="text" name="phone" />

        <button type="submit">Add Contact</button>
      </form>
    </div>
  )
}

export default App
```

Keeping `initialState` outside the component gives us one object that we can use both when the form first loads and when we reset the form.

---

## Create the form state

Inside the component, create state called `formData`.

```jsx
const [formData, setFormData] = useState(initialState)
```

The value of `formData` starts as:

```jsx
{
  name: '',
  email: '',
  phone: '',
}
```

Add the state to the component:

```jsx
const App = () => {
  const [formData, setFormData] = useState(initialState)

  return (
    <div>
      <h1>Contact List</h1>

      <h2>Add a Contact</h2>

      <form>
        Name:
        <input type="text" name="name" />

        Email:
        <input type="email" name="email" />

        Phone:
        <input type="text" name="phone" />

        <button type="submit">Add Contact</button>
      </form>
    </div>
  )
}
```

---

## Connect the inputs to state

A controlled input gets its value from state.

Add a `value` attribute to each input:

```jsx
<input
  type="text"
  name="name"
  value={formData.name}
/>
```

Do the same for the other inputs:

```jsx
<form>
  Name:
  <input
    type="text"
    name="name"
    value={formData.name}
  />

  Email:
  <input
    type="email"
    name="email"
    value={formData.email}
  />

  Phone:
  <input
    type="text"
    name="phone"
    value={formData.phone}
  />

  <button type="submit">Add Contact</button>
</form>
```

At this point, the inputs display information from state.

However, we cannot type in them yet. We need a function that updates state whenever an input changes.
