# Unit Testing

## Testing React components

### 1. Rendering components

Unit testing React components with Jest involves rendering components in isolation to verify their output and behavior. Using React Testing Library, you can render components and assert their structure and content.

**Demo Code:**
```jsx
// Header.js
const Header = ({ title }) => <h1>{title}</h1>;

// Header.test.js
test('renders Header component with correct title', () => {
  // Render the Header component with a specific title prop
  const { getByText } = render(<Header title="Welcome" />);
  
  // Check if the rendered component displays the correct title
  expect(getByText('Welcome')).toBeInTheDocument(); // Asserts that the title is present
});
```

### 2. Testing props and state

Unit testing React components with Jest involves checking if components handle props and state correctly. React Testing Library allows you to render components and assert changes in state and props.

**Demo Code:**
```jsx
// Counter.js
const Counter = ({ initialCount }) => {
  const [count, setCount] = useState(initialCount);
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

// Counter.test.js
test('renders Counter with initialCount prop and updates state on button click', () => {
  // Render the Counter component with initialCount prop set to 0
  const { getByText } = render(<Counter initialCount={0} />);
  
  // Check if the initial count is displayed correctly
  expect(getByText('0')).toBeInTheDocument(); // Asserts initial state from prop
  
  // Simulate button click to increment the count
  fireEvent.click(getByText('Increment'));
  
  // Check if the count is updated correctly after click
  expect(getByText('1')).toBeInTheDocument(); // Asserts state update
});
```

### 3. Verifying rendered output

Verifying rendered output in unit testing ensures that React components display the expected content. Using Jest and React Testing Library, you can render components and assert their output matches the design specifications.

**Demo Code:**

```jsx
// Greeting.js
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;

// Greeting.test.js
test('renders Greeting component with correct name', () => {
  // Render the Greeting component with the name prop set to 'John'
  const { getByText } = render(<Greeting name="John" />);
  
  // Verify that the rendered output matches the expected greeting
  expect(getByText('Hello, John!')).toBeInTheDocument(); // Checks the correct text is displayed
});
```



## Testing utility functions

### 1. Mocking dependencies

In Jest unit testing, mocking dependencies allows testing utility functions in isolation by replacing external dependencies with mock implementations. This ensures the function’s logic is tested independently of other modules.

**Demo Code:**
```javascript
// utils.js
export const fetchData = async (url) => {
  const response = await axios.get(url);
  return response.data;
};

// utils.test.js
// Mock the axios module
jest.mock('axios');

test('fetchData returns data from API', async () => {
  // Mock axios get method to return a resolved promise with mock data
  axios.get.mockResolvedValue({ data: { name: 'John' } });
  
  // Call fetchData and check if it returns the mocked data
  const data = await fetchData('/api/user');
  expect(data).toEqual({ name: 'John' }); // Verifies that fetchData correctly processes the mocked response
});
```

> - **mock:** A simulated version of a function or module used to isolate code during tests.
>
> - **mockResolvedValue:** Sets a mock function to return a resolved promise with specified value when called.

> ```js
> expect(data).toEqual({ name: 'John' });
> ```
>
> #### Can `toEqual` be replaced with `toBe`?
>
> No, `toEqual` cannot be replaced with `toBe` here. `toEqual` checks for deep equality between objects or arrays, comparing their content. `toBe` checks for strict reference equality, which means both references must point to the exact same object, not just matching content.

### 2. Testing side effects

Testing utility functions with side effects in Jest involves verifying changes made by functions, like updates to global objects, file systems, or databases. Mocking and assertions ensure side effects occur as expected.

**Demo Code:**
```javascript
// logger.js
export const logMessage = (message) => {
  console.log(message); // Side effect: prints to console
};

// logger.test.js
test('logMessage prints the correct message to the console', () => {
  // Mock console.log to track calls
  console.log = jest.fn();
  
  // Call the function with a test message
  logMessage('Hello, Jest!');
  
  // Assert that console.log was called with the expected message
  expect(console.log).toHaveBeenCalledWith('Hello, Jest!'); // Verifies the side effect of logging
});
```



## Assertions and matchers

### 1. Checking element existence

In Jest unit testing, assertions and matchers like `toBeInTheDocument` are used to check the existence of elements in the rendered output. This ensures that components render the required elements correctly.

**Demo Code:**
```javascript
// Link.js
const Link = ({ url, text }) => <a href={url}>{text}</a>;

// Link.test.js
test('checks if the link element exists', () => {
  // Render the Link component with specific props
  const { getByText } = render(<Link url="https://example.com" text="Visit Site" />);
  
  // Assert that the link element with the text 'Visit Site' exists in the document
  expect(getByText('Visit Site')).toBeInTheDocument(); // Checks element existence
});
```

### 2. Verifying text and attributes

In Jest unit testing, assertions and matchers like `toHaveTextContent` and `toHaveAttribute` verify specific text content and attributes of elements. This ensures components render the expected content and attributes correctly.

**Demo Code:**
```javascript
// Button.js
const Button = ({ label, type }) => <button type={type}>{label}</button>;

// Button.test.js
test('verifies button text and type attribute', () => {
  // Render the Button component with specific props
  const { getByText } = render(<Button label="Submit" type="submit" />);
  
  // Assert that the button has the correct text content
  expect(getByText('Submit')).toHaveTextContent('Submit'); // Checks text content
  
  // Assert that the button has the correct type attribute
  expect(getByText('Submit')).toHaveAttribute('type', 'submit'); // Checks attribute value
});
```



## Mocking functions and modules

Mocking functions and modules in Jest allows you to isolate and test units of code without external dependencies. Jest’s `jest.mock` and `jest.fn` help create mock implementations and track function calls.

**Demo Code:**
```javascript
// api.js
export const fetchData = () => {
  return fetch('/data').then((response) => response.json());
};

// api.test.js
// Mock the fetch function globally
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ name: 'John' }),
  })
);

test('mocks fetch function and verifies returned data', async () => {
  // Call fetchData which uses the mocked fetch
  const data = await fetchData();
  
  // Verify the fetch function was called once
  expect(global.fetch).toHaveBeenCalledTimes(1);
  
  // Verify the returned data from the mock
  expect(data).toEqual({ name: 'John' }); // Checks mock implementation
});
```