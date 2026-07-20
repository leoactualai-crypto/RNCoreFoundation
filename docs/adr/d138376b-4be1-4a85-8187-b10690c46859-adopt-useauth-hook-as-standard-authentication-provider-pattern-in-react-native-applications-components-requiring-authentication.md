# Adopt useAuth Hook as Standard Authentication Provider Pattern in React Native Applications: Components Requiring Authentication

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all React Native application components that require authentication state management.

## Context

- The application uses React Native with React Navigation stack navigator, requiring consistent authentication state management across navigation boundaries
- A custom useAuth() hook pattern has been established to provide authentication context to components, enabling declarative access to authentication state
- The application structure includes styled components with StyleSheet.create patterns, indicating a component-based architecture where authentication state needs to be accessible throughout the component tree
- React hooks-based state management (useState) is already in use, establishing a functional component pattern that aligns with modern React authentication approaches

## Problem Statement

React Native applications with navigation stacks require a consistent, accessible authentication state management solution that works across screen boundaries and component hierarchies without prop drilling or tight coupling to specific authentication implementations.

## Decision

1. MUST: All components requiring authentication state MUST access it through the useAuth() hook rather than direct state management or prop passing

## Policy Block

- MUST All components requiring authentication state MUST access it through the useAuth() hook rather than direct state management or prop passing

In scope:
- All React Native functional components requiring authentication state
- Screen components within React Navigation stack navigators
- Custom hooks that depend on authentication context
- Components rendering conditional UI based on authentication status

Out of scope:
- Native module code outside the React component tree
- Backend API authentication logic
- Token storage mechanisms (AsyncStorage, Keychain)
- Third-party authentication SDK initialization

Exceptions:
- EXC-001: Authentication provider setup code at application root level
- EXC-002: Testing utilities that mock authentication state

## Rationale

- The detected pattern shows useAuth() usage in App.tsx, indicating an established authentication provider pattern that centralizes authentication state management
- React Navigation stack navigator usage requires authentication state to be accessible across screen transitions without passing props through navigation parameters
- The hooks-based approach (useState detected) aligns with React best practices and enables composition of authentication logic with other hooks
- Centralizing authentication through a custom hook reduces coupling and enables easier testing, mocking, and authentication provider swapping

## Consequences

Positive:
- Consistent authentication state access pattern across all application components
- Reduced prop drilling and simplified component interfaces
- Easier testing through mockable hook interface
- Decoupled authentication implementation from component logic, enabling provider changes without component modifications

Negative:
- Additional Context provider overhead in the component tree
- Potential for unnecessary re-renders if authentication context is not optimized with memoization
- Learning curve for developers unfamiliar with React Context and custom hooks patterns
- Debugging authentication issues may require understanding Context propagation and hook lifecycle

## Alternatives

- Pass authentication state through React Navigation screen params (rejected)
  Rejected because: Navigation params are designed for screen-specific data, not global application state; this approach leads to prop drilling through navigation hierarchy and tight coupling between navigation and authentication
  When valid: Only for screen-specific authentication tokens or temporary authentication flows isolated to a single navigation stack
- Use Redux or similar state management library for authentication state (rejected)
  Rejected because: Adds significant dependency overhead for a single concern; the detected pattern shows hooks-based state management (useState) without Redux, indicating preference for lighter-weight solutions
  When valid: When application already uses Redux for complex global state management beyond authentication
- Direct AsyncStorage or secure storage access in components (rejected)
  Rejected because: Creates tight coupling to storage implementation, makes testing difficult, and introduces async storage calls throughout component tree leading to inconsistent state and race conditions
  When valid: Never for authentication state; storage should be abstracted behind the authentication provider

## Risks

- Authentication context re-renders may cause performance issues if not properly memoized, affecting all consuming components
  Mitigation: Implement React.memo for components consuming authentication state, use useMemo/useCallback in authentication provider, split authentication context into separate contexts for state and actions
  Owner: Frontend engineering team
- Components may call useAuth() outside of provider scope, causing runtime errors
  Mitigation: Implement error boundary in useAuth() hook that throws descriptive error when called outside provider context, add ESLint rules to detect hook usage patterns
  Owner: Frontend engineering team
- Authentication state synchronization issues between multiple tabs or app instances
  Mitigation: Implement storage event listeners in authentication provider to detect external authentication changes, add token expiration checks on app foreground events
  Owner: Frontend engineering team

## Implementation Notes

- Create AuthProvider component wrapping the root App component, above NavigationContainer, to ensure authentication context is available to all screens
- Implement useAuth() hook with clear TypeScript interfaces defining authentication state shape and available methods (login, logout, refresh)
- Use React.createContext with default values that throw errors when accessed outside provider, ensuring developer feedback for incorrect usage
- Consider splitting authentication context into AuthStateContext and AuthActionsContext to prevent unnecessary re-renders when only actions are needed
- Add comprehensive unit tests for authentication provider and integration tests for useAuth() hook usage in components

## Continuation Context


Verify commands:
- grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts" | wc -l
- grep -r "import.*useAuth" template/src --include="*.tsx" --include="*.ts"
- grep -r "createContext.*Auth" template/src --include="*.tsx" --include="*.ts"

Accept when:
- useAuth() hook is imported and called in at least one component file
- Authentication context provider is present in the application root or App component
- No direct authentication state management (useState for auth tokens) exists outside the authentication provider

## Enforcement

- Verified by: Code review checklist requiring useAuth() pattern for authentication state access
- Verified by: ESLint custom rules detecting direct authentication state management outside provider
- Verified by: Automated grep-based verification in CI pipeline checking for useAuth() usage patterns
- Violation handling: CI pipeline fails if authentication state is managed outside useAuth() pattern
- Violation handling: Code review blocks merge if components access authentication without useAuth() hook
- Violation handling: Automated comments on pull requests identifying non-compliant authentication patterns
- Exception process: Developer documents exception rationale in code comments with EXC-ID reference
- Exception process: Lead developer or architect reviews and approves exception in pull request
- Exception process: Exception is logged in architecture decision log with justification and expiration date