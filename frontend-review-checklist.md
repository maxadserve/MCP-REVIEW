# React Frontend Code Review Checklist

## General Code Standards

### File and Function Structure
- [ ] **File length** - Ensure files are under 150 lines
- [ ] **Function length** - Verify functions are max 20-30 lines per function
- [ ] **Meaningful names** - Review variable/function names for clarity and descriptiveness
- [ ] **Complex comments** - Ensure complex logic has explanatory comments

### Design Principles
- [ ] **Law of Demeter** - Check adherence to Law of Demeter - objects should only talk to immediate friends
- [ ] **Single Responsibility** - Verify files and functions follow Single Responsibility Principle
- [ ] **DRY principle** - Check for code duplication - ensure DRY principle is followed
- [ ] **SOLID principles** - Review adherence to SOLID principles (Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion)

### Quality Assurance
- [ ] **Error handling** - Verify proper error handling and edge case coverage
- [ ] **Security checks** - Review for security vulnerabilities (XSS, injection attacks, sensitive data exposure)
- [ ] **Performance basics** - Check for obvious performance issues (unnecessary loops, inefficient algorithms)
- [ ] **Code formatting** - Ensure consistent code formatting and style guidelines

## React-Specific Standards

### Component Design
- [ ] **React component design** - Verify functional components only, single responsibility, and proper composition patterns
- [ ] **State management** - Check proper state management hierarchy - local > lifted > context > external libraries
- [ ] **Performance optimization** - Review memoization usage (React.memo, useMemo, useCallback) and rendering optimization

### React Patterns
- [ ] **Hooks compliance** - Verify hooks rules compliance, proper dependency arrays, and React 19 patterns
- [ ] **Error handling** - Check error boundaries implementation and proper error handling strategies
- [ ] **Testing** - Ensure user-centric testing with RTL, proper mocking, and meaningful coverage

### User Experience
- [ ] **Accessibility** - Verify WCAG 2.1 AA compliance, semantic HTML, and keyboard navigation
- [ ] **Security** - Check XSS prevention, input sanitization, and secure authentication patterns

## Common React Bugs & Performance Issues

### Critical Bug Detection
- [ ] **State mutations** - Watch for direct state mutations - ensure immutable updates with spread operator or immer
- [ ] **Key props** - Check for missing or unstable key props in lists - use stable unique identifiers
- [ ] **Memory leaks** - Identify memory leaks from uncleaned effects, event listeners, and subscriptions
- [ ] **Conditional hooks** - Detect conditional hook calls that violate Rules of Hooks

### Performance Issues
- [ ] **Unnecessary re-renders** - Identify unnecessary re-renders from unstable references and missing memoization
- [ ] **Large lists** - Check for unvirtualized large lists that impact scroll performance
- [ ] **Inline objects** - Detect inline object/function creation in render that breaks memoization

### Anti-patterns
- [ ] **Props drilling** - Identify excessive props drilling that should use Context or state management
- [ ] **DOM manipulation** - Check for direct DOM manipulation that bypasses React's virtual DOM
- [ ] **Async state** - Watch for race conditions from async state updates without proper cleanup

## React Best Practices Reference

### Component Structure Standards
```javascript
// Preferred component structure
const ComponentName = ({ prop1, prop2 }) => {
  // 1. Hooks at the top level
  const [state, setState] = useState();
  const customData = useCustomHook();
  
  // 2. Event handlers
  const handleClick = useCallback(() => {
    // handler logic
  }, [dependencies]);
  
  // 3. Early returns for loading/error states
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorBoundary />;
  
  // 4. Main render
  return (
    <div>
      {/* JSX content */}
    </div>
  );
};
```

### Performance Optimization Examples
```javascript
// Prevent unnecessary re-renders
const ExpensiveComponent = React.memo(({ data, onUpdate }) => {
  const processedData = useMemo(() => {
    return expensiveOperation(data);
  }, [data]);
  
  const handleUpdate = useCallback((id) => {
    onUpdate(id);
  }, [onUpdate]);
  
  return <div>{/* rendered content */}</div>;
});
```

### Error Boundary Implementation
```javascript
// Modern error boundary with react-error-boundary
import { ErrorBoundary } from 'react-error-boundary';

const ErrorFallback = ({ error, resetErrorBoundary }) => (
  <div role="alert">
    <h2>Something went wrong:</h2>
    <pre>{error.message}</pre>
    <button onClick={resetErrorBoundary}>Try again</button>
  </div>
);

// Usage
<ErrorBoundary
  FallbackComponent={ErrorFallback}
  onError={(error, errorInfo) => {
    logErrorToService(error, errorInfo);
  }}
>
  <App />
</ErrorBoundary>
```

### Accessibility Example
```javascript
// Accessible form component
const AccessibleForm = () => {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');
  const emailInputRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (!email.includes('@')) {
      setError('Please enter a valid email address');
      emailInputRef.current.focus(); // Focus management
      return;
    }
    // Submit logic
  };
  
  return (
    <form onSubmit={handleSubmit} noValidate>
      <label htmlFor="email">Email Address</label>
      <input
        id="email"
        ref={emailInputRef}
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        aria-describedby={error ? "email-error" : undefined}
        aria-invalid={!!error}
        required
      />
      {error && (
        <div id="email-error" role="alert" aria-live="polite">
          {error}
        </div>
      )}
      <button type="submit">Submit</button>
    </form>
  );
};
```

### Security Best Practices
```javascript
// Safe content rendering
const SafeComponent = ({ userContent, trustedHtml }) => {
  return (
    <div>
      {/* React automatically escapes content */}
      <p>{userContent}</p>
      
      {/* When you must use dangerouslySetInnerHTML */}
      <div
        dangerouslySetInnerHTML={{
          __html: DOMPurify.sanitize(trustedHtml)
        }}
      />
    </div>
  );
};
```

## Quick Reference: Critical Issues to Watch For

1. **State Mutations** - Always use immutable updates
2. **Missing Keys** - Use stable, unique identifiers
3. **Memory Leaks** - Clean up effects, listeners, and subscriptions
4. **Unnecessary Re-renders** - Memoize expensive operations and stable references
5. **Incorrect Dependencies** - Include all dependencies in effect arrays
6. **Conditional Hooks** - Always call hooks in the same order
7. **Props Drilling** - Use Context or state management for deep prop passing
8. **Large Lists** - Implement virtualization for performance

## Essential Tools for Code Review

- React DevTools (Profiler and Components tabs)
- ESLint with react-hooks plugin
- React.StrictMode in development
- Performance monitoring and testing
- Accessibility testing tools (axe-core)
- Bundle analysis tools
