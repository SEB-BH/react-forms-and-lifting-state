<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Lifting State</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to lift state into a parent component and pass state and functions to child components through props.

## Starting point

Previously, we built a `Form` component that:

* Stores the current input values in `formData`
* Stores submitted contacts in `contacts`
* Adds a contact inside `handleSubmit`
* Maps through the contacts and displays them

This works, but the `Form` component is now responsible for two different jobs:

1. Collecting information from the user
2. Displaying the submitted contacts

We want to separate these responsibilities.

## Our new component structure

We will create three components:

```txt
App
├── Form
└── Contacts
```

Each component will have a separate responsibility:

* `App` stores the contacts
* `Form` creates new contacts
* `Contacts` displays the contacts

However, this creates a problem.

`Form` needs to update the contacts, while `Contacts` needs to read the contacts.

Sibling components cannot directly share state with one another:

```txt
Form        Contacts
  ✖────────────✖
```

Instead, we place the state in their closest shared parent:

```txt
           App
        contacts
        /      \
     Form     Contacts
```

Moving state into a shared parent component is called **lifting state**.


## Step 1: Move `contacts` into `App`

Open `App.jsx` and import `useState`.

Create the `contacts` state inside `App`:

```jsx
import { useState } from 'react'
import Form from './components/Form/Form'
import Contacts from './components/Contacts/Contacts'

const App = () => {
  const [contacts, setContacts] = useState([])

  return (
    <div>
      <h1>Contact List</h1>

      <Form />
      <Contacts />
    </div>
  )
}

export default App
```

The contacts array now belongs to `App`.

```jsx
const [contacts, setContacts] = useState([])
```

Remove this line from the `Form` component:

```jsx
const [contacts, setContacts] = useState([])
```

The `formData` state should remain inside `Form`:

```jsx
const [formData, setFormData] = useState(initialState)
```

Only `Form` needs to know what the user is currently typing, so there is no reason to lift `formData`.


## Step 2: Pass state to `Contacts`

The `Contacts` component needs the contacts array so it can display each contact.

Pass `contacts` from `App` to `Contacts` as a prop:

```jsx
<Contacts contacts={contacts} />
```

Our `App` component now looks like this:

```jsx
import { useState } from 'react'
import Form from './components/Form/Form'
import Contacts from './components/Contacts/Contacts'

const App = () => {
  const [contacts, setContacts] = useState([])

  return (
    <div>
      <h1>Contact List</h1>

      <Form />
      <Contacts contacts={contacts} />
    </div>
  )
}

export default App
```

The data moves down from `App` to `Contacts`:

```txt
App
│
│ contacts
▼
Contacts
```

## Step 3: Create the `Contacts` component

Create a new file:

```txt
src/components/Contacts/Contacts.jsx
```

The `Contacts` component receives the contacts through props:

```jsx
const Contacts = ({ contacts }) => {
  return (
    <div>
      <h2>Contacts</h2>

      {contacts.map((contact) => (
        <div key={contact.id}>
          <h3>{contact.name}</h3>
          <p>Email: {contact.email}</p>
          <p>Phone: {contact.phone}</p>
        </div>
      ))}
    </div>
  )
}

export default Contacts
```

We destructure `contacts` from the props object:

```jsx
const Contacts = ({ contacts }) => {
```

We can then map through the array:

```jsx
contacts.map((contact) => (
```

The `Contacts` component does not change the array. It only receives and displays it.


## Step 4: Pass the updater function to `Form`

The `contacts` state now belongs to `App`, but `Form` needs to add new contacts to it.

We can pass both `contacts` and `setContacts` to `Form` as props:

```jsx
<Form
  contacts={contacts}
  setContacts={setContacts}
/>
```

Our updated `App.jsx` is:

```jsx
import { useState } from 'react'
import Form from './components/Form/Form'
import Contacts from './components/Contacts/Contacts'

const App = () => {
  const [contacts, setContacts] = useState([])

  return (
    <div>
      <h1>Contact List</h1>

      <Form
        contacts={contacts}
        setContacts={setContacts}
      />

      <Contacts contacts={contacts} />
    </div>
  )
}

export default App
```

Notice that both child components receive information from `App`:

```txt
                 App
          contacts, setContacts
             /           \
            ▼             ▼
          Form         Contacts
```

`Form` receives:

* `contacts`
* `setContacts`

`Contacts` receives:

* `contacts`


## Step 5: Receive the props in `Form`

Update the beginning of the `Form` component:

```jsx
const Form = ({ contacts, setContacts }) => {
```

The component can now use the contacts state that belongs to `App`.

The `handleSubmit` function can stay mostly the same:

```jsx
const handleSubmit = (event) => {
  event.preventDefault()

  const newContact = {
    ...formData,
    id: crypto.randomUUID(),
  }

  setContacts([...contacts, newContact])
  setFormData(initialState)
}
```

Although `handleSubmit` runs inside `Form`, `setContacts` updates the state inside `App`.


## Completed component structure

```txt
src
├── components
│   ├── Contacts
│   │   └── Contacts.jsx
│   └── Form
│       └── Form.jsx
├── App.jsx
└── main.jsx
```

## Why did we lift `contacts` but not `formData`?

State should live in the closest component that needs to control it.

Only `Form` needs the current form values:

```jsx
const [formData, setFormData] = useState(initialState)
```

Therefore, `formData` stays inside `Form`.

Both `Form` and `Contacts` need access to the contacts array:

* `Form` adds contacts
* `Contacts` displays contacts

Therefore, `contacts` moves into their shared parent, `App`.

A useful question to ask is:

> Which components need access to this state?

If multiple sibling components need it, move it into their closest shared parent.


## State versus props

After lifting state, the components use the same information differently.

### In `App`

`contacts` is state:

```jsx
const [contacts, setContacts] = useState([])
```

### In `Form`

`contacts` and `setContacts` are props:

```jsx
const Form = ({ contacts, setContacts }) => {
```

### In `Contacts`

`contacts` is a prop:

```jsx
const Contacts = ({ contacts }) => {
```

The same value can be state in one component and a prop in another component.


## You Do

Add a component called `ContactCount`.

It should display the number of contacts:

```txt
Total contacts: 3
```

Create the component:

```jsx
const ContactCount = ({ contacts }) => {
  return (
    <p>Total contacts: {contacts.length}</p>
  )
}

export default ContactCount
```

Import it into `App` and pass it the contacts array:

```jsx
<ContactCount contacts={contacts} />
```

Your component hierarchy will now be:

```txt
App
├── Form
├── ContactCount
└── Contacts
```

All three child components use the contacts state owned by `App`.
