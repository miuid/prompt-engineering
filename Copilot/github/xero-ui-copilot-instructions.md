# Copilot Instructions - Payments One Onboarding UI

## Project Overview
**Purpose**: React SPA for customer onboarding with external payment providers in Xero's Payments Core platform  
**Architecture**: Redux-Saga based state management with BFF (Backend for Frontend) pattern  
**Regions**: Supports AU onboarding processing (with UK/US expansion planned)  
**Base Framework**: Built on `@xero/spa-boilerplate-tools`

### Key Technologies
- **React 18.3.1** with functional components and hooks only
- **TypeScript 5.8.3** with strict type checking
- **Redux Toolkit 2.8.2 + Redux-Saga 1.3.0** for state management
- **React Router 6.30.1** for navigation
- **@xero/xui** for UI components (design system)
- **React Intl** for internationalization
- **Jest 29.7.0 + React Testing Library 16.3.0** for testing
- **Playwright** for E2E testing
- **MSW (Mock Service Worker)** for API mocking

## Project Structure
```
src/
├── actions/           # Redux actions (camelCase exports)
├── api/              # API layer with ports pattern
├── components/       # React components
│   ├── functional/   # Reusable functional components
│   └── routes/       # Route-level components
├── constants/        # Application constants
├── helpers/          # Utility functions and helpers
├── hooks/           # Custom React hooks
├── mocks/           # Mock data for testing
├── models/          # TypeScript type definitions
├── reducers/        # Redux reducers
├── sagas/           # Redux-Saga middleware
├── selectors/       # Redux selectors
├── locales/         # Internationalization files
└── scss/            # Global styles with CSS Modules
```

## React Best Practices

### Component Patterns
```tsx
// MUST use functional components only (class components prohibited)
// NO React.FC/FunctionComponent types (ESLint prohibited)
import { componentClassName } from "~helpers";
export const createClass = componentClassName("ComponentName");

const MyComponent = ({ title, onSave }: IProps) => {
  // Group hooks by type: state, selectors, callbacks, effects
  const [isLoading, setIsLoading] = React.useState(false);
  const document = useSelector(selectors.getDocumentById(id));
  
  // Use useCallback ONLY for event handlers (e.g., handleOnBlur)
  const handleSave = React.useCallback(() => {
    dispatch(actions.saveDocument(document));
  }, [document]);
  
  // Use effects with specific dependencies, add comments for purpose
  React.useEffect(() => {
    // Fetch document when component mounts if not already loaded
    if (!document && !isLoading) {
      dispatch(actions.fetchDocument(id));
    }
  }, [id, document, isLoading]);

  return <div className={createClass()}>{title}</div>;
};

// Apply React.memo to dumb components for performance
export default React.memo(MyComponent);
```

### Component Organization & Principles
- **Keep components lightweight** - No business logic, only rendering and UI events
- **API requests only in sagas** - Never in components
- **Single responsibility principle** - Each component handles one specific UI concern
- **Smart vs Dumb categorization** - Smart (connected to Redux) vs Dumb (pure)
- **Avoid object destructuring in components** - Prevents memory allocations
- **Use collapsible region comments** for complex components:
  ```tsx
  // region State
  const [value, setValue] = React.useState();
  // endregion State
  
  // region Event handlers  
  const onSave = React.useCallback(() => {}, []);
  // endregion Event handlers
  ```

## XUI Design System

### Core Principles
- **Use XUI components exclusively** - Never create custom HTML elements when XUI equivalents exist
- **Follow XUI naming conventions** - Components are prefixed with `XUI`
- **Import from specific paths** - Use deep imports like `@xero/xui/react/button`
- **WCAG compliance mandatory** - Accessibility is non-negotiable

### Common XUI Components
```tsx
// Layout Components
import { XUIPanel, XUIPanelSection } from "@xero/xui/react/panel";
import { XUIPageHeader } from "@xero/xui/react/pageheader";

// Interactive Components  
import XUIButton from "@xero/xui/react/button";
import XUILoader from "@xero/xui/react/loader";

// Icons
import XUIIcon from "@xero/xui/react/components/icon/XUIIcon";
import external from "@xero/xui-icon/icons/external";

// Usage Examples
<XUIPanel className="xui-page-width-large">
  <XUIPanelSection className="xui-padding-large">
    <XUIButton onClick={handleClick}>
      <FormattedMessage id="SAVE" />
    </XUIButton>
  </XUIPanelSection>
</XUIPanel>

<XUILoader 
  ariaLabel={formatMessage({ id: "LOADING" })}
  className={createClass("loader")}
/>
```

