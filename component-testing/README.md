<h1>
  <span class="headline">Test Driven Development with Jest and React</span>
  <span class="subhead">Component Testing</span>
</h1>

**Learning Objective:** By the end of this lesson, students will be able to execute React component tests using Jest, including testing component rendering, debugging, and mocking functions for unit testing.

## Component Testing

In this section, we’ll explore testing techniques for React components using an example from our starter codebase. Our focus is on ensuring each component does what it’s supposed to do when users interact with it. That means we’ll look at how a component renders, how it handles props, and whether it responds correctly to user events.

We’ll be testing a `Navbar` component. You can find this component in the starter code at `src/components/Navbar.js`. The idea is to confirm that the navigation buttons call the correct functions and that the component renders as expected every time.

### The `Navbar` Component

The `Navbar` component is a simple piece of UI that includes four buttons: **Home**, **About**, **Contact**, and **Star Wars**. Each button triggers a function passed down from the parent via the `onNavChange` prop.

Let's take a look at the code:

```js
import React from 'react';

const Navbar = ({ onNavChange }) => (
  <nav>
    <ul>
      <li>
        <button onClick={() => onNavChange('Home')}>Home</button>
      </li>
      <li>
        <button onClick={() => onNavChange('About')}>About</button>
      </li>
      <li>
        <button onClick={() => onNavChange('Contact')}>Contact</button>
      </li>
      <li>
        <button onClick={() => onNavChange('StarWars')}>Star Wars</button>
      </li>
    </ul>
  </nav>
);

export default Navbar;
```

In our upcoming tests, we’ll focus on verifying that:

1. The `Navbar` renders correctly.
2. Each button calls `onNavChange` with the right argument when clicked.

### Writing a test for the `Navbar`

The tests for this component are in `__tests__/Navbar.test.js`. Let's look at the code for the first of the Navbar tests:

```jsx
describe('Navbar', () => {
  it('renders the component', () => {
    const component = render(<Navbar />);
    // Outputs the built component to the terminal for inspection.
    component.debug();
  });

  // more tests below
});
```

#### What this test does

The test uses the `render` function to render the `Navbar` component.

- If the component cannot render, Jest will throw an error, causing the test to fail.
- The `.debug()` method prints the rendered HTML structure of the `Navbar` to the terminal.
- This is useful for confirming that the component structure matches your expectations.

### Example debug output

When `.debug()` is called, you might see output like this in your terminal:

```html
<body>
  <div>
    <nav>
      <ul>
        <li>
          <button>Home</button>
        </li>
        <li>
          <button>About</button>
        </li>
        <li>
          <button>Contact</button>
        </li>
        <li>
          <button>Star Wars</button>
        </li>
      </ul>
    </nav>
  </div>
</body>
```

This output shows how the `Navbar` renders as an HTML structure. Use it to verify that the component renders as expected, including the presence of all navigation buttons.

To execute these tests yourself use the command:

```bash
yarn test
```

This command runs all the tests in your project using the test runner configured in your `package.json` file (in this case, Jest).

Alternatively, if you are using npm instead of yarn, the equivalent command would be:

```bash
npm test
```

### Why test component rendering?

This simple rendering test is an important step in React component testing. This ensure that:

- The `Navbar` component renders without crashing.
- The expected structure (ex: `nav` tags, `button` elements) is present in the output.

In the next sections, we’ll build on this foundation by testing how the `Navbar` handles user interactions and behaves with its `onNavChange` function.

## Testing functionality with mocking

A key concept in testing is **mocking**, which allows us to isolate components, functions, or classes by creating fake versions of their dependencies. This lets us:

- Test individual pieces (unit testing) without relying on external systems.
- Verify that dependencies are being used correctly.

Here’s an example of testing the `Navbar` _functionality_ using mocking:

```js
it('calls onNavChange when a button is clicked', () => {
  // SETUP
  // Create a mock function to simulate the onNavChange handler.
  const mockOnNavChange = jest.fn();
  const component = render(<Navbar onNavChange={mockOnNavChange} />);

  // EXECUTION
  // Find the "Home" button and simulate a click event.
  const homeButton = component.getByText('Home');
  fireEvent.click(homeButton);

  // ASSERTION
  // Verify the mock function was called correctly.
  expect(mockOnNavChange).toHaveBeenCalledTimes(1);
  expect(mockOnNavChange).toHaveBeenCalledWith('Home');
});
```

### What is happening...

1. **During setup:**

   - We define a `mockHandleChange` function using `jest.fn()`.
   - This mock function is passed as the `onNavChange` prop to the `Navbar` component.

2. **On execution:**

   - The `Home` button is retrieved using `getByText('Home')`.
   - The `fireEvent.click()` function simulates a user clicking the button.

3. **During assertion:**
   - The test checks that `mockHandleChange` was called exactly **once** (`toHaveBeenCalledTimes(1)`).
   - It also verifies that it was called with the argument `'Home'` (`toHaveBeenCalledWith('Home')`).

## Why should we use Mocks?

Mocks allow us to focus on the component's behavior without worrying about how its dependencies work.

- In this example, we didn’t need a real implementation of `onNavChange`. Instead, we tested how the `Navbar` interacts with it.
- This ensures that our test results are reliable and isolated from unrelated issues in other parts of the codebase.
