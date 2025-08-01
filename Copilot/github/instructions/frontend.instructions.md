# Frontend Development Instructions - React TypeScript

> **Specialization:** React 18+ with TypeScript, Jest Unit Testing, Playwright E2E Testing
> **Version:** 1.0
> **Last Updated:** [DATE]
> **Maintainer:** [TEAM_NAME] Frontend Team

---

## React & TypeScript Standards

### Component Architecture

#### Component Types & Patterns
```typescript
// ✅ Functional Components (Preferred)
interface UserProfileProps {
  user: User;
  onEdit: (user: User) => void;
  className?: string;
}

const UserProfile: React.FC<UserProfileProps> = ({
  user,
  onEdit,
  className = ''
}) => {
  return (
    <div className={`user-profile ${className}`}>
      <h2>{user.name}</h2>
      <button onClick={() => onEdit(user)}>Edit</button>
    </div>
  );
};

export default UserProfile;

// ✅ Custom Hooks for Logic
const useUserProfile = (userId: string) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        const userData = await userService.getUser(userId);
        setUser(userData);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]);

  return { user, loading, error, refetch: () => fetchUser() };
};
```

#### Component Organization Principles
- **Single Responsibility:** Each component should have one clear purpose
- **Composition over Inheritance:** Use composition and hooks instead of class inheritance
- **Pure Components:** Prefer pure functional components that don't cause side effects
- **Props Interface:** Always define TypeScript interfaces for props
- **Default Props:** Use default parameters instead of defaultProps

#### File Structure & Naming
```
src/
├── components/           # Reusable UI components
│   ├── common/          # Generic components (Button, Input, Modal)
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   ├── Button.stories.tsx (if using Storybook)
│   │   │   └── index.ts
│   │   └── index.ts     # Barrel exports
│   ├── forms/           # Form-specific components
│   └── layout/          # Layout components (Header, Sidebar, Footer)
├── pages/               # Page components (Next.js) or route components
├── hooks/               # Custom React hooks
├── services/            # API services and external integrations
├── utils/               # Utility functions
├── types/               # TypeScript type definitions
├── constants/           # Application constants
└── __tests__/           # Global test utilities and setup
```

### TypeScript Best Practices

#### Type Definitions
```typescript
// ✅ Interface for object shapes
interface User {
  readonly id: string;
  email: string;
  firstName: string;
  lastName: string;
  role: UserRole;
  createdAt: Date;
  updatedAt: Date;
}

// ✅ Union types for constants
type UserRole = 'admin' | 'user' | 'moderator';
type ButtonVariant = 'primary' | 'secondary' | 'danger' | 'ghost';

// ✅ Generic interfaces for reusable patterns
interface ApiResponse<T> {
  data: T;
  message: string;
  success: boolean;
}

interface PaginatedResponse<T> extends ApiResponse<T[]> {
  pagination: {
    page: number;
    limit: number;
    total: number;
    totalPages: number;
  };
}

// ✅ Utility types for transformations
type CreateUserRequest = Omit<User, 'id' | 'createdAt' | 'updatedAt'>;
type UpdateUserRequest = Partial<Pick<User, 'firstName' | 'lastName' | 'email'>>;
```

#### React-Specific Types
```typescript
// ✅ Event handlers
interface FormProps {
  onSubmit: (event: React.FormEvent<HTMLFormElement>) => void;
  onChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
  onClick: (event: React.MouseEvent<HTMLButtonElement>) => void;
}

// ✅ Ref types
const inputRef = useRef<HTMLInputElement>(null);
const buttonRef = useRef<HTMLButtonElement>(null);

// ✅ Children props
interface LayoutProps {
  children: React.ReactNode;
  header?: React.ComponentType;
}

// ✅ Component props with HTML attributes
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant: ButtonVariant;
  loading?: boolean;
  icon?: React.ComponentType<{ className?: string }>;
}
```

