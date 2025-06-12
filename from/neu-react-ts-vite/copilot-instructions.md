# React Development Instructions for AI Assistants

This document provides comprehensive guidelines for AI assistants working with React projects. Follow these instructions to ensure code quality, consistency, and adherence to modern React best practices.

## Core React Principles

1. **Component Composition**: Build complex UIs from small, reusable components
2. **Unidirectional Data Flow**: Data flows down through props, events flow up
3. **Declarative Programming**: Describe what the UI should look like, not how to manipulate it
4. **Immutability**: Treat state as immutable to enable proper re-rendering
5. **Single Responsibility**: Each component should have one clear purpose

## Project Structure

### Modern React Application Structure
```
src/
├── components/           # Reusable UI components
│   ├── common/          # Generic components (Button, Input, Modal)
│   ├── forms/           # Form-specific components
│   └── layout/          # Layout components (Header, Footer, Sidebar)
├── pages/               # Page components for routing
├── hooks/               # Custom React hooks
├── contexts/            # React context providers
├── services/            # API calls and external services
├── utils/               # Helper functions and utilities
├── types/               # TypeScript type definitions
├── styles/              # Global styles and theme
├── assets/              # Static assets (images, fonts)
├── __tests__/           # Test files
└── App.tsx
```

### File Naming Conventions
- Components: `PascalCase.tsx` (e.g., `UserProfile.tsx`)
- Hooks: `camelCase.ts` starting with "use" (e.g., `useAuth.ts`)
- Utilities: `camelCase.ts` (e.g., `formatDate.ts`)
- Types: `camelCase.types.ts` (e.g., `user.types.ts`)
- Tests: `ComponentName.test.tsx` or `__tests__/ComponentName.test.tsx`

## Component Development

### Functional Components with TypeScript
```tsx
import React from 'react';

interface UserCardProps {
  user: {
    id: string;
    name: string;
    email: string;
    avatar?: string;
  };
  onEdit?: (userId: string) => void;
  className?: string;
}

export const UserCard: React.FC<UserCardProps> = ({ 
  user, 
  onEdit, 
  className = '' 
}) => {
  const handleEdit = () => {
    onEdit?.(user.id);
  };

  return (
    <div className={`user-card ${className}`}>
      {user.avatar && (
        <img 
          src={user.avatar} 
          alt={`${user.name} avatar`}
          className="user-avatar"
        />
      )}
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
      {onEdit && (
        <button onClick={handleEdit} className="edit-button">
          Edit
        </button>
      )}
    </div>
  );
};
```

### Component Guidelines
1. Use functional components with hooks instead of class components
2. Define clear TypeScript interfaces for props
3. Use default parameter values for optional props
4. Extract complex logic into custom hooks
5. Keep components small and focused (< 200 lines)
6. Use meaningful names that describe the component's purpose

## State Management

### Local State with useState
```tsx
import React, { useState, useCallback } from 'react';

interface FormData {
  name: string;
  email: string;
}

export const UserForm: React.FC = () => {
  const [formData, setFormData] = useState<FormData>({
    name: '',
    email: ''
  });
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [errors, setErrors] = useState<Partial<FormData>>({});

  const updateField = useCallback((field: keyof FormData, value: string) => {
    setFormData(prev => ({ ...prev, [field]: value }));
    // Clear error when user starts typing
    if (errors[field]) {
      setErrors(prev => ({ ...prev, [field]: undefined }));
    }
  }, [errors]);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsSubmitting(true);
    
    try {
      await submitUser(formData);
      setFormData({ name: '', email: '' });
    } catch (error) {
      console.error('Submission failed:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Form JSX */}
    </form>
  );
};
```

### Context for Global State
```tsx
import React, { createContext, useContext, useReducer } from 'react';

interface User {
  id: string;
  name: string;
  email: string;
}

interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
}

type AuthAction = 
  | { type: 'LOGIN_START' }
  | { type: 'LOGIN_SUCCESS'; payload: User }
  | { type: 'LOGIN_FAILURE' }
  | { type: 'LOGOUT' };

const authReducer = (state: AuthState, action: AuthAction): AuthState => {
  switch (action.type) {
    case 'LOGIN_START':
      return { ...state, isLoading: true };
    case 'LOGIN_SUCCESS':
      return { 
        user: action.payload, 
        isAuthenticated: true, 
        isLoading: false 
      };
    case 'LOGIN_FAILURE':
      return { 
        user: null, 
        isAuthenticated: false, 
        isLoading: false 
      };
    case 'LOGOUT':
      return { 
        user: null, 
        isAuthenticated: false, 
        isLoading: false 
      };
    default:
      return state;
  }
};

const AuthContext = createContext<{
  state: AuthState;
  dispatch: React.Dispatch<AuthAction>;
} | null>(null);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ 
  children 
}) => {
  const [state, dispatch] = useReducer(authReducer, {
    user: null,
    isAuthenticated: false,
    isLoading: false
  });

  return (
    <AuthContext.Provider value={{ state, dispatch }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

### State Management Guidelines
1. Use `useState` for simple local component state
2. Use `useReducer` for complex state logic
3. Use Context API for state that needs to be shared across many components
4. Consider external libraries (Redux Toolkit, Zustand) for complex global state
5. Keep state as close to where it's used as possible
6. Use `useCallback` and `useMemo` to optimize performance when needed

## Custom Hooks

### Data Fetching Hook
```tsx
import { useState, useEffect } from 'react';

