# Mocking and Stubbing

## Mocking components
### Using jest.mock for component mocking

Mocking components with `jest.mock` allows you to replace a component with a mock version in tests, isolating the component under test and controlling its behavior.

**Demo Code:**
```jsx
// Child.js
const Child = () => <div>Child Component</div>;

// Parent.js
const Parent = () => (
  <div>
    <h1>Parent Component</h1>
    <Child />
  </div>
);

// Parent.test.js
// Mock the Child component
jest.mock('./Child', () => () => <div>Mocked Child</div>);

test('renders Parent with mocked Child', () => {
  const { getByText } = render(<Parent />);
  
  // Verify Parent component renders mocked Child component
  expect(getByText('Mocked Child')).toBeInTheDocument(); // Checks mocked Child
});
```

Here, `jest.mock` replaces `Child` with a simple mock component, isolating and controlling its output for the `Parent` component test.

### Mocking library modules

Mocking library modules with Jest involves replacing library functionality with mock implementations to control behavior and isolate tests from external dependencies.

**Demo Code:**
```jsx
// api.js
export const fetchData = () => fetch('/data').then(res => res.json());

// Component.js
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData);
  }, []);

  return <div>{data ? data.message : 'Loading...'}</div>;
};

// Component.test.js
// Mock the fetchData function from api module
jest.mock('./api', () => ({
  fetchData: jest.fn(() => Promise.resolve({ message: 'Mocked data' }))
}));

test('displays mocked data', async () => {
  const { getByText } = render(<Component />);
  
  // Wait for the component to update
  await waitFor(() => {
    // Verify mocked data is displayed
    expect(getByText('Mocked data')).toBeInTheDocument();
  });
});
```

In this code, `jest.mock` is used to replace `fetchData` with a mock function returning predefined data, isolating the component test from actual API calls.



## Mocking APIs and network requests
### Using jest-fetch-mock or nock

Mocking APIs and network requests with Jest can be done using libraries like `jest-fetch-mock` or `nock` to simulate API responses and control test conditions.

**Demo Code with `jest-fetch-mock`:**
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
import fetchMock from 'jest-fetch-mock';

fetchMock.enableMocks();

beforeEach(() => {
  fetch.resetMocks();
});

test('displays mocked API data', async () => {
  // Mock API response
  fetch.mockResponseOnce(JSON.stringify({ message: 'Mocked data' }));

  const { getByText } = render(<Component />);

  // Wait for the component to update
  await waitFor(() => {
    // Verify mocked data is displayed
    expect(getByText('Mocked data')).toBeInTheDocument();
  });
});
```

**Demo Code with `nock`:**
```jsx
// api.js
export const fetchData = () => axios.get('/api/data').then(res => res.data);

// Component.js
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData);
  }, []);

  return <div>{data ? data.message : 'Loading...'}</div>;
};

// Component.test.js
import nock from 'nock';

test('displays mocked API data', async () => {
  // Mock API response
  nock('http://localhost')
    .get('/api/data')
    .reply(200, { message: 'Mocked data' });

  const { getByText } = render(<Component />);

  // Wait for the component to update
  await waitFor(() => {
    // Verify mocked data is displayed
    expect(getByText('Mocked data')).toBeInTheDocument();
  });
});
```

In both cases, `jest-fetch-mock` and `nock` are used to simulate API responses, allowing you to test components without making real network requests.



## Mocking internal functions
### Using jest.spyOn

Mocking internal functions with `jest.spyOn` allows you to track or control calls to functions within your module, enabling precise testing of function interactions and behavior.

**Demo Code:**
```jsx
// utils.js
export const calculate = (a, b) => a + b;

// Component.js
const Component = ({ a, b }) => {
  const result = calculate(a, b);
  return <div>{result}</div>;
};

// Component.test.js
test('calls calculate with correct arguments', () => {
  // Spy on the calculate function
  const calculateSpy = jest.spyOn(utils, 'calculate').mockReturnValue(10);

  render(<Component a={5} b={5} />);
  
  // Verify calculate was called with correct arguments
  expect(calculateSpy).toHaveBeenCalledWith(5, 5);

  // Verify the mocked return value is used
  expect(calculateSpy).toHaveReturnedWith(10);

  // Clean up
  calculateSpy.mockRestore();
});
```

Here, `jest.spyOn` is used to monitor and control the behavior of `calculate`, allowing you to verify its interactions and ensure the component behaves as expected.



## Mocking third-party libraries

Mocking third-party libraries with Jest involves replacing library functions with mock implementations to control behavior and isolate tests from external dependencies.

**Demo Code:**
```jsx
// apiClient.js (third-party library simulation)
export const fetchData = () => axios.get('/api/data');

// Component.js
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(response => setData(response.data));
  }, []);

  return <div>{data ? data.message : 'Loading...'}</div>;
};

// Component.test.js
// Mock the axios library
jest.mock('axios');

test('displays mocked API data', async () => {
  // Mock the axios get method
  axios.get.mockResolvedValue({ data: { message: 'Mocked data' } });

  const { getByText } = render(<Component />);

  // Wait for the component to update
  await waitFor(() => {
    // Verify mocked data is displayed
    expect(getByText('Mocked data')).toBeInTheDocument();
  });
});
```

In this example, `jest.mock` is used to replace `axios.get` with a mock function, controlling the response for the `Component` test.