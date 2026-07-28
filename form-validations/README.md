<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Form Validation</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to prevent invalid form submissions and display a helpful error message.

## What is form validation?

Form validation checks the user's information before allowing the form to be submitted.

For our contact form, we want to make sure:

* The name is not empty
* The email is not empty
* The phone number is not empty

If information is missing, we will display an error message instead of adding the contact.

---

## Add built-in validation

HTML provides some validation through input attributes.

Add `required` to each input:

```jsx
Name:
<input
  type="text"
  name="name"
  value={formData.name}
  onChange={handleChange}
  required
/>

Email:
<input
  type="email"
  name="email"
  value={formData.email}
  onChange={handleChange}
  required
/>

Phone:
<input
  type="text"
  name="phone"
  value={formData.phone}
  onChange={handleChange}
  required
/>
```

The `required` attribute prevents the form from being submitted when an input is empty.

The email input also uses:

```jsx
type="email"
```

This asks the browser to check whether the value looks like an email address.

For example:

```txt
mona@example.com
```

The browser would reject a value such as:

```txt
mona
```

This is called **built-in browser validation**.

---

## Add an error message state

We can also create our own validation using React.

Inside `Form`, create state for an error message:

```jsx
const [errorMessage, setErrorMessage] = useState('')
```

The starting value is an empty string because there is no error when the component first loads.

```jsx
const Form = ({ contacts, setContacts }) => {
  const [formData, setFormData] = useState(initialState)
  const [errorMessage, setErrorMessage] = useState('')
```

---

## Validate the form in `handleSubmit`

Add a check near the beginning of `handleSubmit`:

```jsx
const handleSubmit = (event) => {
  event.preventDefault()

  if (
    formData.name.trim() === '' ||
    formData.email.trim() === '' ||
    formData.phone.trim() === ''
  ) {
    setErrorMessage('Please complete every field.')
    return
  }

  const newContact = {
    ...formData,
    id: crypto.randomUUID(),
  }

  setContacts([...contacts, newContact])
  setFormData(initialState)
}
```

The `if` statement checks whether any field is empty.

```jsx
formData.name.trim() === ''
```

The `.trim()` method removes extra spaces from the beginning and end of a string.

Without `.trim()`, a user could enter only spaces:

```txt
"     "
```

That value is technically not an empty string.

After using `.trim()`, it becomes:

```txt
""
```

---

## Why do we use `return`?

If the form is invalid, we run:

```jsx
return
```

This immediately stops `handleSubmit`.

The code below the `return` will not run, so the invalid contact will not be added to the array.

```jsx
if (formData.name.trim() === '') {
  setErrorMessage('Please enter a name.')
  return
}

const newContact = {
  ...formData,
  id: crypto.randomUUID(),
}
```

Without `return`, React would show the error but continue adding the contact.

---

## Display the error message

Place the error message above the form:

```jsx
<h2>Add a Contact</h2>

{errorMessage && (
  <p>{errorMessage}</p>
)}

<form onSubmit={handleSubmit}>
```

This uses conditional rendering.

If `errorMessage` is an empty string, nothing is displayed.

If it contains a message, the paragraph appears:

```txt
Please complete every field.
```

---

## Clear the error after a successful submission

When the form is successfully submitted, clear the old error message:

```jsx
setErrorMessage('')
```

Add it after updating the contacts:

```jsx
const handleSubmit = (event) => {
  event.preventDefault()

  if (
    formData.name.trim() === '' ||
    formData.email.trim() === '' ||
    formData.phone.trim() === ''
  ) {
    setErrorMessage('Please complete every field.')
    return
  }

  const newContact = {
    ...formData,
    id: crypto.randomUUID(),
  }

  setContacts([...contacts, newContact])
  setFormData(initialState)
  setErrorMessage('')
}
```

---

## Avoid saving extra spaces

We can also use `.trim()` when creating the new contact:

```jsx
const newContact = {
  id: crypto.randomUUID(),
  name: formData.name.trim(),
  email: formData.email.trim(),
  phone: formData.phone.trim(),
}
```

Suppose the user enters:

```txt
"  Mona  "
```

The saved contact will contain:

```txt
"Mona"
```

This keeps our data cleaner.

---

## Updated `handleSubmit`

```jsx
const handleSubmit = (event) => {
  event.preventDefault()

  if (
    formData.name.trim() === '' ||
    formData.email.trim() === '' ||
    formData.phone.trim() === ''
  ) {
    setErrorMessage('Please complete every field.')
    return
  }

  const newContact = {
    id: crypto.randomUUID(),
    name: formData.name.trim(),
    email: formData.email.trim(),
    phone: formData.phone.trim(),
  }

  setContacts([...contacts, newContact])
  setFormData(initialState)
  setErrorMessage('')
}
```

---

## Updated `Form.jsx`

```jsx
import { useState } from 'react'

const initialState = {
  name: '',
  email: '',
  phone: '',
}

const Form = ({ contacts, setContacts }) => {
  const [formData, setFormData] = useState(initialState)
  const [errorMessage, setErrorMessage] = useState('')

  const handleChange = (event) => {
    setFormData({
      ...formData,
      [event.target.name]: event.target.value,
    })
  }

  const handleSubmit = (event) => {
    event.preventDefault()

    if (
      formData.name.trim() === '' ||
      formData.email.trim() === '' ||
      formData.phone.trim() === ''
    ) {
      setErrorMessage('Please complete every field.')
      return
    }

    const newContact = {
      id: crypto.randomUUID(),
      name: formData.name.trim(),
      email: formData.email.trim(),
      phone: formData.phone.trim(),
    }

    setContacts([...contacts, newContact])
    setFormData(initialState)
    setErrorMessage('')
  }

  return (
    <div>
      <h2>Add a Contact</h2>

      {errorMessage && (
        <p>{errorMessage}</p>
      )}

      <form onSubmit={handleSubmit}>
        Name:
        <input
          type="text"
          name="name"
          value={formData.name}
          onChange={handleChange}
          required
        />

        Email:
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          required
        />

        Phone:
        <input
          type="text"
          name="phone"
          value={formData.phone}
          onChange={handleChange}
          required
        />

        <button type="submit">Add Contact</button>
      </form>
    </div>
  )
}

export default Form
```

---

## Browser validation versus React validation

The form now uses two kinds of validation.

### Browser validation

These attributes are handled by the browser:

```jsx
type="email"
required
```

They provide quick validation without requiring much JavaScript.

### React validation

This code is handled inside `handleSubmit`:

```jsx
if (
  formData.name.trim() === '' ||
  formData.email.trim() === '' ||
  formData.phone.trim() === ''
) {
  setErrorMessage('Please complete every field.')
  return
}
```

React validation gives us more control over:

* What rules we check
* What messages we show
* What happens when the information is invalid

In a real application, the back-end should also validate submitted information. Front-end validation improves the user experience, but it should not be the application's only protection.

---

## You Do

Require the contact's name to contain at least two characters.

Add another check inside `handleSubmit`:

```jsx
if (formData.name.trim().length < 2) {
  setErrorMessage('The name must contain at least two characters.')
  return
}
```

Place this check after checking for empty fields:

```jsx
if (
  formData.name.trim() === '' ||
  formData.email.trim() === '' ||
  formData.phone.trim() === ''
) {
  setErrorMessage('Please complete every field.')
  return
}

if (formData.name.trim().length < 2) {
  setErrorMessage('The name must contain at least two characters.')
  return
}
```

### Bonus

Require the phone number to contain at least eight characters:

```jsx
if (formData.phone.trim().length < 8) {
  setErrorMessage('The phone number must contain at least eight characters.')
  return
}
```