### Styling Guidelines
- **SCSS with CSS Modules** - Component-scoped styling
- **XUI utility classes** - `xui-page-width-large`, `xui-padding-large`
- **CSS Classes** - Use `kebab-case` for class names
- **Responsive Design** - Mobile-first approach, XUI handles responsive patterns

## Redux Architecture & Naming Conventions

### Naming Standards
- **Actions**: `[verb][~noun]` format (e.g., `fetchBatchData`)
- **Action Types**: `[PREFIX]/[verb]_[noun]` format (e.g., `BATCH/FETCH_BATCH_DATA`)
- **Selectors**: `get/is[stateKey]` format (e.g., `getBatchData`, `isModalOpen`)
- **Reducers**: `set[action/noun]` format (e.g., `setBatchData`)
- **Event Handlers**: `handle` prefix for callbacks (e.g., `handleOnBlur`)
- **Implementation Functions**: `handler` suffix (e.g., `onBlurHandler`)

### File Organization
- **Actions**: `[resource-group].actions.ts` (e.g., `batch.actions.ts`)
- **Selectors**: `[resource-group].selectors.ts` (e.g., `batch.selectors.ts`)
- **Reducers**: `[resource].reducers.ts` (e.g., `batch.reducers.ts`)
- **API**: `src/api/[methodName].api.ts`
- **Components**: `src/components/functional/[ComponentName]/` or `src/components/routes/[RouteName]/`
- Use `camelCase` for files with multiple exports, `PascalCase` for component files

### Ports Pattern (Critical)
```typescript
// API methods injected via configurePorts() in App.tsx
export const fetchDocuments = (request: TRequest) => async (query: string) => {
  try {
    const response = await request('/api/documents', { params: { query } });
    return success(documentsModel(response.data));
  } catch (error) {
    return failure(error);
  }
};
```

### Saga Patterns
```typescript
// Sagas must be defined with ports injection
export const fetchDocumentsSaga = ({ api }: TPorts) =>
  function* (): SagaIterator {
    yield takeLatest(actions.fetchDocuments, function* ({ payload }): SagaIterator {
      const response: SagaReturnType<typeof api.fetchDocuments> = yield call(
        api.fetchDocuments, payload.query
      );
      
      if (response.ok) {
        yield put(actions.fetchDocumentsDone(response.data));
      } else {
        yield put(actions.fetchDocumentsFailed());
      }
    });
  };
```

### Selector Patterns  
```typescript
// Always define selectors for store access, use createSelector for memoization
export const getDocuments = (state: TStoreState) => state.documents.items;
export const getFilteredDocuments = createSelector(
  [getDocuments, getSearchQuery],
  (documents, query) => documents.filter(doc => doc.title.includes(query))
);
```

## Testing Standards

### Coverage & Quality Requirements
- **100% Coverage Required** - Branches, functions, lines, and statements
- **Testing Libraries** - Jest + React Testing Library + Redux-Saga-Test-Plan
- **File Naming** - `[component].test.tsx`
- **Test Structure** - One test file per component, single assertion per test
- **Test Naming** - Lowercase, action-first, grouped by behavior

### Component Testing
```typescript
// Use custom Wrapper class with ports and Redux mocking
import { Wrapper, screen } from "~test-utils";

const component = new Wrapper(MyComponent)
  .withDefaultProps({ title: "Test" })
  .withDefaultReduxState({ documents: { items: [] } });

describe("[components] <MyComponent />", () => {
  it("renders the component", () => {
    component.render();
  });
  
  // Test outcomes, not internals - Find by role/text first
  it("renders the title", () => {
    expect(screen.getByText("Test")).toBeDefined();
  });
  
  // Use userEvent, not fireEvent - Always test callback actions with correct parameters
  it("calls onSave when button clicked", async () => {
    await userEvent.click(screen.getByRole("button", { name: "Save" }));
    expect(component.reduxHistory.filter(actions.saveDocument.match)).toHaveLength(1);
  });
});
```

