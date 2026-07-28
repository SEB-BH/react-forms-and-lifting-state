<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Give the Form Functionality</span>
</h1>

## Create `handleChange`

Create a function called `handleChange`:

```jsx
const handleChange = (event) => {
  setFormData({
    ...formData,
    [event.target.name]: event.target.value,
  })
}
```

The event gives us information about the input that changed.

```jsx
event.target.name
```

This gives us the input's `name`.

```jsx
event.target.value
```

This gives us what the user typed.

For example, imagine the user types `Mona` into this input:

```jsx
<input name="name" />
```

The event contains:

```jsx
event.target.name
// "name"

event.target.value
// "Mona"
```

The state update becomes:

```jsx
setFormData({
  ...formData,
  name: 'Mona',
})
```

The spread operator copies the other properties so that changing one input does not erase the others.

---

## Add `handleChange` to the inputs

Use the `onChange` attribute to run `handleChange` whenever the user types:

```jsx
<form>
  Name:
  <input
    type="text"
    name="name"
    value={formData.name}
    onChange={handleChange}
  />

  Email:
  <input
    type="email"
    name="email"
    value={formData.email}
    onChange={handleChange}
  />

  Phone:
  <input
    type="text"
    name="phone"
    value={formData.phone}
    onChange={handleChange}
  />

  <button type="submit">Add Contact</button>
</form>
```

The inputs are now controlled by React state.

When the user types:

1. `handleChange` runs
2. `formData` is updated
3. React re-renders the component
4. The input displays the new value

---

## Create the contacts state

We also need somewhere to store the submitted contacts.

Create another piece of state called `contacts`:

```jsx
const [contacts, setContacts] = useState([])
```

The initial value is an empty array because no contacts have been submitted yet.

Our component now has two pieces of state:

```jsx
const [contacts, setContacts] = useState([])
const [formData, setFormData] = useState(initialState)
```

`formData` stores the current form values:

```jsx
{
  name: 'Mona',
  email: 'mona@example.com',
  phone: '12345678',
}
```

`contacts` stores every submitted contact:

```jsx
[
  {
    name: 'Mona',
    email: 'mona@example.com',
    phone: '12345678',
  },
  {
    name: 'Ali',
    email: 'ali@example.com',
    phone: '87654321',
  },
]
```

---

## Create `handleSubmit`

Create a function called `handleSubmit`:

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

Let's break this down.

### Prevent the page from refreshing

Forms normally refresh the page when submitted.

```jsx
event.preventDefault()
```

This prevents the refresh so React can handle the submission.

### Create the new contact

```jsx
const newContact = {
  ...formData,
  id: crypto.randomUUID(),
}
```

This copies the information from `formData` and gives the contact a unique `id`.

### Add the contact to the array

```jsx
setContacts([...contacts, newContact])
```

The spread operator copies the existing contacts into a new array.

The new contact is then added to the end.

### Reset the form

```jsx
setFormData(initialState)
```

This changes each form value back to an empty string.

Because the inputs use `formData` as their values, the inputs will also become empty.