#### Advanced TypeScript Patterns
```typescript
// ✅ Discriminated unions for component variants
type AlertProps = {
  message: string;
  onClose: () => void;
} & (
  | { type: 'success'; icon?: never }
  | { type: 'error'; icon: React.ComponentType }
  | { type: 'warning'; icon?: React.ComponentType }
);

// ✅ Generic component props
interface DataTableProps<T> {
  data: T[];
  columns: Array<{
    key: keyof T;
    label: string;
    render?: (value: T[keyof T], item: T) => React.ReactNode;
  }>;
  onRowClick?: (item: T) => void;
}

// ✅ Higher-order component types
const withAuth = <P extends object>(
  Component: React.ComponentType<P>
): React.ComponentType<P> => {
  return (props: P) => {
    const { user } = useAuth();

    if (!user) {
      return <LoginPrompt />;
    }

    return <Component {...props} />;
  };
};
```

---

## State Management

### Local State with useState
```typescript
// ✅ Proper state typing
const [user, setUser] = useState<User | null>(null);
const [users, setUsers] = useState<User[]>([]);
const [loading, setLoading] = useState<boolean>(false);
const [filters, setFilters] = useState<UserFilters>({
  role: 'all',
  status: 'active',
  search: ''
});

// ✅ State updates with proper typing
const handleUserUpdate = (updatedUser: User) => {
  setUsers(prevUsers =>
    prevUsers.map(user =>
      user.id === updatedUser.id ? updatedUser : user
    )
  );
};

// ✅ Complex state with useReducer
interface AppState {
  users: User[];
  selectedUser: User | null;
  loading: boolean;
  error: string | null;
}

type AppAction =
  | { type: 'FETCH_USERS_START' }
  | { type: 'FETCH_USERS_SUCCESS'; payload: User[] }
  | { type: 'FETCH_USERS_ERROR'; payload: string }
  | { type: 'SELECT_USER'; payload: User }
  | { type: 'CLEAR_SELECTION' };

const appReducer = (state: AppState, action: AppAction): AppState => {
  switch (action.type) {
    case 'FETCH_USERS_START':
      return { ...state, loading: true, error: null };
    case 'FETCH_USERS_SUCCESS':
      return { ...state, loading: false, users: action.payload };
    case 'FETCH_USERS_ERROR':
      return { ...state, loading: false, error: action.payload };
    case 'SELECT_USER':
      return { ...state, selectedUser: action.payload };
    case 'CLEAR_SELECTION':
      return { ...state, selectedUser: null };
    default:
      return state;
  }
};
```

### Global State Management
Choose based on complexity:
- **React Context:** Simple global state, theme, auth
- **Zustand:** Medium complexity, good TypeScript support
- **Redux Toolkit:** Complex applications, time-travel debugging needed

```typescript
// ✅ Context with TypeScript
interface AuthContextType {
  user: User | null;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  loading: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export const useAuth = (): AuthContextType => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

---

## Custom Hooks

### Hook Design Principles
```typescript
// ✅ Custom hook with proper typing and error handling
const useApi = <T>(url: string): {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
} => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
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
  }, [url]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
};

// ✅ Form handling hook
const useForm = <T extends Record<string, any>>(
  initialValues: T,
  validationSchema?: (values: T) => Record<keyof T, string>
) => {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
  const [touched, setTouched] = useState<Partial<Record<keyof T, boolean>>>({});

  const handleChange = (name: keyof T) => (
    event: React.ChangeEvent<HTMLInputElement>
  ) => {
    setValues(prev => ({ ...prev, [name]: event.target.value }));
    if (touched[name]) {
      validateField(name, event.target.value);
    }
  };

  const handleBlur = (name: keyof T) => () => {
    setTouched(prev => ({ ...prev, [name]: true }));
    validateField(name, values[name]);
  };

  const validateField = (name: keyof T, value: any) => {
    if (validationSchema) {
      const fieldErrors = validationSchema({ ...values, [name]: value });
      setErrors(prev => ({ ...prev, [name]: fieldErrors[name] }));
    }
  };

  const reset = () => {
    setValues(initialValues);
    setErrors({});
    setTouched({});
  };

  return {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    reset,
    isValid: Object.keys(errors).length === 0
  };
};
```

### Common Hook Patterns
```typescript
// ✅ Local storage hook
const useLocalStorage = <T>(key: string, initialValue: T) => {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  };

  return [storedValue, setValue] as const;
};

