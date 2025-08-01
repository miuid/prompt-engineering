# Frontend Development Instructions for Copilot

## Overview

This document provides comprehensive instructions for developing ReactJS applications with TypeScript using our company's SPA Boilerplate Tools. These guidelines ensure code quality, maintainability, and consistency across all frontend projects.

## Technology Stack

- **Frontend Framework**: ReactJS with TypeScript
- **State Management**: Redux with Redux Toolkit
- **Testing Frameworks**: Jest (unit tests), Playwright (E2E tests)
- **Testing Library**: React Testing Library
- **Build Tool**: Webpack with custom WebpackConfigBuilder
- **Styling**: Sass/SCSS
- **Package Manager**: Yarn
- **Linting**: ESLint + Prettier

## Architecture Principles

### Component Structure
- Use functional components with hooks exclusively
- Create prop-driven components instead of exposing public methods
- Keep components lightweight and focused on single responsibility
- Split components when they become too large or handle multiple concerns
- Implement lazy loading for route components

### Project Organization
```
src/
├── actions/           # Redux action creators
├── api/               # API service functions
├── components/        # Reusable UI components
├── constants/         # Application constants
├── helpers/          # Utility functions
├── hooks/            # Custom React hooks
├── locales/          # Localization files and translations
├── models/           # TypeScript interfaces and types
├── reducers/         # Redux reducers
├── sagas/            # Redux-Saga effects
├── selectors/        # Redux selectors
├── types/            # Global TypeScript types
├── utilities/        # Testing utilities and helpers
└── validators/       # Input validation functions
```

## ReactJS Best Practices

### Component Development
```typescript
// ✅ Good: Functional component with proper typing
interface DocumentListProps {
  documents: Document[];
  onDocumentClick: (id: string) => void;
  isLoading?: boolean;
}

export const DocumentList: React.FC<DocumentListProps> = ({
  documents,
  onDocumentClick,
  isLoading = false
}) => {
  // Use early returns for conditional rendering
  if (isLoading) {
    return <LoadingSpinner />;
  }

  if (documents.length === 0) {
    return <EmptyState message="No documents found" />;
  }

  return (
    <ul className={createClass("document-list")}>
      {documents.map((doc) => (
        <DocumentItem
          key={doc.id}
          document={doc}
          onClick={() => onDocumentClick(doc.id)}
        />
      ))}
    </ul>
  );
};
```

### Hook Usage
```typescript
// ✅ Good: Custom hook for document management
export const useDocuments = () => {
  const [documents, setDocuments] = useState<Document[]>([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const fetchDocuments = useCallback(async () => {
    setIsLoading(true);
    setError(null);

    try {
      const response = await api.getDocuments();
      setDocuments(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Unknown error');
    } finally {
      setIsLoading(false);
    }
  }, []);

  return { documents, isLoading, error, fetchDocuments };
};
```

### State Management Patterns
```typescript
// ✅ Good: Keep state minimal and derived data in selectors
const useDocumentState = () => {
  const documents = useSelector(selectDocuments);
  const filteredDocuments = useSelector(selectFilteredDocuments);
  const isLoading = useSelector(selectDocumentsLoading);

  const dispatch = useDispatch();

  const actions = useMemo(() => ({
    fetchDocuments: () => dispatch(documentsActions.fetchDocuments()),
    updateDocument: (doc: Document) => dispatch(documentsActions.updateDocument(doc)),
    deleteDocument: (id: string) => dispatch(documentsActions.deleteDocument(id))
  }), [dispatch]);

  return { documents, filteredDocuments, isLoading, actions };
};
```

## TypeScript Guidelines

### Interface and Type Definitions
```typescript
// ✅ Good: Clear, well-documented interfaces
interface Document {
  id: string;
  title: string;
  content: string;
  createdAt: Date;
  updatedAt: Date;
  status: DocumentStatus;
  tags: string[];
}

type DocumentStatus = 'draft' | 'published' | 'archived';

// Use utility types for common patterns
type CreateDocumentRequest = Omit<Document, 'id' | 'createdAt' | 'updatedAt'>;
type UpdateDocumentRequest = Partial<Pick<Document, 'title' | 'content' | 'tags'>>;
```

