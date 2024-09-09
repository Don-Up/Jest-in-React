# Testing Error Boundaries

## Simulating errors in components

Testing error boundaries in Jest involves simulating errors in components to ensure that error boundaries properly catch and handle them.

**Demo Code:**

1. **Error Boundary Component:**

   ```jsx
   // ErrorBoundary.js
   class ErrorBoundary extends Component {
     constructor(props) {
       super(props);
       this.state = { hasError: false };
     }
   
     static getDerivedStateFromError() {
       return { hasError: true };
     }
   
     componentDidCatch(error, info) {
       console.error('Error caught by ErrorBoundary:', error, info);
     }
   
     render() {
       if (this.state.hasError) {
         return <h1>Something went wrong.</h1>;
       }
       return this.props.children;
     }
   }
   ```

2. **Component That Throws Error:**

   ```jsx
   // FaultyComponent.js
   const FaultyComponent = () => {
     throw new Error('Test error');
     return <div>Content</div>;
   };
   ```

3. **Test File:**

   ```js
   // ErrorBoundary.test.js
   test('catches errors from child components', () => {
     // Render ErrorBoundary with FaultyComponent as a child
     const { getByText } = render(
       <ErrorBoundary>
         <FaultyComponent />
       </ErrorBoundary>
     );
   
     // Assert that error message is displayed
     expect(getByText('Something went wrong.')).toBeInTheDocument();
   });
   ```

**Comments in Code:**

- `ErrorBoundary.js`: Defines an error boundary to catch errors.
- `FaultyComponent.js`: Simulates an error by throwing an exception.
- `ErrorBoundary.test.js`: Tests if the error boundary catches errors and displays a fallback UI.

This setup ensures that error boundaries correctly handle and display errors from child components.



## Testing error handling and fallbacks

Testing error boundaries in Jest involves verifying that error boundaries handle errors correctly and display fallback UIs.

**Demo Code:**

1. **Error Boundary Component:**
   ```jsx
   // ErrorBoundary.js
   class ErrorBoundary extends Component {
     constructor(props) {
       super(props);
       this.state = { hasError: false };
     }
   
     static getDerivedStateFromError() {
       return { hasError: true };
     }
   
     componentDidCatch(error, info) {
       console.error('Error caught by ErrorBoundary:', error, info);
     }
   
     render() {
       if (this.state.hasError) {
         return <h1>Something went wrong. Please try again later.</h1>;
       }
       return this.props.children;
     }
   }
   ```
   
2. **Component That Throws Error:**
   ```jsx
   // FaultyComponent.js
   const FaultyComponent = () => {
     throw new Error('Test error');
     return <div>Content</div>;
   };
   ```
   
3. **Test File:**
   ```js
   // ErrorBoundary.test.js
   test('displays fallback UI on error', () => {
     // Render ErrorBoundary with FaultyComponent
     const { getByText } = render(
       <ErrorBoundary>
         <FaultyComponent />
       </ErrorBoundary>
     );
   
     // Check for fallback UI
     expect(getByText('Something went wrong. Please try again later.')).toBeInTheDocument();
   });
   ```

**Comments in Code:**
- `ErrorBoundary.js`: Implements error handling and a fallback UI.
- `FaultyComponent.js`: Simulates an error.
- `ErrorBoundary.test.js`: Verifies that the fallback UI is displayed when an error occurs.

This ensures that the error boundary properly handles errors and shows the expected fallback UI.



## Verifying error boundary behavior

Verifying error boundary behavior in Jest involves ensuring that the error boundary correctly catches errors and executes error handling logic.

**Demo Code:**

1. **Error Boundary Component:**
   ```jsx
   // ErrorBoundary.js
   class ErrorBoundary extends Component {
     constructor(props) {
       super(props);
       this.state = { hasError: false };
     }
   
     static getDerivedStateFromError() {
       return { hasError: true };
     }
   
     componentDidCatch(error, info) {
       // Log error information
       console.log('Caught error:', error, info);
     }
   
     render() {
       if (this.state.hasError) {
         return <h1>Something went wrong. Please try again later.</h1>;
       }
       return this.props.children;
     }
   }
   ```
   
2. **Component That Throws Error:**
   ```jsx
   // FaultyComponent.js
   const FaultyComponent = () => {
     throw new Error('Simulated error');
     return <div>Content</div>;
   };
   ```
   
3. **Test File:**
   ```js
   // ErrorBoundary.test.js
   test('verifies error boundary behavior', () => {
     // Render ErrorBoundary with FaultyComponent
     const { getByText } = render(
       <ErrorBoundary>
         <FaultyComponent />
       </ErrorBoundary>
     );
   
     // Check for fallback UI
     expect(getByText('Something went wrong. Please try again later.')).toBeInTheDocument();
   
     // Verify error logging (optional, use a mock logger if needed)
     // jest.spyOn(console, 'log').mockImplementation(() => {});
   });
   ```

**Comments in Code:**
- `ErrorBoundary.js`: Implements error handling and fallback UI.
- `FaultyComponent.js`: Component designed to throw an error.
- `ErrorBoundary.test.js`: Ensures that the fallback UI is shown and error handling is triggered.

This ensures the error boundary catches errors and displays the appropriate fallback UI.