// ✅ Debounced value hook
const useDebounce = <T>(value: T, delay: number): T => {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
};
```

---

## Styling & UI

### CSS-in-JS / Styling Solutions
Choose one approach consistently:

#### Styled Components (if using)
```typescript
import styled from 'styled-components';

interface ButtonProps {
  variant: 'primary' | 'secondary';
  size: 'small' | 'medium' | 'large';
  disabled?: boolean;
}

const StyledButton = styled.button<ButtonProps>`
  padding: ${({ size }) => {
    switch (size) {
      case 'small': return '0.5rem 1rem';
      case 'large': return '1rem 2rem';
      default: return '0.75rem 1.5rem';
    }
  }};

  background-color: ${({ variant, theme }) =>
    variant === 'primary' ? theme.colors.primary : theme.colors.secondary
  };

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;
```

#### CSS Modules (if using)
```typescript
// Button.module.css
.button {
  padding: 0.75rem 1.5rem;
  border-radius: 0.25rem;
  font-weight: 500;
  transition: all 0.2s ease;
}

.primary {
  background-color: var(--color-primary);
  color: white;
}

.secondary {
  background-color: var(--color-secondary);
  color: var(--color-text);
}

// Button.tsx
import styles from './Button.module.css';

interface ButtonProps {
  variant: 'primary' | 'secondary';
  children: React.ReactNode;
}

const Button: React.FC<ButtonProps> = ({ variant, children }) => {
  return (
    <button className={`${styles.button} ${styles[variant]}`}>
      {children}
    </button>
  );
};
```

#### Tailwind CSS (if using)
```typescript
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 disabled:opacity-50 disabled:pointer-events-none',
  {
    variants: {
      variant: {
        primary: 'bg-blue-600 text-white hover:bg-blue-700',
        secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
        ghost: 'hover:bg-gray-100 hover:text-gray-900',
      },
      size: {
        sm: 'h-9 px-3',
        md: 'h-10 px-4 py-2',
        lg: 'h-11 px-8',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
);

interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {}

const Button: React.FC<ButtonProps> = ({
  className,
  variant,
  size,
  ...props
}) => {
  return (
    <button
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
};
```

### Responsive Design
```typescript
// ✅ Mobile-first responsive design
const ResponsiveGrid: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  return (
    <div className={`
      grid
      grid-cols-1
      gap-4
      sm:grid-cols-2
      md:grid-cols-3
      lg:grid-cols-4
      xl:grid-cols-6
    `}>
      {children}
    </div>
  );
};

// ✅ Custom breakpoint hook
const useBreakpoint = () => {
  const [breakpoint, setBreakpoint] = useState<string>('mobile');

  useEffect(() => {
    const updateBreakpoint = () => {
      const width = window.innerWidth;
      if (width >= 1200) setBreakpoint('xl');
      else if (width >= 992) setBreakpoint('lg');
      else if (width >= 768) setBreakpoint('md');
      else if (width >= 576) setBreakpoint('sm');
      else setBreakpoint('mobile');
    };

    updateBreakpoint();
    window.addEventListener('resize', updateBreakpoint);
    return () => window.removeEventListener('resize', updateBreakpoint);
  }, []);

  return breakpoint;
};
```

---

## Performance Optimization

### React Performance Patterns
```typescript
// ✅ Memoization for expensive calculations
const ExpensiveComponent: React.FC<{ data: ComplexData[] }> = ({ data }) => {
  const processedData = useMemo(() => {
    return data
      .filter(item => item.isActive)
      .sort((a, b) => b.priority - a.priority)
      .slice(0, 10);
  }, [data]);

  return (
    <div>
      {processedData.map(item => (
        <ItemComponent key={item.id} item={item} />
      ))}
    </div>
  );
};

// ✅ Component memoization
const ItemComponent = React.memo<{ item: ComplexData }>(({ item }) => {
  return <div>{item.name}</div>;
});

// ✅ Callback memoization
const UserList: React.FC<{ users: User[] }> = ({ users }) => {
  const handleUserClick = useCallback((userId: string) => {
    // Handle user click
    console.log('User clicked:', userId);
  }, []);

  return (
    <div>
      {users.map(user => (
        <UserItem
          key={user.id}
          user={user}
          onClick={handleUserClick}
        />
      ))}
    </div>
  );
};
```