### Type Safety Best Practices
```typescript
// ✅ Good: Proper error handling with types
interface ApiResponse<T> {
  data: T;
  success: boolean;
  error?: string;
}

const handleApiResponse = <T>(response: ApiResponse<T>): T => {
  if (!response.success) {
    throw new Error(response.error || 'API request failed');
  }
  return response.data;
};

// ✅ Good: Type guards for runtime type checking
const isDocument = (obj: unknown): obj is Document => {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    typeof (obj as Document).id === 'string' &&
    typeof (obj as Document).title === 'string'
  );
};
```

## Redux Implementation

### Action Creators
```typescript
// ✅ Good: Use createAction from Redux Toolkit
export const documentsActions = {
  fetchDocuments: createAction('documents/fetchDocuments'),
  fetchDocumentsSuccess: createAction<Document[]>('documents/fetchDocumentsSuccess'),
  fetchDocumentsFailed: createAction<string>('documents/fetchDocumentsFailed'),
  updateDocument: createAction<Document>('documents/updateDocument'),
  deleteDocument: createAction<string>('documents/deleteDocument')
};
```

### Reducers
```typescript
// ✅ Good: Use createSlice for reducers
const documentsSlice = createSlice({
  name: 'documents',
  initialState: {
    items: [] as Document[],
    loading: false,
    error: null as string | null
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(documentsActions.fetchDocuments, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(documentsActions.fetchDocumentsSuccess, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(documentsActions.fetchDocumentsFailed, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});
```

### Selectors
```typescript
// ✅ Good: Use createSelector for memoized selectors
export const selectDocuments = (state: RootState) => state.documents.items;
export const selectDocumentsLoading = (state: RootState) => state.documents.loading;

export const selectDocumentById = createSelector(
  [selectDocuments, (_: RootState, id: string) => id],
  (documents, id) => documents.find(doc => doc.id === id)
);

export const selectFilteredDocuments = createSelector(
  [selectDocuments, selectSearchTerm],
  (documents, searchTerm) =>
    documents.filter(doc =>
      doc.title.toLowerCase().includes(searchTerm.toLowerCase())
    )
);
```

## Testing Guidelines

### Unit Testing with Jest and React Testing Library

#### Component Testing
```typescript
// ✅ Good: Component test structure
describe('[components] <DocumentList />', () => {
  const mockDocuments: Document[] = [
    { id: '1', title: 'Test Doc', content: 'Content', status: 'draft', tags: [] }
  ];

  describe('when documents are provided', () => {
    it('renders the document list', () => {
      render(
        <DocumentList
          documents={mockDocuments}
          onDocumentClick={jest.fn()}
        />
      );

      expect(screen.getByRole('list')).toBeInTheDocument();
      expect(screen.getByText('Test Doc')).toBeInTheDocument();
    });

    it('calls onDocumentClick when document is clicked', async () => {
      const mockOnClick = jest.fn();
      render(
        <DocumentList
          documents={mockDocuments}
          onDocumentClick={mockOnClick}
        />
      );

      await userEvent.click(screen.getByText('Test Doc'));
      expect(mockOnClick).toHaveBeenCalledWith('1');
    });
  });

  describe('when loading is true', () => {
    it('renders loading spinner', () => {
      render(
        <DocumentList
          documents={[]}
          onDocumentClick={jest.fn()}
          isLoading={true}
        />
      );

      expect(screen.getByRole('progressbar')).toBeInTheDocument();
    });
  });
});
```

#### Query Priority Guidelines
```typescript
// ✅ Good: Use queries in order of preference
// 1. Role-based queries (most preferred)
screen.getByRole('button', { name: /submit/i });

// 2. Label-based queries
screen.getByLabelText(/email address/i);

// 3. Text content queries
screen.getByText(/welcome message/i);

// 4. Test ID queries (use data-automationid)
screen.getByTestId('document-list');

// 5. Class-based queries (least preferred, use createClass helper)
screen.getByClassName(createClass('document-item'));
```

