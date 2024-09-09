# Testing Asynchronous Code

## Testing promises

Testing asynchronous code in Jest involves handling promises to ensure that the code resolves or rejects as expected. You can use `async/await` or `.resolves`/`.rejects` matchers for this purpose.

**Demo Code:**
```jsx
// api.js
export const fetchData = () => new Promise((resolve) =>
  setTimeout(() => resolve({ message: 'Hello' }), 1000)
);

// fetchData.test.js
test('fetchData resolves with correct data', async () => {
  // Test promise with async/await
  const data = await fetchData();
  expect(data).toEqual({ message: 'Hello' });
});

test('fetchData resolves with correct data using .resolves', () => {
  // Test promise with .resolves
  return expect(fetchData()).resolves.toEqual({ message: 'Hello' });
});
```

In this code, both `async/await` and `.resolves` are used to test the resolution of the `fetchData` promise, ensuring it returns the expected data.



## Async/await syntax

Testing asynchronous code with `async/await` in Jest involves using `async` functions to wait for promises to resolve and handle the results.

**Demo Code:**
```jsx
// api.js
export const fetchData = () => new Promise((resolve) =>
  setTimeout(() => resolve({ message: 'Hello' }), 1000)
);

// fetchData.test.js
test('fetchData resolves with correct data', async () => {
  // Use async/await to handle the promise
  const data = await fetchData();
  
  // Verify the resolved data
  expect(data).toEqual({ message: 'Hello' });
});
```

In this code, `async/await` is used to handle the promise returned by `fetchData`, allowing you to write more readable and maintainable asynchronous tests.



## Simulating delays and timeouts

Simulating delays and timeouts in Jest tests involves using functions like `jest.useFakeTimers` and `jest.advanceTimersByTime` to control and test asynchronous code behavior.

**Demo Code:**
```jsx
// api.js
export const fetchData = () => new Promise((resolve) =>
  setTimeout(() => resolve({ message: 'Hello' }), 1000)
);

// fetchData.test.js
test('fetchData handles delay', async () => {
  // Use fake timers
  jest.useFakeTimers();
  const promise = fetchData();

  // Fast-forward time
  jest.advanceTimersByTime(1000);
  
  // Resolve the promise
  const data = await promise;
  
  // Verify the result
  expect(data).toEqual({ message: 'Hello' });

  // Clean up
  jest.useRealTimers();
});
```

In this code, `jest.useFakeTimers` and `jest.advanceTimersByTime` are used to simulate and fast-forward the delay of `fetchData`, allowing you to test asynchronous behavior without waiting for real time to pass.



## Mocking async operations

Mocking asynchronous operations in Jest involves replacing functions that return promises with mock implementations to control their behavior during tests.

**Demo Code:**
```jsx
// api.js
export const fetchData = () => fetch('/api/data').then(res => res.json());

// Component.js
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData);
  }, []);

  return <div>{data ? data.message : 'Loading...'}</div>;
};

// Component.test.js
// Mock fetchData function
jest.mock('./api', () => ({
  fetchData: jest.fn()
}));

test('displays mocked API data', async () => {
  // Mock the fetchData implementation
  api.fetchData.mockResolvedValue({ message: 'Mocked data' });

  const { getByText } = render(<Component />);

  // Wait for the component to update
  await waitFor(() => {
    expect(getByText('Mocked data')).toBeInTheDocument();
  });
});
```

In this code, `jest.mock` is used to replace `fetchData` with a mock function that returns a resolved promise, allowing you to control the asynchronous behavior for testing.
