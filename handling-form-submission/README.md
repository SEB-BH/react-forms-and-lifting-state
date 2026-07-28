<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Handling Form Submission</span>
</h1>

## Add `handleSubmit` to the form

Use the form's `onSubmit` attribute:

```jsx
<form onSubmit={handleSubmit}>
```

It is better to handle submission on the form instead of adding an `onClick` to the button.

This allows the form to be submitted by clicking the button or pressing Enter.

```jsx
<form onSubmit={handleSubmit}>
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

---

## Display the contacts

Use `.map()` to display each contact in the `contacts` array:

```jsx
<div>
  {contacts.map((contact) => (
    <div key={contact.id}>
      <h3>{contact.name}</h3>
      <p>Email: {contact.email}</p>
      <p>Phone: {contact.phone}</p>
    </div>
  ))}
</div>
```

Each contact needs a unique `key`.

We can use the `id` that was created inside `handleSubmit`:

```jsx
key={contact.id}
```

## You Do

Add a `company` input to the contact form.

You will need to update:

1. The `initialState` object
2. The form
3. The displayed contact

The finished contact should display something similar to:

```txt
Mona
Company: General Assembly
Email: mona@example.com
Phone: 12345678
```

### Bonus

Prevent the user from submitting the form when the name is empty.

Inside `handleSubmit`, check the name before adding the contact:

```jsx
if (formData.name === '') {
  return
}
```

The lesson can also be shortened into a code-along version with fewer explanations for live instruction.
