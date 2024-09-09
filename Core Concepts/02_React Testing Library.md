# React Testing Library

## Testing components

Jest tests React components by **simulating** user interactions and **verifying** component output. Combined with React Testing Library, it helps ensure components render correctly and respond to events as expected.

**Demo Code:**

```jsx
// Button.js
const Button = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);

// Button.test.js
test('Button calls onClick when clicked', () => {
  const handleClick = jest.fn();
  const { getByText } = render(<Button onClick={handleClick}>Click Me</Button>);
  
  fireEvent.click(getByText('Click Me'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

> - **jest.fn():** Creates a mock function for tracking calls and interactions during tests.
>   
> - **render:** Renders a React component into a virtual DOM for testing.
>
> - **getByText:** Queries the rendered component for an element with specific text content.
>
> - **fireEvent:** Simulates user events like clicks or keystrokes in tests.
>
> - **click:** Simulates a click event on a specified element.
>
> - **expect:** Asserts that a value meets specified conditions in tests.
>
> - **toHaveBeenCalledTimes:** Asserts that a mock function was called a specific number of times.

## Rendering and querying elements

Jest, with React Testing Library, allows rendering React components and querying elements to test their presence and behavior. It ensures components render correctly and interact as expected by simulating user actions.

**Demo Code:**
```jsx
// Input.js
const Input = ({ placeholder }) => <input placeholder={placeholder} />;

// Input.test.js
test('renders input with placeholder', () => {
  const { getByPlaceholderText } = render(<Input placeholder="Enter text" />);
  expect(getByPlaceholderText('Enter text')).toBeInTheDocument();
});
```

> - **getByPlaceholderText:** Queries the rendered component for an input element with a specific placeholder text.
>
> - **toBeInTheDocument:** Asserts that an element is present in the DOM.

## Simulating user events

Jest, in conjunction with React Testing Library, simulates user events like clicks and keystrokes to test component interactions. This helps ensure that components behave correctly in response to user actions.

**Demo Code:**
```jsx
// TextInput.js
const TextInput = () => {
  const [value, setValue] = useState('');
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
      placeholder="Type here"
    />
  );
};

// TextInput.test.js
test('updates input value on user typing', () => {
  // Render the component
  const { getByPlaceholderText } = render(<TextInput />);
  
  // Simulate user typing
  const input = getByPlaceholderText('Type here');
  // Simulate a change event on the `input` element, setting its value to 'Hello'.
  fireEvent.change(input, { target: { value: 'Hello' } });
  
  // Verify the input value has been updated
  expect(input.value).toBe('Hello');
});
```

## Custom queries and matchers

Jest supports custom queries and matchers for tailored testing. Custom queries extend standard querying methods, while custom matchers enhance assertions for specific conditions. They help write more readable and precise tests.

**Demo Code:**
```jsx
// CustomMatchers.js
expect.extend({
  toBeInTheDocument(received) {
    const pass = document.body.contains(received);
    return {
      message: () =>
        `expected ${received} to ${pass ? 'not ' : ''}be in the document`,
      pass,
    };
  },
});

// CustomQuery.js
const CustomComponent = () => <div id="custom">Hello</div>;

// CustomQuery.test.js
test('checks custom matcher and query', () => {
  // Render the component
  const { getByText } = render(<CustomComponent />);
  
  // Use custom matcher
  const element = document.getElementById('custom');
  expect(element).toBeInTheDocument(); // Custom matcher assertion

  // Standard query example
  expect(getByText('Hello')).toBeInTheDocument();
});
```