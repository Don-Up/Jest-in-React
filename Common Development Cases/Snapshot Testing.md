# Snapshot Testing

## Creating component snapshots

Snapshot testing in Jest involves capturing and comparing the rendered output of a component to ensure it remains consistent. This is done by generating a snapshot file and comparing it against future renders.

**Demo Code:**
```jsx
// Button.js
const Button = ({ label }) => <button>{label}</button>;

// Button.test.js
// Test to create and compare snapshot
test('matches the snapshot', () => {
  // Render the Button component
  const { asFragment } = render(<Button label="Click me" />);
  
  // Generate and compare snapshot
  expect(asFragment()).toMatchSnapshot(); // Creates or matches the snapshot
});
```

In this code, `toMatchSnapshot` helps in creating or comparing snapshots, ensuring the component output remains unchanged across tests.



## Updating snapshots

Updating snapshots in Jest involves modifying the snapshot file to reflect changes in the component’s rendered output. This is useful when intentional changes are made to the component.

**Demo Code:**
```jsx
// Button.js
const Button = ({ label }) => <button>{label}</button>;

// Button.test.js
// Test to create or update snapshot
test('matches the snapshot', () => {
  const { asFragment } = render(<Button label="Click me" />);
  expect(asFragment()).toMatchSnapshot(); // Creates or matches the snapshot
});

// To update snapshots, run: `jest --updateSnapshot` or `jest -u`
```

Use the `--updateSnapshot` flag with Jest to update snapshots when component changes are expected and intentional.



## Handling dynamic content

Handling dynamic content in Jest snapshot testing involves ensuring snapshots account for variable data by normalizing or excluding dynamic parts. This ensures consistent snapshots despite varying content.

**Demo Code:**
```jsx
// Message.js
const Message = ({ text }) => {
  const date = format(new Date(), 'yyyy-MM-dd');
  return <div>{`${text} - ${date}`}</div>;
};

// Message.test.js
test('matches the snapshot with dynamic content', () => {
  const { asFragment } = render(<Message text="Hello" />);
  
  // Snapshot includes dynamic date, so normalize it
  const fragment = asFragment();
  const snapshot = fragment.innerHTML.replace(/(\d{4}-\d{2}-\d{2})/, 'DATE'); // Replace date with placeholder

  expect(snapshot).toMatchSnapshot(); // Compare with normalized snapshot
});
```

Here, the date is replaced with a placeholder to manage dynamic content in snapshots.



## Reviewing snapshot changes

Reviewing snapshot changes in Jest involves inspecting differences between current and previous snapshots to verify intentional updates. This ensures that any changes to the component’s rendered output are deliberate and expected.

**Demo Code:**
```jsx
// Button.js
const Button = ({ label }) => <button>{label}</button>;

// Button.test.js
test('matches the snapshot', () => {
  const { asFragment } = render(<Button label="Click me" />);
  expect(asFragment()).toMatchSnapshot(); // Create or update snapshot
});

// After running tests, review snapshot changes in __snapshots__/Button.test.js
```

To review changes, check the `__snapshots__/Button.test.js` file for differences. Jest provides a diff output when snapshots do not match. Use `jest -u` to update snapshots after verifying changes.