### Code Splitting & Lazy Loading
```typescript
// ✅ Route-based code splitting
const Home = lazy(() => import('../pages/Home'));
const Profile = lazy(() => import('../pages/Profile'));
const Settings = lazy(() => import('../pages/Settings'));

const App: React.FC = () => {
  return (
    <Router>
      <Suspense fallback={<LoadingSpinner />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/settings" element={<Settings />} />
        </Routes>
      </Suspense>
    </Router>
  );
};

// ✅ Component-based lazy loading
const HeavyChart = lazy(() => import('../components/HeavyChart'));

const Dashboard: React.FC = () => {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <h1>Dashboard</h1>
      <button onClick={() => setShowChart(true)}>
        Show Chart
      </button>

      {showChart && (
        <Suspense fallback={<div>Loading chart...</div>}>
          <HeavyChart />
        </Suspense>
      )}
    </div>
  );
};
```

### Image Optimization
```typescript
// ✅ Next.js Image optimization (if using Next.js)
import Image from 'next/image';

const ProfileImage: React.FC<{ user: User }> = ({ user }) => {
  return (
    <Image
      src={user.avatarUrl}
      alt={`${user.firstName} ${user.lastName}`}
      width={100}
      height={100}
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,..."
      priority={false} // Set to true for above-the-fold images
    />
  );
};

// ✅ Generic image with loading states
const OptimizedImage: React.FC<{
  src: string;
  alt: string;
  className?: string;
}> = ({ src, alt, className = '' }) => {
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(false);

  return (
    <div className={`relative ${className}`}>
      {loading && <div className="absolute inset-0 bg-gray-200 animate-pulse" />}
      {error && <div className="absolute inset-0 bg-gray-100 flex items-center justify-center">Failed to load</div>}
      <img
        src={src}
        alt={alt}
        onLoad={() => setLoading(false)}
        onError={() => {
          setLoading(false);
          setError(true);
        }}
        className={loading ? 'opacity-0' : 'opacity-100 transition-opacity'}
      />
    </div>
  );
};
```

---

## Testing with Jest

### Unit Testing Best Practices

#### Component Testing Setup
```typescript
// setupTests.ts
import '@testing-library/jest-dom';
import { configure } from '@testing-library/react';

// Configure testing library
configure({ testIdAttribute: 'data-testid' });

// Mock IntersectionObserver
global.IntersectionObserver = class IntersectionObserver {
  constructor() {}
  observe() { return null; }
  disconnect() { return null; }
  unobserve() { return null; }
};

// Mock matchMedia
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(),
    removeListener: jest.fn(),
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```

#### Testing Components
```typescript
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from './Button';

describe('Button Component', () => {
  it('renders with correct text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
  });

  it('handles click events', async () => {
    const handleClick = jest.fn();
    const user = userEvent.setup();

    render(<Button onClick={handleClick}>Click me</Button>);

    await user.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('applies correct variant styles', () => {
    render(<Button variant="primary">Primary Button</Button>);
    const button = screen.getByRole('button');
    expect(button).toHaveClass('btn-primary');
  });

  it('is disabled when loading', () => {
    render(<Button loading>Loading Button</Button>);
    const button = screen.getByRole('button');
    expect(button).toBeDisabled();
    expect(screen.getByText(/loading/i)).toBeInTheDocument();
  });

  it('renders with custom className', () => {
    render(<Button className="custom-class">Button</Button>);
    expect(screen.getByRole('button')).toHaveClass('custom-class');
  });
});
```