#### Async Testing
```typescript
// ✅ Good: Async testing patterns
describe('when loading documents', () => {
  it('shows loading state then displays documents', async () => {
    const mockFetch = jest.fn().mockResolvedValue({ data: mockDocuments });

    render(<DocumentContainer fetchDocuments={mockFetch} />);

    // Assert loading state
    expect(screen.getByRole('progressbar')).toBeInTheDocument();

    // Wait for documents to load
    await waitFor(() => {
      expect(screen.queryByRole('progressbar')).not.toBeInTheDocument();
    });

    expect(screen.getByText('Test Doc')).toBeInTheDocument();
  });
});
```

### E2E Testing with Playwright

#### Test Structure
```typescript
// ✅ Good: Playwright test structure
import { test } from "@xero/spa-boilerplate-tools/playwright";

test.describe('[routes] DocumentRoute (/documents)', () => {
  test.describe('when viewing documents', () => {
    test('displays document list correctly', async ({ page }) => {
      await test.step('navigates to documents page', async () => {
        await gotoAndLogin(page, '/documents');
      });

      await test.step('displays document list', async () => {
        await expect(page.getByRole('heading', { name: 'Documents' })).toBeVisible();
        await expect(page.getByRole('list')).toBeVisible();
      });

      await test.step('has no accessibility violations', async () => {
        await injectAxe(page);
        expect(await getViolations(page, '#shell-app-root')).toEqual([]);
      });
    });
  });
});
```

### Testing Requirements
- **Coverage**: 100% line, branch, function, and statement coverage
- **Test Location**: Adjacent to source files (e.g., `Component.test.tsx` next to `Component.tsx`)
- **Naming**: Use descriptive test names that explain the scenario and expectation
- **Structure**: Use nested `describe` blocks to create clear test hierarchies

## Code Quality Standards

### Linting and Formatting
- Follow ESLint configuration provided in the project
- Use Prettier for consistent code formatting
- Enable TypeScript strict mode
- Use meaningful variable and function names

### Performance Considerations
```typescript
// ✅ Good: Memoization for expensive calculations
const ExpensiveComponent: React.FC<Props> = ({ data }) => {
  const processedData = useMemo(() => {
    return data.map(item => processExpensiveOperation(item));
  }, [data]);

  const handleClick = useCallback((id: string) => {
    onItemClick(id);
  }, [onItemClick]);

  return <ProcessedList data={processedData} onItemClick={handleClick} />;
};

// ✅ Good: Lazy loading for routes
const DocumentRoute = lazy(() => import('./routes/DocumentRoute'));
const SettingsRoute = lazy(() => import('./routes/SettingsRoute'));
```

### Error Handling
```typescript
// ✅ Good: Consistent error handling
const useDocumentActions = () => {
  const [error, setError] = useState<string | null>(null);

  const saveDocument = useCallback(async (document: Document) => {
    try {
      setError(null);
      await api.saveDocument(document);
      showToast({ type: 'success', message: 'Document saved successfully' });
    } catch (err) {
      const errorMessage = err instanceof Error ? err.message : 'Failed to save document';
      setError(errorMessage);
      showToast({ type: 'error', message: errorMessage });
    }
  }, []);

  return { saveDocument, error };
};
```

## Accessibility Guidelines

### ARIA and Semantic HTML
```typescript
// ✅ Good: Semantic HTML with proper ARIA
const DocumentList: React.FC<Props> = ({ documents, searchTerm }) => (
  <section aria-labelledby="documents-heading">
    <h2 id="documents-heading">Documents</h2>
    <div role="search">
      <label htmlFor="search-input">Search documents</label>
      <input
        id="search-input"
        type="search"
        value={searchTerm}
        aria-describedby="search-help"
      />
      <div id="search-help">
        Search by document title or content
      </div>
    </div>
    <ul role="list" aria-live="polite">
      {documents.map(doc => (
        <li key={doc.id}>
          <button
            onClick={() => onDocumentSelect(doc.id)}
            aria-describedby={`doc-${doc.id}-meta`}
          >
            {doc.title}
          </button>
          <div id={`doc-${doc.id}-meta`}>
            Last updated: {formatDate(doc.updatedAt)}
          </div>
        </li>
      ))}
    </ul>
  </section>
);
```

## Internationalization

