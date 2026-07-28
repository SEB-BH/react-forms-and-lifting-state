<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Create the Form Component</span>
</h1>


**Learning objective:** By the end of this lesson, students will be able to use a controlled form to collect information and add a new object to an array in state.

## Introduction

Forms allow users to enter information into an application.

In this lesson, we will build a form that lets a user add a contact with a name, email address, and phone number.

When the form is submitted, the new contact will be added to an array and displayed on the page.

Our application will use two pieces of state:

* `formData` stores what the user is currently typing
* `contacts` stores the submitted contacts

---

## Starting the component

Begin by creating a `Form` component and importing `useState`:

```jsx
import { useState } from 'react'

const Form = () => {
  return (
    <div>
      <h1>Contact List</h1>
    </div>
  )
}

export default Form
```

Import and render this Form on `App.jsx`.

## Create the form

Next, create a simple form.

We will use regular text instead of `<label>` elements for now.

```jsx
import { useState } from 'react'

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

Each input has a `name` attribute.

```jsx
name="name"
name="email"
name="phone"
```

These names will match the keys in our form state.
