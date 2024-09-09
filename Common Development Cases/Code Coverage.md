# Code Coverage

## Measuring test coverage

Measuring test coverage in Jest involves using built-in tools to report the percentage of code exercised by tests, helping identify untested parts of your codebase.

**Demo Code:**
1. **Add Coverage Configuration in `jest.config.js`:**
   ```js
   // jest.config.js
   module.exports = {
     collectCoverage: true, // Enable coverage collection
     coverageDirectory: 'coverage', // Output directory for coverage reports
     coverageReporters: ['text', 'lcov'], // Report formats
   };
   ```

2. **Run Tests and Collect Coverage:**
   ```bash
   npm test -- --coverage
   ```

3. **View Coverage Report:**
   - Check the terminal output for coverage percentages.
   - Open the `coverage/lcov-report/index.html` file in a browser for a detailed report.

This configuration enables Jest to collect and report coverage data, helping ensure your tests cover all critical code paths.



## Configuring coverage thresholds

Configuring coverage thresholds in Jest ensures that your tests meet specified coverage criteria, helping maintain code quality.

**Demo Code:**
1. **Add Coverage Threshold Configuration in `jest.config.js`:**
   ```js
   // jest.config.js
   module.exports = {
     collectCoverage: true, // Enable coverage collection
     coverageDirectory: 'coverage', // Output directory for coverage reports
     coverageReporters: ['text', 'lcov'], // Report formats
     coverageThreshold: {
       global: {
         branches: 80, // Minimum coverage percentage for branches
         functions: 80, // Minimum coverage percentage for functions
         lines: 80, // Minimum coverage percentage for lines
         statements: 80, // Minimum coverage percentage for statements
       },
     },
   };
   ```

2. **Run Tests and Check Coverage:**
   ```bash
   npm test -- --coverage
   ```

3. **View Threshold Report:**
   - Jest will report if coverage thresholds are not met in the terminal.

This setup ensures your tests meet coverage standards, and Jest will fail if thresholds are not achieved.



## Interpreting coverage reports

Interpreting coverage reports in Jest involves analyzing the detailed metrics on code coverage to identify untested areas and improve test coverage.

**Demo Code:**
1. **Generate Coverage Report:**
   ```bash
   npm test -- --coverage
   ```

2. **View Coverage Report:**
   - Open the `coverage/lcov-report/index.html` file in a browser.

**Comments in Code:**
```js
// Example code file: example.js
export const sum = (a, b) => a + b; // This function needs testing

// Example test file: example.test.js
import { sum } from './example';

test('adds two numbers', () => {
  expect(sum(1, 2)).toBe(3);
});
```

**Report Insights:**
- **Line Coverage:** % of lines executed. High values indicate more code is covered by tests.
- **Branch Coverage:** % of code branches executed. Ensures all possible branches are tested.
- **Function Coverage:** % of functions executed. Checks if all functions are called in tests.
- **Statements Coverage:** % of statements executed. Indicates the percentage of executable lines tested.

Review these metrics to ensure comprehensive test coverage.



## Ignoring files or lines

Ignoring files or lines in Jest code coverage ensures certain parts of the codebase are excluded from coverage metrics, useful for test-exempt code.

**Demo Code:**

1. **Configure Jest to Ignore Files in `jest.config.js`:**
   ```js
   // jest.config.js
   module.exports = {
     collectCoverage: true,
     coverageDirectory: 'coverage',
     coverageReporters: ['text', 'lcov'],
     coveragePathIgnorePatterns: ['/node_modules/', '/dist/'], // Ignore specific directories
   };
   ```

2. **Ignore Lines in Code Files:**
   ```js
   // example.js
   export const sum = (a, b) => a + b; // eslint-disable-line no-unused-vars
   ```

3. **Alternative: Use Coverage Ignore Comments:**
   ```js
   // example.js
   /* istanbul ignore next */
   export const debugFunction = () => {
     // Code that should be ignored by coverage
   };
   ```

**Comments in Code:**
- `coveragePathIgnorePatterns`: Array of regex patterns to ignore files or directories.
- `/* istanbul ignore next */`: Inline comment to exclude the following line from coverage.

This setup helps focus coverage reports on relevant code and exclude irrelevant sections.