### Message Definition
```typescript
// ✅ Good: Internationalization setup
export const messages = defineMessages({
  DOCUMENT_TITLE: {
    id: 'documents.title',
    defaultMessage: 'Documents'
  },
  SAVE_BUTTON: {
    id: 'documents.save',
    defaultMessage: 'Save Document'
  },
  DELETE_CONFIRMATION: {
    id: 'documents.deleteConfirmation',
    defaultMessage: 'Are you sure you want to delete this document?'
  }
});

// Usage in components
const DocumentHeader: React.FC = () => {
  const intl = useIntl();

  return (
    <h1>{intl.formatMessage(messages.DOCUMENT_TITLE)}</h1>
  );
};
```

## Build and Deployment

### Environment Configuration
```typescript
// ✅ Good: Environment-specific configuration
interface AppConfig {
  apiBaseUrl: string;
  enableDevTools: boolean;
  logLevel: 'debug' | 'info' | 'warn' | 'error';
}

const config: AppConfig = {
  apiBaseUrl: process.env.REACT_APP_API_BASE_URL || 'http://localhost:3001',
  enableDevTools: process.env.NODE_ENV === 'development',
  logLevel: (process.env.REACT_APP_LOG_LEVEL as AppConfig['logLevel']) || 'info'
};
```

### Webpack Configuration
- Use the provided `WebpackConfigBuilder` for consistent builds
- Configure CDN asset paths in `webpack.config.js`
- Enable code splitting for route-based chunks
- Configure proper source maps for debugging

## Security Best Practices

### Input Validation
```typescript
// ✅ Good: Input validation and sanitization
const validateDocumentInput = (input: unknown): Document => {
  if (!isPlainObject(input)) {
    throw new Error('Invalid document format');
  }

  const { title, content } = input as Record<string, unknown>;

  if (typeof title !== 'string' || title.trim().length === 0) {
    throw new Error('Document title is required');
  }

  if (typeof content !== 'string') {
    throw new Error('Document content must be a string');
  }

  return {
    id: generateId(),
    title: sanitizeText(title),
    content: sanitizeText(content),
    createdAt: new Date(),
    updatedAt: new Date(),
    status: 'draft',
    tags: []
  };
};
```

### Secure API Communication
```typescript
// ✅ Good: Secure API configuration
const apiClient = axios.create({
  baseURL: config.apiBaseUrl,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
    'X-Requested-With': 'XMLHttpRequest'
  }
});

// Add auth token to requests
apiClient.interceptors.request.use((config) => {
  const token = getAuthToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

## Common Patterns and Anti-Patterns

### ✅ Good Patterns
```typescript
// Consistent error boundaries
class DocumentErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    logger.error('Document error boundary caught error:', { error, errorInfo });
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback error={this.state.error} />;
    }

    return this.props.children;
  }
}

// Custom hooks for business logic
const useDocumentManagement = () => {
  const [documents, setDocuments] = useState<Document[]>([]);
  const [selectedDocument, setSelectedDocument] = useState<Document | null>(null);

  const selectDocument = useCallback((id: string) => {
    const doc = documents.find(d => d.id === id);
    setSelectedDocument(doc || null);
  }, [documents]);

  return { documents, selectedDocument, selectDocument };
};
```

### ❌ Anti-Patterns to Avoid
```typescript
// ❌ Don't mutate props or state directly
const BadComponent = ({ items }) => {
  items.push(newItem); // Never do this
  return <List items={items} />;
};

// ❌ Don't use any type
const badFunction = (data: any) => { // Avoid any
  return data.someProperty;
};

// ❌ Don't test implementation details
// Bad test
expect(component.state.isVisible).toBe(true);

// Good test
expect(screen.getByRole('dialog')).toBeVisible();

// ❌ Don't use magic numbers or strings
const BadComponent = () => (
  <div style={{ width: 300 }}> {/* Use constants */}
    <span>Status: active</span> {/* Use enums/constants */}
  </div>
);
```

## Conclusion

Following these guidelines ensures:
- **Consistency**: Uniform code style and patterns across the application
- **Maintainability**: Code that is easy to understand, modify, and extend
- **Quality**: High test coverage and robust error handling
- **Performance**: Optimized React components and efficient state management
- **Accessibility**: Inclusive user experiences for all users
- **Security**: Safe handling of user data and API communications

Always refer to the project's existing code examples and documentation for specific implementation details and updated patterns.