#### Testing Custom Hooks
```typescript
// useCounter.test.ts
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter Hook', () => {
  it('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });

  it('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });

  it('increments count', () => {
    const { result } = renderHook(() => useCounter());

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

  it('resets count to initial value', () => {
    const { result } = renderHook(() => useCounter(10));

    act(() => {
      result.current.increment();
      result.current.increment();
      result.current.reset();
    });

    expect(result.current.count).toBe(10);
  });
});
```

#### Testing with Context
```typescript
// AuthProvider.test.tsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { AuthProvider, useAuth } from './AuthProvider';

// Test component that uses the context
const TestComponent: React.FC = () => {
  const { user, login, logout, loading } = useAuth();

  return (
    <div>
      <div>User: {user?.name || 'Not logged in'}</div>
      <div>Loading: {loading ? 'Yes' : 'No'}</div>
      <button onClick={() => login({ email: 'test@example.com', password: 'password' })}>
        Login
      </button>
      <button onClick={logout}>Logout</button>
    </div>
  );
};

const renderWithAuth = (ui: React.ReactElement) => {
  return render(
    <AuthProvider>
      {ui}
    </AuthProvider>
  );
};

describe('AuthProvider', () => {
  it('provides initial state', () => {
    renderWithAuth(<TestComponent />);

    expect(screen.getByText('User: Not logged in')).toBeInTheDocument();
    expect(screen.getByText('Loading: No')).toBeInTheDocument();
  });

  it('handles login flow', async () => {
    const user = userEvent.setup();
    renderWithAuth(<TestComponent />);

    await user.click(screen.getByText('Login'));

    expect(screen.getByText('Loading: Yes')).toBeInTheDocument();

    await waitFor(() => {
      expect(screen.getByText(/User: /)).toBeInTheDocument();
    });
  });
});
```

#### Testing API Integration
```typescript
// UserService.test.ts
import { rest } from 'msw';
import { setupServer } from 'msw/node';
import { userService } from './userService';

const server = setupServer(
  rest.get('/api/users/:id', (req, res, ctx) => {
    const { id } = req.params;
    return res(
      ctx.json({
        id,
        name: 'John Doe',
        email: 'john@example.com'
      })
    );
  }),

  rest.post('/api/users', (req, res, ctx) => {
    return res(
      ctx.status(201),
      ctx.json({
        id: '123',
        name: 'New User',
        email: 'new@example.com'
      })
    );
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('UserService', () => {
  it('fetches user by id', async () => {
    const user = await userService.getUser('123');

    expect(user).toEqual({
      id: '123',
      name: 'John Doe',
      email: 'john@example.com'
    });
  });

  it('creates new user', async () => {
    const newUser = {
      name: 'New User',
      email: 'new@example.com'
    };

    const result = await userService.createUser(newUser);

    expect(result).toEqual({
      id: '123',
      name: 'New User',
      email: 'new@example.com'
    });
  });

  it('handles API errors', async () => {
    server.use(
      rest.get('/api/users/:id', (req, res, ctx) => {
        return res(ctx.status(404), ctx.json({ error: 'User not found' }));
      })
    );

    await expect(userService.getUser('999')).rejects.toThrow('User not found');
  });
});
```

### Test Organization & Utilities
```typescript
// test-utils.tsx
import React from 'react';
import { render, RenderOptions } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { AuthProvider } from '../contexts/AuthContext';
import { ThemeProvider } from '../contexts/ThemeContext';

interface AllTheProvidersProps {
  children: React.ReactNode;
}

const AllTheProviders: React.FC<AllTheProvidersProps> = ({ children }) => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });

  return (
    <QueryClientProvider client={queryClient}>
      <BrowserRouter>
        <AuthProvider>
          <ThemeProvider>
            {children}
          </ThemeProvider>
        </AuthProvider>
      </BrowserRouter>
    </QueryClientProvider>
  );
};

const customRender = (
  ui: React.ReactElement,
  options?: Omit<RenderOptions, 'wrapper'>
) => render(ui, { wrapper: AllTheProviders, ...options });

export * from '@testing-library/react';
export { customRender as render };

// Custom matchers
export const expectToBeInTheDocument = (element: HTMLElement