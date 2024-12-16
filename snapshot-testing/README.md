<h1>
  <span class="headline">Test Driven Development with Jest and React</span>
  <span class="subhead">Snapshot Testing</span>
</h1>

**Learning Objective:**  
By the end of this lesson, students will be able to evaluate component rendering using snapshot testing to make sure changes in UI behavior are intentional and do not introduce bugs.

## Snapshot Testing

Snapshot testing is a powerful technique that helps confirm a React component’s rendered output. When you run a snapshot test for the first time, Jest captures the component’s “snapshot” (its HTML structure and content at that moment) and stores it in a folder, typically in `__snapshots__`. This saved snapshot becomes the “expected” output for future test runs.

### How snapshots are created and stored

1. **First Test Run:**

   - Jest sees that no snapshot file exists for your test. It then creates a snapshot file in a folder named something like `__snapshots__`.
   - The snapshot file is usually automatically named to match your test file, which helps keep things organized.

2. **Subsequent Test Runs:**
   - Each time you run the tests again, Jest compares the current component output to the stored snapshot.
   - If the two match, the test passes. If they differ unexpectedly, the test fails, alerting you to a potential UI change or bug.

> In our React application, snapshots are stored in `src/__tests__/__snapshots__` by default, though the exact location can vary based on your project setup.

### Why snapshot testing is important

- If a component's rendering changes unexpectedly, the snapshot test will fail, signaling that something might be broken.
- When changes to the component's output are intentional (such as new design requirements), the snapshots can be updated to reflect the new output.

### Snapshot testing example

Here’s a simple snapshot test for the `App` component of our React application:

```jsx
it('matches snapshot on initial render', () => {
  const app = render(<App />);
  // Verifies the app's initial render matches the saved snapshot
  expect(app.baseElement).toMatchSnapshot();
});
```

#### What is happening?

1. **`render(<App />)`**: Renders the `App` component for testing.
2. **`app.baseElement`**: Represents the root element of the rendered component.
3. **`toMatchSnapshot()`**: Compares the current output with the saved snapshot in `src/__tests__/__snapshots__`. If they don’t match, the test fails.

## Testing snapshots after a user Interaction

You can also create snapshots after user actions to make sure that the UI updates as expected.

```jsx
it('matches snapshot after clicking About', () => {
  const app = render(<App />);

  // Simulate a user interaction
  const aboutButton = screen.getByText('About');
  fireEvent.click(aboutButton);

  // Verify the updated UI matches the snapshot
  expect(app.baseElement).toMatchSnapshot();
});
```

#### What is happening?

- **Simulated User Actions:** We use tools like `fireEvent` to simulate actions a user might take, such as clicking a button.
- **Capturing UI Changes:** After a user interacts with the component (for example, clicking a button), the component's appearance or behavior may change. Snapshot testing helps us verify that these changes are correct. In this case, we check that clicking the "About" button causes the UI to update as expected and matches the stored snapshot.

## Updating snapshots

When you make deliberate changes to your component's rendering, your snapshots will no longer match, causing tests to fail. In this case, you can update the snapshots:

```bash
yarn jest --updateSnapshot
```

or

```bash
npm run test -- --updateSnapshot
```

> **Important:** Always check that the new output is exactly what you want before you update the snapshot. Otherwise, you risk overlooking genuine bugs.
