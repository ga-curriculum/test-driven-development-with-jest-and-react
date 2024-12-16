<h1>
  <span class="headline">Test Driven Development with Jest and React</span>
  <span class="subhead">Testing Advanced Components</span>
</h1>

**Learning Objective:** By the end of this lesson, students will be able to write tests for components that interact with APIs, using mock data to ensure reliable, isolated testing without real API calls.

## Testing components that make API calls

Now we’re stepping into more complex territory.

When testing React components, **_never_** make real API calls during tests.

Here’s why:

- _Tests could become slow_: API calls may take time, slowing down your tests.
- _Tests could become unreliable_: If the API is down or changes, your tests might fail, even though your code is correct.

Instead, we want to make sure that our React component tests can run independently of any network calls. The goal is to _isolate_ the component’s behavior from external dependencies like APIs.

### Mocking API calls

To avoid these problems, we will again use the technique of mocking— by simulating the behavior of the API rather than calling it directly.

For example, we use a library called [Mock Service Worker](https://www.npmjs.com/package/msw) (`msw`) to mock `HTTP` requests. This allows us to return fake test data, which we can then use to run our tests reliably.

Take a look at this example in `StarWars.test.js`. It demonstrates how we use `msw` to mock an API call and ensure that our tests remain independent of external data.

```js
import React from 'react';
import { act, render, waitFor, screen } from '@testing-library/react';
// Importing msw (Mock Service Worker) to mock API calls
import { rest } from 'msw';
import { setupServer } from 'msw/node';

import StarWars from '../components/StarWars';

// Set up the server to intercept network requests and mock API responses
const server = setupServer(
  // Intercept GET requests to the 'https://swapi.dev/api/films/' endpoint
  rest.get('https://swapi.dev/api/films/', (_req, res, ctx) => {
    // Return mock data when the API is called
    return res(
      // The mock data simulates a real API response
      ctx.json({
        results: [
          {
            title: 'Star Wars: The Return of the Jedi',
            episode_id: 6,
            opening_crawl:
              'The Galactic Empire is on the brink of defeat. Rebel forces are gathering their strength for a final assault on the Empire’s ultimate weapon, the second Death Star. Meanwhile, the fate of Han Solo rests in the hands of Princess Leia, Luke Skywalker, and the Ewoks on the forest moon of Endor.',
          },
        ],
      })
    );
  })
);

// Before all tests, we start the mock server to intercept network requests
beforeAll(() => server.listen());

// After all tests, we stop the mock server
afterAll(() => server.close());
```

### Key takeaways

By mocking data you can:

- Test components that depend on API calls without worrying about the actual API.
- Run your tests consistently, without the risk of failures due to network issues or changes in the API that you do not control.

## Further reading

A good place to get started is [Jest's Documentation: Testing React Apps](https://jestjs.io/docs/tutorial-react).
