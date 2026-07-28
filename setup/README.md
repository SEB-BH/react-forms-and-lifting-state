<h1>
  <span class="headline">React Forms and Lifting State</span>
  <span class="subhead">Setup</span>
</h1>

## Setup

Open your Terminal application and navigate to your `~/code/ga/lectures` directory:

```bash
cd ~/code/ga/lectures
```

Create a new Vite project for your React app:

```bash
npm create vite@latest react-forms-and-lifting-state -- --template react
```

- Select a linter. Use the arrow keys to choose the `ESLint` option and hit `Enter`.

- Select `No` when asked about installation.

Navigate to your new project directory and install the necessary dependencies:

```bash
cd react-forms-and-lifting-state
npm i
```

Open the project's folder in your code editor:

```bash
code .
```

### Update `App.jsx`

Open the `App.jsx` file in the `src` directory and replace the contents of it with the following:

```jsx
import './App.css'

const App = () => {
  return (
    <h1>Hello world!</h1>
  )
}

export default App
```
