# Integration Testing

## Testing component interactions
### 1. Parent-child component communication

Integration testing in Jest involves checking interactions between parent and child components. You ensure the parent correctly passes data and handles events triggered by the child.

**Demo Code:**
```jsx
// Parent.js
const Parent = () => {
  const [message, setMessage] = React.useState('');
  const handleMessage = (msg) => setMessage(msg);

  return (
    <div>
      <Child onSendMessage={handleMessage} />
      <p>{message}</p>
    </div>
  );
};

// Child.js
const Child = ({ onSendMessage }) => (
  <button onClick={() => onSendMessage('Hello from Child')}>Send Message</button>
);

// Parent.test.js
test('Parent handles message from Child correctly', () => {
  // Render the Parent component
  const { getByText } = render(<Parent />);
  
  // Simulate click event in Child component
  fireEvent.click(getByText('Send Message'));
  
  // Assert that the Parent component displays the message from Child
  expect(getByText('Hello from Child')).toBeInTheDocument(); // Verifies parent-child interaction
});
```

### 2. Handling callback functions

Integration testing with Jest for component interactions involves verifying that callback functions are correctly handled between components. This ensures the parent component responds appropriately to events triggered by the child.

**Demo Code:**
```jsx
// Parent.js
const Parent = () => {
  const [message, setMessage] = useState('');

  const handleCallback = (data) => setMessage(data);

  return (
    <div>
      <Child onCallback={handleCallback} />
      <p>{message}</p>
    </div>
  );
};

// Child.js
const Child = ({ onCallback }) => (
  <button onClick={() => onCallback('Callback received')}>Trigger Callback</button>
);

// Parent.test.js
test('Parent handles callback from Child correctly', () => {
  // Render the Parent component
  const { getByText } = render(<Parent />);
  
  // Simulate button click in Child component
  fireEvent.click(getByText('Trigger Callback'));
  
  // Assert that Parent displays the callback message
  expect(getByText('Callback received')).toBeInTheDocument(); // Verifies callback handling
});
```

## Testing hooks
### 1. Custom hooks

Integration testing custom hooks with Jest involves verifying their functionality in isolation or within components. React Testing Library’s `renderHook` is used to test custom hooks by rendering them directly and asserting their behavior.

**Demo Code:**
```jsx
// useCounter.js
const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);
  const increment = () => setCount(count + 1);
  return { count, increment };
};

// useCounter.test.js
test('should initialize and update counter value', () => {
  // Render the custom hook
  const { result } = renderHook(() => useCounter(0));

  // Verify initial state
  expect(result.current.count).toBe(0);

  // Trigger state update
  act(() => {
    result.current.increment();
  });

  // Verify state update
  expect(result.current.count).toBe(1); // Verifies custom hook functionality
});
```

### 2. Built-in hooks (useState, useEffect)

Integration testing built-in hooks like `useState` and `useEffect` with Jest involves verifying their behavior within components. React Testing Library’s `render` function helps test how these hooks affect component state and side effects.

**Demo Code:**
```jsx
// Counter.js
const Counter = () => {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

// Counter.test.js
test('updates count and document title on button click', () => {
  // Render the Counter component
  const { getByText } = render(<Counter />);
  
  // Simulate button click to increment count
  fireEvent.click(getByText('Increment'));
  
  // Verify count value
  expect(getByText('1')).toBeInTheDocument(); // Checks updated count
  
  // Verify document title update (using a mock for document.title)
  expect(document.title).toBe('Count: 1'); // Checks useEffect side effect
});
```



## Testing context providers

### 1. Testing context values

Testing context providers with Jest involves verifying that context values are correctly passed and consumed by components. You use React Testing Library to render components within context providers and assert context values.

**Demo Code:**
```jsx
// ThemeContext.js
const ThemeContext = createContext('light');

export const useTheme = () => useContext(ThemeContext);
export const ThemeProvider = ({ children, theme }) => (
  <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>
);

// ThemedComponent.js
const ThemedComponent = () => {
  const theme = useTheme();
  return <div>{theme}</div>;
};

// ThemedComponent.test.js
test('ThemedComponent displays context value', () => {
  // Render ThemedComponent within ThemeProvider with a specific theme
  const { getByText } = render(
    <ThemeProvider theme="dark">
      <ThemedComponent />
    </ThemeProvider>
  );
  
  // Assert that ThemedComponent correctly displays the context value
  expect(getByText('dark')).toBeInTheDocument(); // Checks context value
});
```

### 2. Verifying context updates

Testing context provider updates with Jest involves verifying that changes in context values are reflected in components. React Testing Library helps test if components react to context updates correctly.

**Demo Code:**
```jsx
// ThemeContext.js
const ThemeContext = createContext();

export const useTheme = () => useContext(ThemeContext);

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// ThemedComponent.js
const ThemedComponent = () => {
  const { theme, setTheme } = useTheme();
  return (
    <div>
      <div>{theme}</div>
      <button onClick={() => setTheme('dark')}>Change Theme</button>
    </div>
  );
};

export default ThemedComponent;

// ThemedComponent.test.js
test('verifies context updates in ThemedComponent', () => {
  // Render ThemedComponent within ThemeProvider
  const { getByText } = render(
    <ThemeProvider>
      <ThemedComponent />
    </ThemeProvider>
  );

  // Verify initial context value
  expect(getByText('light')).toBeInTheDocument(); // Checks initial theme
  
  // Simulate button click to change theme
  fireEvent.click(getByText('Change Theme'));

  // Verify updated context value
  expect(getByText('dark')).toBeInTheDocument(); // Checks updated theme
});
```