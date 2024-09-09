# Code Organization

## Structuring test files

Structuring test files improves maintainability and readability of tests.

**Demo Code:**

1. **Project Structure:**
   ```
   src/
     components/
       Button.js
       Button.test.js
     utils/
       formatDate.js
       formatDate.test.js
   ```

2. **Example Test File:**
   ```js
   // Button.js
   function Button({ label, onClick }) {
     return <button onClick={onClick}>{label}</button>;
   }
   export default Button;
   
   // Button.test.js
   import { render, screen } from '@testing-library/react';
   import Button from './Button';
   
   test('renders Button with correct label', () => {
     render(<Button label="Click Me" />);
     expect(screen.getByText('Click Me')).toBeInTheDocument();
   });
   ```

3. **Best Practices:**
   - **Component Tests**: Place test files next to components or functions.
   - **Utility Tests**: Group related tests in the same directory.
   - **Naming**: Use descriptive names for test files and directories.

**Comments in Code:**
- **Project Structure**: Organize test files alongside their corresponding components or utilities.
- **Naming**: Ensure file names are descriptive and match the files they test.

Organizing test files enhances code readability and makes maintenance easier.



## Reusing test setups and mocks

Reusing test setups and mocks simplifies and standardizes tests, reducing duplication.

**Demo Code:**

1. **Shared Test Setup:**
   ```js
   // setupTests.js
   import '@testing-library/jest-dom/extend-expect';
   // Global setup or mocks
   ```

2. **Mock Implementation:**
   ```js
   // __mocks__/api.js
   export const fetchData = jest.fn(() => Promise.resolve({ data: 'mocked data' }));
   ```

3. **Using Mocks in Tests:**
   ```js
   // dataFetcher.js
   import { fetchData } from './api';
   
   export async function getData() {
     const response = await fetchData();
     return response.data;
   }
   
   // dataFetcher.test.js
   import { getData } from './dataFetcher';
   import { fetchData } from './api';
   
   jest.mock('./api'); // Automatically uses mock implementation
   
   test('fetches data', async () => {
     fetchData.mockResolvedValue({ data: 'mocked data' });
     expect(await getData()).toBe('mocked data');
   });
   ```

4. **Best Practices:**
   - **Shared Setup**: Use `setupTests.js` for global configurations.
   - **Mock Files**: Centralize mocks in a `__mocks__` directory.
   - **Reuse**: Reference mocks across multiple tests to ensure consistency.

**Comments in Code:**
- **Shared Setup**: Configure global setups in a common file.
- **Mock Implementation**: Define reusable mocks in a dedicated directory.

Reusing test setups and mocks reduces duplication and ensures consistent test behavior.



## Grouping related tests

Grouping related tests organizes code, improving readability and maintenance.

**Demo Code:**

1. **Example Test File:**
   ```js
   // mathUtils.js
   function add(a, b) {
     return a + b;
   }
   function subtract(a, b) {
     return a - b;
   }
   module.exports = { add, subtract };
   
   // mathUtils.test.js
   const { add, subtract } = require('./mathUtils');
   
   describe('Addition Tests', () => {
     test('adds 1 + 2 to equal 3', () => {
       expect(add(1, 2)).toBe(3);
     });
     test('adds negative numbers correctly', () => {
       expect(add(-1, -2)).toBe(-3);
     });
   });
   
   describe('Subtraction Tests', () => {
     test('subtracts 5 - 3 to equal 2', () => {
       expect(subtract(5, 3)).toBe(2);
     });
     test('subtracts negative numbers correctly', () => {
       expect(subtract(-1, -2)).toBe(1);
     });
   });
   ```

2. **Best Practices:**
   - **Use `describe`**: Group related tests together for clarity.
   - **Organize by Functionality**: Group tests by functionality or component.
   - **Maintain Structure**: Follow a consistent pattern for grouping.

**Comments in Code:**
- **`describe` Blocks**: Organize tests into logical groups.
- **Functionality-Based Groups**: Group tests based on the functionality being tested.

Grouping related tests enhances organization and readability, making maintenance easier.