interface UseApiResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => void;
}

export const useApi = <T>(url: string): UseApiResult<T> => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = async () => {
    try {
      setLoading(true);
      setError(null);
      const response = await fetch(url);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, [url]);

  return { data, loading, error, refetch: fetchData };
};
```

### Form Hook
```tsx
import { useState, useCallback } from 'react';

interface UseFormOptions<T> {
  initialValues: T;
  validate?: (values: T) => Partial<Record<keyof T, string>>;
  onSubmit: (values: T) => Promise<void> | void;
}

export const useForm = <T extends Record<string, any>>({
  initialValues,
  validate,
  onSubmit
}: UseFormOptions<T>) => {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const setValue = useCallback((field: keyof T, value: any) => {
    setValues(prev => ({ ...prev, [field]: value }));
    if (errors[field]) {
      setErrors(prev => ({ ...prev, [field]: undefined }));
    }
  }, [errors]);

  const handleSubmit = async (e?: React.FormEvent) => {
    e?.preventDefault();
    
    if (validate) {
      const validationErrors = validate(values);
      if (Object.keys(validationErrors).length > 0) {
        setErrors(validationErrors);
        return;
      }
    }

    setIsSubmitting(true);
    try {
      await onSubmit(values);
    } catch (error) {
      console.error('Form submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const reset = () => {
    setValues(initialValues);
    setErrors({});
    setIsSubmitting(false);
  };

  return {
    values,
    errors,
    isSubmitting,
    setValue,
    handleSubmit,
    reset
  };
};
```

### Custom Hook Guidelines
1. Start hook names with "use"
2. Extract reusable logic from components
3. Return objects with named properties rather than arrays
4. Use TypeScript generics for reusable hooks
5. Handle cleanup with useEffect return functions
6. Memoize expensive computations with useMemo

## Event Handling and Forms

### Form Handling Best Practices
```tsx
import React from 'react';

interface ContactFormData {
  name: string;
  email: string;
  message: string;
}

export const ContactForm: React.FC = () => {
  const { values, errors, isSubmitting, setValue, handleSubmit } = useForm({
    initialValues: { name: '', email: '', message: '' },
    validate: (values) => {
      const errors: Partial<Record<keyof ContactFormData, string>> = {};
      
      if (!values.name.trim()) {
        errors.name = 'Name is required';
      }
      
      if (!values.email.trim()) {
        errors.email = 'Email is required';
      } else if (!/\S+@\S+\.\S+/.test(values.email)) {
        errors.email = 'Email is invalid';
      }
      
      if (!values.message.trim()) {
        errors.message = 'Message is required';
      }
      
      return errors;
    },
    onSubmit: async (data) => {
      await submitContactForm(data);
    }
  });

  return (
    <form onSubmit={handleSubmit} className="contact-form">
      <div className="form-group">
        <label htmlFor="name">Name</label>
        <input
          id="name"
          type="text"
          value={values.name}
          onChange={(e) => setValue('name', e.target.value)}
          className={errors.name ? 'error' : ''}
          disabled={isSubmitting}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          value={values.email}
          onChange={(e) => setValue('email', e.target.value)}
          className={errors.email ? 'error' : ''}
          disabled={isSubmitting}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="message">Message</label>
        <textarea
          id="message"
          value={values.message}
          onChange={(e) => setValue('message', e.target.value)}
          className={errors.message ? 'error' : ''}
          disabled={isSubmitting}
          rows={4}
        />
        {errors.message && <span className="error-message">{errors.message}</span>}
      </div>
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  );
};
```

### Event Handling Guidelines
1. Use TypeScript event types (`React.ChangeEvent<HTMLInputElement>`)
2. Prevent default behavior when necessary
3. Use controlled components for form inputs
4. Validate on blur or form submission, not on every keystroke
5. Disable form during submission to prevent double-submission
6. Provide clear error messages and visual feedback

## Performance Optimization

### Memoization Techniques
```tsx
import React, { memo, useMemo, useCallback } from 'react';

interface ExpensiveListProps {
  items: Array<{ id: string; name: string; value: number }>;
  filter: string;
  onItemClick: (id: string) => void;
}

export const ExpensiveList = memo<ExpensiveListProps>(({ 
  items, 
  filter, 
  onItemClick 
}) => {
  // Memoize expensive filtering computation
  const filteredItems = useMemo(() => {
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);

  // Memoize event handler to prevent child re-renders
  const handleItemClick = useCallback((id: string) => {
    onItemClick(id);
  }, [onItemClick]);

  return (
    <div className="expensive-list">
      {filteredItems.map(item => (
        <ExpensiveListItem
          key={item.id}
          item={item}
          onClick={handleItemClick}
        />
      ))}
    </div>
  );
});

// Child component with memo to prevent unnecessary re-renders
const ExpensiveListItem = memo<{
  item: { id: string; name: string; value: number };
  onClick: (id: string) => void;
}>(({ item, onClick }) => {
  const handleClick = () => onClick(item.id);
  
  return (
    <div className="list-item" onClick={handleClick}>
      <span>{item.name}</span>
      <span>{item.value}</span>
    </div>
  );
});
```

### Performance Guidelines
1. Use `React.memo()` for components that render frequently with same props
2. Use `useMemo()` for expensive computations
3. Use `useCallback()` for event handlers passed to child components
4. Lazy load components with `React.lazy()` and `Suspense`
5. Optimize images with proper sizing and formats
6. Use React DevTools Profiler to identify performance bottlenecks
7. Consider virtualization for large lists

## Testing

### Component Testing with Testing Library
```tsx
import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { UserForm } from './UserForm';

describe('UserForm', () => {
  const mockOnSubmit = jest.fn();

  beforeEach(() => {
    mockOnSubmit.mockClear();
  });

  it('renders form fields correctly', () => {
    render(<UserForm onSubmit={mockOnSubmit} />);
    
    expect(screen.getByLabelText(/name/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument();
  });

  it('shows validation errors for empty fields', async () => {
    const user = userEvent.setup();
    render(<UserForm onSubmit={mockOnSubmit} />);
    
    const submitButton = screen.getByRole('button', { name: /submit/i });
    await user.click(submitButton);
    
    expect(screen.getByText(/name is required/i)).toBeInTheDocument();
    expect(screen.getByText(/email is required/i)).toBeInTheDocument();
    expect(mockOnSubmit).not.toHaveBeenCalled();
  });

  it('submits form with valid data', async () => {
    const user = userEvent.setup();
    render(<UserForm onSubmit={mockOnSubmit} />);
    
    await user.type(screen.getByLabelText(/name/i), 'John Doe');
    await user.type(screen.getByLabelText(/email/i), 'john@example.com');
    await user.click(screen.getByRole('button', { name: /submit/i }));
    
    await waitFor(() => {
      expect(mockOnSubmit).toHaveBeenCalledWith({
        name: 'John Doe',
        email: 'john@example.com'
      });
    });
  });
});
```

### Hook Testing
```tsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('initializes with provided value', () => {
    const { result } = renderHook(() => useCounter(5));
    expect(result.current.count).toBe(5);
  });

  it('increments count', () => {
    const { result } = renderHook(() => useCounter(0));
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });

  it('decrements count', () => {
    const { result } = renderHook(() => useCounter(5));
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(4);
  });
});
```

### Testing Guidelines
1. Test behavior, not implementation details
2. Use `screen` queries from Testing Library for better maintainability
3. Test user interactions with `userEvent` instead of `fireEvent`
4. Mock external dependencies and API calls
5. Write tests for error states and edge cases
6. Use descriptive test names that explain the scenario
7. Keep tests focused and independent

## Styling and CSS

### CSS Modules
```css
/* UserCard.module.css */
.userCard {
  display: flex;
  align-items: center;
  padding: 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.userAvatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  margin-right: 1rem;
}

.userInfo {
  flex: 1;
}

.editButton {
  padding: 0.5rem 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.editButton:hover {
  background: #0056b3;
}
```

```tsx
import styles from './UserCard.module.css';

export const UserCard: React.FC<UserCardProps> = ({ user, onEdit }) => {
  return (
    <div className={styles.userCard}>
      <img src={user.avatar} alt="" className={styles.userAvatar} />
      <div className={styles.userInfo}>
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
      {onEdit && (
        <button className={styles.editButton} onClick={() => onEdit(user.id)}>
          Edit
        </button>
      )}
    </div>
  );
};
```

### Styled Components
```tsx
import styled from 'styled-components';

const Card = styled.div`
  display: flex;
  align-items: center;
  padding: 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
`;

const Avatar = styled.img`
  width: 48px;
  height: 48px;
  border-radius: 50%;
  margin-right: 1rem;
`;

const UserInfo = styled.div`
  flex: 1;
`;

const EditButton = styled.button`
  padding: 0.5rem 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    background: #0056b3;
  }
`;
```

### Styling Guidelines
1. Use CSS Modules or Styled Components for component-scoped styles
2. Follow BEM methodology for CSS class naming
3. Use CSS custom properties for theming
4. Implement responsive design with mobile-first approach
5. Use semantic HTML elements for better accessibility
6. Maintain consistent spacing and typography scales

## Accessibility (A11y)

### Accessible Component Example
```tsx
import React, { useId } from 'react';

interface AccessibleFormProps {
  onSubmit: (data: { name: string; email: string }) => void;
}

export const AccessibleForm: React.FC<AccessibleFormProps> = ({ onSubmit }) => {
  const nameId = useId();
  const emailId = useId();
  const errorId = useId();
  
  const [formData, setFormData] = useState({ name: '', email: '' });
  const [errors, setErrors] = useState<string[]>([]);

  return (
    <form onSubmit={handleSubmit} aria-describedby={errors.length ? errorId : undefined}>
      <h2>Contact Form</h2>
      
      {errors.length > 0 && (
        <div 
          id={errorId}
          role="alert"
          aria-live="polite"
          className="error-summary"
        >
          <h3>Please correct the following errors:</h3>
          <ul>
            {errors.map((error, index) => (
              <li key={index}>{error}</li>
            ))}
          </ul>
        </div>
      )}
      
      <div className="form-group">
        <label htmlFor={nameId}>
          Name <span aria-label="required">*</span>
        </label>
        <input
          id={nameId}
          type="text"
          value={formData.name}
          onChange={(e) => setFormData(prev => ({ ...prev, name: e.target.value }))}
          required
          aria-invalid={errors.some(e => e.includes('name')) ? 'true' : 'false'}
          aria-describedby={errors.some(e => e.includes('name')) ? errorId : undefined}
        />
      </div>
      
      <div className="form-group">
        <label htmlFor={emailId}>
          Email <span aria-label="required">*</span>
        </label>
        <input
          id={emailId}
          type="email"
          value={formData.email}
          onChange={(e) => setFormData(prev => ({ ...prev, email: e.target.value }))}
          required
          aria-invalid={errors.some(e => e.includes('email')) ? 'true' : 'false'}
          aria-describedby={errors.some(e => e.includes('email')) ? errorId : undefined}
        />
      </div>
      
      <button type="submit">
        Submit Form
      </button>
    </form>
  );
};
```

### Accessibility Guidelines
1. Use semantic HTML elements (`main`, `nav`, `section`, `article`)
2. Provide meaningful `alt` text for images
3. Use proper heading hierarchy (`h1`, `h2`, `h3`, etc.)
4. Ensure sufficient color contrast ratios
5. Make all interactive elements keyboard accessible
6. Use ARIA labels and roles when necessary
7. Test with screen readers and keyboard navigation
8. Use `useId()` hook for generating unique IDs

## Error Handling and Debugging

### Error Boundaries
```tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Log to error reporting service
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="error-fallback">
          <h2>Something went wrong</h2>
          <p>We're sorry, but something unexpected happened.</p>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

### Error Handling Guidelines
1. Use Error Boundaries to catch JavaScript errors in component tree
2. Implement proper error states in components
3. Use try-catch blocks for async operations
4. Provide user-friendly error messages
5. Log errors to monitoring services (Sentry, LogRocket)
6. Test error scenarios in development
7. Implement retry mechanisms for failed network requests

## Common Libraries and Tools

### Essential Libraries
- `@types/react` and `@types/react-dom` - TypeScript types
- `react-router-dom` - Client-side routing
- `@tanstack/react-query` - Data fetching and caching
- `react-hook-form` - Form handling
- `@testing-library/react` - Component testing
- `styled-components` or `emotion` - CSS-in-JS
- `framer-motion` - Animations

### Development Tools
- `eslint-plugin-react` and `eslint-plugin-react-hooks` - Linting
- `prettier` - Code formatting
- `storybook` - Component development
- `react-devtools` - Browser extension for debugging
- `vite` or `create-react-app` - Build tools

Following these instructions will help ensure that React applications are maintainable, performant, accessible, and follow modern React development best practices.
