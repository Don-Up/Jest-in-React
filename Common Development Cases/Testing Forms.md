# Testing Forms

## Validating form inputs

Testing form inputs in Jest involves simulating user input and validating form validation logic by rendering the form and interacting with its elements.

**Demo Code:**
```jsx
// Form.js
const Form = () => {
  const [value, setValue] = useState('');
  const [error, setError] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (value === '') {
      setError('Field cannot be empty');
    } else {
      setError('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        aria-label="text-input"
      />
      {error && <span>{error}</span>}
      <button type="submit">Submit</button>
    </form>
  );
};

// Form.test.js
test('validates form input', () => {
  render(<Form />);

  // Simulate user input
  fireEvent.change(screen.getByLabelText('text-input'), { target: { value: '' } });
  fireEvent.click(screen.getByText('Submit'));

  // Check for validation error
  expect(screen.getByText('Field cannot be empty')).toBeInTheDocument();
});
```

This code tests form input validation by simulating user input and form submission, then checking if the validation error is displayed as expected.



## Handling form submissions

Testing form submissions in Jest involves simulating form interactions and verifying that the form handles submissions correctly.

**Demo Code:**
```jsx
// Form.js
const Form = ({ onSubmit }) => {
  const [value, setValue] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(value);  // Call onSubmit with the form value
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
test('handles form submission', () => {
  const handleSubmit = jest.fn();  // Mock function to track submission

  render(<Form onSubmit={handleSubmit} />);

  // Simulate user input
  fireEvent.change(screen.getByLabelText('text-input'), { target: { value: 'Test data' } });
  fireEvent.click(screen.getByText('Submit'));

  // Verify the onSubmit function was called with the correct value
  expect(handleSubmit).toHaveBeenCalledWith('Test data');
});
```

In this code, `jest.fn` is used to create a mock function for `onSubmit`. The test simulates form submission and verifies that the mock function is called with the correct input value.



## Testing form validation logic

Testing form validation logic in Jest involves simulating form input and submission, then verifying that validation rules are correctly enforced and error messages are displayed.

**Demo Code:**
```jsx
// Form.js
const Form = () => {
  const [value, setValue] = useState('');
  const [error, setError] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (value.trim() === '') {
      setError('This field is required');
    } else {
      setError('');
      // Process form data
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        aria-label="text-input"
      />
      {error && <span>{error}</span>}
      <button type="submit">Submit</button>
    </form>
  );
};

// Form.test.js
test('displays error message for empty input', () => {
  render(<Form />);

  // Simulate empty input submission
  fireEvent.click(screen.getByText('Submit'));

  // Verify the validation error message
  expect(screen.getByText('This field is required')).toBeInTheDocument();
});
```

In this code, the form component handles validation by displaying an error message if the input is empty. The test simulates form submission and checks if the validation error message appears correctly.



## Mocking form submissions and responses

Mocking form submissions and responses in Jest involves simulating the form submission and controlling the server response to test how the form handles different scenarios.

**Demo Code:**
```jsx
// api.js
export const submitForm = (data) => fetch('/api/submit', {
  method: 'POST',
  body: JSON.stringify(data),
}).then(response => response.json());

// Form.js
const Form = () => {
  const [value, setValue] = useState('');
  const [response, setResponse] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const result = await submitForm({ value });
    setResponse(result.message);
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
      <div>{response}</div>
    </form>
  );
};

// Form.test.js
// Mock submitForm function
jest.mock('./api', () => ({
  submitForm: jest.fn(),
}));

test('handles form submission and response', async () => {
  // Mock the submitForm response
  api.submitForm.mockResolvedValue({ message: 'Success' });

  render(<Form />);

  // Simulate user input and form submission
  fireEvent.change(screen.getByLabelText('text-input'), { target: { value: 'Test data' } });
  fireEvent.click(screen.getByText('Submit'));

  // Wait for the response to appear
  expect(await screen.findByText('Success')).toBeInTheDocument();
});
```

In this code, `jest.mock` is used to mock `submitForm`, controlling the response to test how the form displays the result. The test simulates user input, form submission, and verifies the displayed response.