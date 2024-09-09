# Test Maintenance

## Keeping tests up-to-date with code changes

Keeping tests up-to-date ensures reliability as code evolves. Regularly review and update tests to reflect code changes.

**Demo Code:**

1. **Example Function:**
   ```js
   // userService.js
   function getUser(id) {
     // Mock implementation
     return { id, name: 'John Doe' };
   }
   module.exports = getUser;
   
   // userService.test.js
   const getUser = require('./userService');
   
   test('gets user by ID', () => {
     expect(getUser(1)).toEqual({ id: 1, name: 'John Doe' });
   });
   ```

2. **Updating Tests with Code Changes:**
   ```js
   // userService.js (updated)
   function getUser(id) {
     // Updated implementation
     if (id === 0) throw new Error('Invalid ID');
     return { id, name: 'John Doe' };
   }
   module.exports = getUser;
   
   // userService.test.js (updated)
   const getUser = require('./userService');
   
   test('throws error for invalid ID', () => {
     expect(() => getUser(0)).toThrow('Invalid ID'); // Update test for new behavior
   });
   
   test('gets user by ID', () => {
     expect(getUser(1)).toEqual({ id: 1, name: 'John Doe' });
   });
   ```

3. **Best Practices:**
   - **Review Tests Regularly**: Update tests when code changes.
   - **Refactor**: Adjust tests to reflect refactored code.
   - **Automate**: Use CI tools to ensure tests run with each code change.

**Comments in Code:**
- **Review Tests**: Update tests as the implementation evolves.
- **Handle New Scenarios**: Add or modify tests to cover new or changed behaviors.

Regularly updating tests ensures they remain accurate and relevant as code changes.



## Regularly updating snapshots

Regularly updating snapshots ensures they reflect current component output and avoid false positives.

**Demo Code:**

1. **Component Snapshot:**
   ```js
   // Button.js
   import React from 'react';
   
   function Button({ label }) {
     return <button>{label}</button>;
   }
   
   export default Button;
   
   // Button.test.js
   import { render } from '@testing-library/react';
   import Button from './Button';
   
   test('matches snapshot', () => {
     const { asFragment } = render(<Button label="Click Me" />);
     expect(asFragment()).toMatchSnapshot();
   });
   ```

2. **Updating Snapshots:**
   - Run tests and update snapshots with:
     ```bash
     jest --updateSnapshot
     ```

3. **Best Practices:**
   - **Review Snapshots**: Ensure they match the expected output.
   - **Update Regularly**: Run snapshot updates when changes are intentional.
   - **Manual Inspection**: Check if snapshots correctly represent the component's UI.

**Comments in Code:**
- **Snapshot Update**: Use `--updateSnapshot` to reflect changes in component output.
- **Review Output**: Verify that updated snapshots accurately reflect the UI.

Regular updates keep snapshots aligned with component changes, maintaining test reliability.



## Refactoring and optimizing tests

Refactoring and optimizing tests improve readability and performance. Regularly review tests to simplify and enhance efficiency.

**Demo Code:**

1. **Initial Test:**
   ```js
   // sum.js
   function sum(a, b) {
     return a + b;
   }
   module.exports = sum;
   
   // sum.test.js
   const sum = require('./sum');
   
   test('sums numbers correctly', () => {
     expect(sum(1, 2)).toBe(3);
     expect(sum(2, 3)).toBe(5);
     // Redundant test case
   });
   ```

2. **Refactored Test:**
   ```js
   // sum.test.js
   const sum = require('./sum');
   
   describe('sum function', () => {
     test.each([
       [1, 2, 3],
       [2, 3, 5],
     ])('correctly sums %i and %i to be %i', (a, b, expected) => {
       expect(sum(a, b)).toBe(expected);
     });
   });
   ```

3. **Best Practices:**
   - **Remove Redundancies**: Consolidate similar test cases.
   - **Use Test Helpers**: Leverage `test.each` for parameterized tests.
   - **Optimize Performance**: Avoid unnecessary re-renders or operations.

**Comments in Code:**
- **Refactor**: Combine similar test cases to streamline code.
- **Optimize**: Use `test.each` for parameterized and efficient testing.

Refactoring and optimizing tests enhance maintainability and execution speed.