### Saga Testing
```typescript
// Use SagaTester for saga testing with full context
import { SagaTester } from "~test-utils";

const tester = new SagaTester(
  { documents: { items: [] } }, // Redux state
  { api: { fetchDocuments: mockWithSuccess([]) } } // Mock ports
);

it("dispatches success action when API succeeds", async () => {
  tester.dispatch(actions.fetchDocuments("query"));
  await tester.waitFor(actions.fetchDocumentsDone);
  
  expect(tester.history.filter(actions.fetchDocumentsDone.match)).toHaveLength(1);
});
```

### Testing XUI Components
```typescript
// Test by role and accessible name, not by XUI class names
expect(screen.getByRole("button", { name: "Save" })).toBeDefined();

// For custom identifiers when needed
expect(screen.getByClassName(createClass("loader"))).toBeDefined();

// Avoid testing XUI internal classes
// ❌ Don't do: screen.getByClassName("xui-button")
// ✅ Do: screen.getByRole("button")
```

### Test Example Structure
```javascript
describe('PayeeReferenceInput', () => {
  it('renders the component', () => {});

  describe('onBlur', () => {
    it('calls handler with correct value', () => {});
    it('shows error when input contains invalid characters', () => {});
  });
});
```

## Development Workflow & Implementation Process

### Implementation Steps
1. **Plan work before implementation** - Architecture and data flow
2. **Follow current project structure** - Proper folder organization  
3. **Define TypeScript interfaces** - Strong typing throughout
4. **Add state management** - Redux + Sagas for async operations
5. **Implement routing** - React Router integration
6. **Add form handling** - Validation and error states
7. **Implement comprehensive error handling** - User-friendly messages
8. **Add testing coverage** - 100% coverage requirement
9. **Optimize performance** - Bundle size and memory management
10. **Ensure accessibility compliance** - WCAG standards
11. **Add documentation** - Code comments and README updates

### Essential Commands
```bash
# Development
yarn start                # Dev server with MSW
yarn start:nomsw          # Start without MSW (UAT/Staging)
yarn start:proxy          # Start with local API proxy

# Testing
yarn test                 # Unit tests
yarn test:coverage        # Tests with coverage
yarn test:playwright      # E2E tests
yarn test:all            # Full test suite (pre-commit)

# Code Quality
yarn lint                 # Run all linters
yarn lint:fix            # Fix linting issues
yarn typecheck           # TypeScript compilation check

# Build
yarn build               # Production build
yarn storybook           # Start Storybook
```

### Quality Gates & Constraints
- **100% test coverage** required (not 90%)
- **No Redux useReducer** - Always use Redux + Sagas for state management
- **No API calls in components** - Only in sagas via ports
- **All async logic in sagas** - Components only for rendering and dispatching
- **No direct Redux store access** - Always use selectors
- **Always use ports pattern** - For testability and dependency injection

## Import Patterns
```typescript
// Always use ~ aliases, never relative imports
import * as actions from "~actions";
import * as selectors from "~selectors";
import { componentClassName } from "~helpers";
import { screen, Wrapper } from "~test-utils";
```

## Integration Points & Regional Support

### External Integrations
- **Shell.js** - Microfrontend container (auth, navigation, org context)
- **LaunchDarkly** - Feature flags via `props.initialFeatures`
- **Analytics** - Via `props.analytics` from shell
- **Monoova** - AU onboarding payment partner

### Regional Features
- **AU/UK/US** - Different onboarding flows per region
- **Validation** - Region-specific validation rules
- **Testing** - Use region-specific test data patterns

## API Integration & Performance

### BFF Pattern & Error Handling
- **Single API calls per page** - Let BFF aggregate data
- **Comprehensive error states** - User-friendly error messages and recovery paths
- **Loading States** - Proper loading indicators for all async operations
- **Mock Service Worker** - Use MSW for development and testing

### Performance Optimization
- **Code Splitting** - Lazy loading for route components
- **Bundle Analysis** - Monitor bundle size
- **Memory Management** - Avoid unnecessary re-renders, no object destructuring
- **API Optimization** - Minimize API calls, efficient data fetching

## Critical Reminders

⚠️ **Never modify boilerplate files** - Breaks upgrade automation and support  
⚠️ **No direct Redux store access** - Always use selectors  
⚠️ **Always use ports pattern** - For testability and dependency injection  
⚠️ **100% test coverage mandatory** - No exceptions  
⚠️ **XUI components only** - Never create custom HTML when XUI exists  
⚠️ **Functional components only** - Class components are prohibited  
⚠️ **API calls only in sagas** - Never in components