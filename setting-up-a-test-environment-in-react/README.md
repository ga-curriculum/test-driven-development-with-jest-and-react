<h1>
  <span class="headline">Test Driven Development with Jest and React</span>
  <span class="subhead">Setting Up an Environment in React</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to set up a complete testing environment for a React application using Jest and related tools, enabling them to write and execute tests for React components and features.

## Overview

Testing a React application involves more than just writing unit tests for small functions. React applications are dynamic and interactive, which means you need to test various aspects of the application to ensure it works as intended. In this lesson, we'll cover:

- **Component testing:** Verifying the behavior of individual components
- **Snapshot testing:** Ensuring that components' output hasn't unexpectedly changed
- **Mocking API calls:** Testing components that interact with external data

For this lesson, the React application and example tests have already been prepared for you. However, understanding how to set up a testing environment is important for applying these techniques to your own projects. Below, we’ll walk through the necessary setup steps and key files to help you grasp the process.

### Adding packages

To test a React application, we need several third-party packages that enhance Jest's functionality. These were added as development dependencies (using the `--dev` flag) to ensure they’re only used during development.

Here’s an example of the command used to add the required packages:

```sh
yarn add --dev jest babel-jest @babel/preset-env @babel/preset-react @testing-library/react @testing-library/jest-dom react-test-renderer msw regenerator-runtime
```

> Note: This has already been done for you in the starter code.

### Key packages

| Package                         | Purpose                                                                     |
| ------------------------------- | --------------------------------------------------------------------------- |
| **`jest` and `babel-jest`**     | Core testing framework and Babel integration to handle JSX and ES6+ syntax. |
| **`@testing-library/react`**    | Tool for rendering React components in tests.                               |
| **`@testing-library/jest-dom`** | Provides custom assertions for testing DOM elements.                        |
| **`react-test-renderer`**       | Used for snapshot testing.                                                  |
| **`msw`**                       | Mock Service Worker for testing API calls.                                  |
| **`regenerator-runtime`**       | Enables async/await in tests.                                               |

### Running tests

To run tests, use the `yarn test` command. This runs the Jest test runner, configured via the `scripts` section of the `package.json` file:

```json
"scripts": {
  "test": "jest",
  "test:watch": "jest --watch",
  "build": "webpack --mode=production --env NODE_ENV=production",
  "start": "webpack-dev-server --mode=development --env NODE_ENV=local"
}
```

- **`yarn test`**: Executes all tests using Jest.
- **`yarn test:watch`**: Runs tests in watch mode, automatically re-running them when files change.

---

### Alternative: Running tests with `npm`

If you’re using npm instead of Yarn, you can run the same tests with the `npm run` command. The test scripts in `package.json` are compatible with both package managers.

Here’s how to use npm:

- **`npm test`**: Executes all tests using Jest (equivalent to `yarn test`).
- **`npm run test:watch`**: Runs tests in watch mode, automatically re-running them when files change (equivalent to `yarn test:watch`).

Both commands rely on the same `scripts` configuration in the `package.json` file, so you can use either Yarn or npm depending on your preference or project setup.

## Configuring Jest

To effectively test React applications with Jest, you need to configure it to handle JSX syntax, stylesheets, and DOM-based testing. Luckily, in the provided starter code, this setup is already in place.

Here’s a breakdown of the key files and their purposes:

### Jest configuration file

In the root directory, you'll find the `jest.config.js` file. This file customizes Jest’s behavior for your React project. It includes:

```javascript
module.exports = {
  verbose: true, // Provides detailed test results in the console.
  moduleNameMapper: {
    '^.+\\.(css|less|scss)$': 'babel-jest', // Mocks stylesheet imports to prevent errors.
  },
  setupFilesAfterEnv: ['./src/setupTests.js'], // Specifies a setup file to run before tests.
  testEnvironment: 'jsdom', // Simulates a browser-like environment for testing React components.
};
```

### Setup file

In the `src` directory, there’s a `setupTests.js` file. This file loads dependencies that all test files need, such as:

```javascript
import '@testing-library/jest-dom'; // Adds custom DOM assertions like toBeVisible() or toHaveTextContent().
import 'regenerator-runtime/runtime'; // Enables support for async/await syntax in tests.
```

### Why these configurations are important

- **Handling JSX Syntax:** Jest uses Babel (via `babel-jest`) to interpret JSX, ensuring it understands React’s component syntax.
- **Mocking Stylesheets:** Importing stylesheets in tests can lead to errors. The `moduleNameMapper` mocks them, preventing issues.
- **Simulating a Browser:** React components often rely on browser-like behavior, which is simulated by Jest’s `jsdom` environment.
- **Supporting Async/Await:** Many tests involve asynchronous operations, and `regenerator-runtime` ensures that async/await syntax works as expected.

## Tour the starter code

Before diving into the tests themselves, let’s take a moment to understand the React application we’re working with. This “tour” gives context on how the app is structured and what each part does:

1. **Basic App Structure**:  
   The application includes a welcome message "Learn React Testing" and a **`Navbar`** component with four demo links: **Home**, **About**, **Contact**, and **StarWars**. Each link simulates page navigation by displaying different content.

2. **Home, About, and Contact Links**:  
   These links illustrate how typical page navigation might look—when clicked, they display a simple message showing the current page. There’s no real routing here; instead, the app updates a piece of state that changes what’s rendered on the screen.

3. **StarWars Link**:  
   Clicking the **StarWars** link demonstrates how an application could display data from an external API.
   - **Simulated API Call**: In the starter code, you’ll notice that the component is structured as if it’s making a real HTTP request. However, due to CORS limitations and local testing constraints, the fetched data is actually mocked. This allows you to test the logic around API calls without encountering CORS errors or relying on live endpoints.
   - **Display of Movie Data**: After the simulated request, the page renders movie titles or other Star Wars data—just as if it came from a real server.

### Next Steps

- **Look at the component code**: Get familiar with the components in `src/components`—especially the `Navbar` and any page components.
- **Check out the tests**: Each lesson step will show how we test different parts of the app, from rendering and user interactions to verifying data flow and snapshot comparisons.
