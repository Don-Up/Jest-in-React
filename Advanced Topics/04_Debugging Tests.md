# Debugging Tests

## Using Jest watch mode

Using Jest's watch mode helps in debugging by re-running tests automatically when code changes.

**Demo Code:**

1. **Starting Jest in Watch Mode:**
   ```bash
   # Run Jest in watch mode from the command line
   npm test -- --watch
   ```

2. **Watch Mode Options:**
   ```bash
   # Available watch mode options
   npm test -- --watchAll  # Watch all files
   npm test -- --watch  # Watch only changed files
   ```

3. **Sample Jest Watch Mode Output:**
   ```text
   Watch Usage
   Press a to run all tests.
   Press p to filter by a test name or pattern.
   Press q to quit watch mode.
   ```

**Comments in Code:**
- **Command Line**: Run `npm test -- --watch` to enable watch mode.
- **Options**: `--watchAll` and `--watch` to control which files Jest monitors.

Jest watch mode aids in real-time debugging by automatically rerunning tests on code changes.



## Inspecting failed tests

Inspecting failed tests in Jest helps identify issues by providing detailed failure information.

**Demo Code:**

1. **Running Jest with Verbose Output:**
   ```bash
   # Run Jest with verbose mode for detailed test results
   npm test -- --verbose
   ```

2. **Sample Output for Failed Test:**
   ```text
   FAIL  ./example.test.js
   ● Example test case
   
     expect(received).toBe(expected) // Object.is equality
   
     Expected: 2
     Received: 1
   
     5 | test('Example test case', () => {
     6 |   expect(1 + 1).toBe(2); // This will fail
       |                             ^
     7 | });
   ```

3. **Debugging with `console.log`:**
   ```js
   // example.test.js
   test('Example test case', () => {
     console.log('Debug:', 1 + 1); // Output value for inspection
     expect(1 + 1).toBe(2); // This will fail
   });
   ```

**Comments in Code:**
- **Verbose Mode**: `--verbose` provides detailed test results.
- **Sample Output**: Displays failure reason and test code for inspection.
- **Debugging**: Use `console.log` for additional debug information.

This approach aids in diagnosing and fixing test failures by providing clear error messages and debugging information.



## Debugging with console logs

Debugging with console logs in Jest helps trace and understand issues by printing values and states during test execution.

**Demo Code:**

1. **Adding Console Logs:**
   ```js
   // example.test.js
   test('Debugging with console logs', () => {
     const result = 1 + 1;
     console.log('Debug result:', result); // Output the result for inspection
     expect(result).toBe(2); // Check if result equals 2
   });
   ```

2. **Running Tests and Viewing Logs:**
   ```bash
   # Run Jest and observe console logs in the output
   npm test
   ```

3. **Sample Test Output:**
   ```text
   Debug result: 2
   PASS  ./example.test.js
   ```

**Comments in Code:**
- **Console Logs**: `console.log` outputs variable values during test execution.
- **Viewing Logs**: Jest displays logs in the terminal for debugging.

Using `console.log` in Jest helps trace values and identify issues during tests.



## Using VS Code Jest plugin

Using the VS Code Jest plugin enhances debugging by providing an integrated interface for running and inspecting tests.

**Demo Code:**

1. **Installing Jest Extension:**
   - Search for "Jest" in the VS Code Extensions view and install it.

2. **Running Tests from VS Code:**
   - Click on the "Jest" icon in the Activity Bar.
   - Click the play button to run all tests or specific tests.

3. **Sample Debugging Setup:**
   ```json
   // .vscode/launch.json
   {
     "version": "0.2.0",
     "configurations": [
       {
         "type": "node",
         "request": "launch",
         "name": "Debug Jest Tests",
         "program": "${workspaceFolder}/node_modules/.bin/jest",
         "args": ["--runInBand"],
         "autoAttachChildProcesses": true,
         "console": "integratedTerminal"
       }
     ]
   }
   ```

**Comments in Code:**
- **Extension**: Install Jest extension for VS Code to manage tests easily.
- **Debugging Configuration**: Set up `launch.json` to debug tests directly from VS Code.

The VS Code Jest plugin provides an integrated environment for running and debugging Jest tests efficiently.