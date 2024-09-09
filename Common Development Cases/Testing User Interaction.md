# Testing User Interaction

## Simulating user events
### Clicking, typing, selecting

Simulating user interactions in Jest involves using utilities from React Testing Library to mimic user actions like clicking, typing, and selecting, and verifying the resulting behavior.

**Demo Code:**
```jsx
// Form.js
const Form = () => {
  const [text, setText] = useState('');
  const [selected, setSelected] = useState('');

  const handleChange = (e) => setText(e.target.value);
  const handleSelect = (e) => setSelected(e.target.value);

  return (
    <div>
      <input
        type="text"
        value={text}
        onChange={handleChange}
        aria-label="text-input"
      />
      <select
        value={selected}
        onChange={handleSelect}
        aria-label="select-input"
      >
        <option value="">Select</option>
        <option value="Option1">Option 1</option>
        <option value="Option2">Option 2</option>
      </select>
      <button onClick={() => alert('Clicked')}>Click Me</button>
    </div>
  );
};

// Form.test.js
test('simulates user interactions', () => {
  render(<Form />);

  // Simulate typing into input
  fireEvent.change(screen.getByLabelText('text-input'), { target: { value: 'Hello' } });
  expect(screen.getByLabelText('text-input')).toHaveValue('Hello');

  // Simulate selecting an option
  fireEvent.change(screen.getByLabelText('select-input'), { target: { value: 'Option1' } });
  expect(screen.getByLabelText('select-input')).toHaveValue('Option1');

  // Simulate button click
  window.alert = jest.fn(); // Mock window.alert
  fireEvent.click(screen.getByText('Click Me'));
  expect(window.alert).toHaveBeenCalledWith('Clicked');
});
```

This code demonstrates simulating user interactions—typing, selecting, and clicking—using `fireEvent`, and then verifying the changes in the form or the outcomes of these actions.



## Verifying event handlers

Verifying event handlers in Jest involves simulating user interactions and ensuring the associated event handlers are called with the expected arguments.

**Demo Code:**
```jsx
// Button.js
const Button = ({ onClick }) => {
  return <button onClick={onClick}>Click Me</button>;
};

// Button.test.js
test('verifies onClick handler is called', () => {
  // Create a mock function
  const handleClick = jest.fn();

  render(<Button onClick={handleClick} />);

  // Simulate button click
  fireEvent.click(screen.getByText('Click Me'));

  // Verify the onClick handler was called
  expect(handleClick).toHaveBeenCalled();
});
```

This code demonstrates how to test if an event handler (`onClick`) is triggered correctly when a user interacts with the component (e.g., clicking a button).



## Testing form interactions

Testing form interactions in Jest involves simulating user input, form submissions, and verifying that the form handles data as expected.

**Demo Code:**
```jsx
// Form.js
const Form = ({ onSubmit }) => {
  const [value, setValue] = useState('');

  const handleChange = (e) => setValue(e.target.value);
  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        aria-label="text-input"
      />
      <button type="submit">Submit</button>
    </form>
  );
};

// Form.test.js
test('tests form interactions', () => {
  const handleSubmit = jest.fn();

  render(<Form onSubmit={handleSubmit} />);

  // Simulate user input
  fireEvent.change(screen.getByLabelText('text-input'), { target: { value: 'Test data' } });

  // Simulate form submission
  fireEvent.click(screen.getByText('Submit'));

  // Verify onSubmit was called with the input value
  expect(handleSubmit).toHaveBeenCalledWith('Test data');
});
```

This code demonstrates how to test form interactions by simulating user input and submission, and then checking if the submission handler receives the correct data.



## Using fireEvent and userEvent from React Testing Library

Testing user interactions with `fireEvent` and `userEvent` from React Testing Library involves simulating actions like clicks and typing, and verifying their effects on the component.

**Demo Code:**
```jsx
// Form.js
const Form = ({ onSubmit }) => {
  const [value, setValue] = useState('');

  const handleChange = (e) => setValue(e.target.value);
  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        aria-label="text-input"
      />
      <button type="submit">Submit</button>
    </form>
  );
};

// Form.test.js
test('tests form interactions using fireEvent and userEvent', () => {
  const handleSubmit = jest.fn();

  render(<Form onSubmit={handleSubmit} />);

  // Simulate typing using userEvent
  userEvent.type(screen.getByLabelText('text-input'), 'Test data');

  // Simulate form submission using fireEvent
  fireEvent.submit(screen.getByRole('form'));

  // Verify onSubmit was called with the input value
  expect(handleSubmit).toHaveBeenCalledWith('Test data');
});
```

This code shows how to use `fireEvent` for form submissions and `userEvent` for typing interactions, and then verifies that the submission handler is called with the correct input.