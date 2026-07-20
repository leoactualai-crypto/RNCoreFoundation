# Standardize React Hooks for UI State Management in Public API Components: Authentication State Managed

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native application template exposes public API contracts through screen components (LoginScreen, ChatScreen, AuthStackNavigator) that serve as integration points for external consumers
- UI interaction patterns are implemented using React hooks (useState, useEffect, useMemo, useCallback, useSelector) across 5 files with consistent patterns for state management
- Authentication flow integrates external Strapi API endpoints via axios HTTP client, requiring coordinated state management between UI components and authentication hooks
- Component architecture separates concerns between navigation (react-navigation/stack), UI presentation (react-native-paper), and state management (react-redux, custom hooks)
- StyleSheet.create patterns appear consistently across components, indicating standardized approach to styling within the public API surface

## Problem Statement

Public API components in React Native applications require consistent state management patterns to ensure predictable behavior, maintainability, and integration reliability for consumers. Without standardized hook usage patterns, components may exhibit inconsistent lifecycle behavior, unpredictable re-rendering, and difficult-to-debug state synchronization issues across authentication, navigation, and UI interaction boundaries.

## Decision

1. MUST: Authentication state MUST be managed through custom hooks (useAuth) that encapsulate authentication logic and external API integration

## Policy Block

- MUST Authentication state MUST be managed through custom hooks (useAuth) that encapsulate authentication logic and external API integration

In scope:
- Screen components exported as public API contracts (LoginScreen, ChatScreen, AuthStackNavigator)
- Custom authentication hooks (useAuth) that manage external API integration
- Navigator components using @react-navigation/stack
- Components using react-native-paper UI library
- State management through react-redux and custom hooks

Out of scope:
- Internal utility functions that do not manage component state
- Pure presentational components without state or side effects
- Configuration files and constants
- Test files and mock implementations

## Rationale

- Evidence shows consistent React hooks usage across 5 files (App.tsx, useAuth.ts, AuthStackNavigator.tsx, Chat.tsx, Login.tsx) with 85.66% confidence, indicating established architectural pattern
- Authentication integration with external Strapi API requires coordinated state management between UI components and HTTP client, necessitating standardized hook patterns for predictable behavior
- Public API contracts (RootStackParamList, screen components) serve as integration boundaries requiring stable, well-defined state management patterns for external consumers
- StyleSheet.create usage across all components demonstrates commitment to performance optimization and type safety in the public API surface

## Consequences

Positive:
- Consistent hook usage patterns improve maintainability and reduce cognitive load for developers working across different components
- Custom authentication hooks (useAuth) encapsulate complex authentication logic, providing clean API for screen components
- TypeScript interfaces for navigation parameters enable type-safe routing and compile-time validation of component contracts
- Centralized axios configuration enables consistent error handling and authentication token management across all external API calls

Negative:
- React hooks introduce learning curve for developers unfamiliar with functional component patterns and hook lifecycle rules
- Custom hook abstractions (useAuth) may obscure underlying authentication flow, making debugging more complex
- Dependency on react-redux for global state adds bundle size and requires understanding of Redux patterns alongside React hooks
- Hook dependency arrays require careful management to avoid stale closures or unnecessary re-renders

## Alternatives

- Use class-based React components with lifecycle methods instead of functional components with hooks (rejected)
  Rejected because: Class components are legacy pattern in React ecosystem; hooks provide better code reuse through custom hooks and reduce boilerplate compared to HOCs and render props
  When valid: When maintaining legacy codebases that already use class components extensively
- Use MobX or Zustand for state management instead of react-redux with hooks (rejected)
  Rejected because: Evidence shows established react-redux integration with useSelector hook; changing state management library would require significant refactoring across navigation and authentication boundaries
  When valid: For new projects without existing Redux infrastructure or when simpler state management is sufficient
- Use fetch API instead of axios for HTTP requests (rejected)
  Rejected because: Axios provides interceptors for centralized authentication token injection and error handling, which is critical for managing external API integration across multiple endpoints
  When valid: For simple applications with minimal HTTP client requirements and no need for request/response interceptors

## Risks

- Hook dependency arrays may be incorrectly specified, leading to stale closures or infinite re-render loops
  Mitigation: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage; conduct code reviews focused on hook usage patterns
  Owner: engineering team
- Custom authentication hook (useAuth) creates single point of failure for all authentication flows across the application
  Mitigation: Implement comprehensive error handling in useAuth hook; add unit tests covering authentication success, failure, and network error scenarios; document authentication state machine
  Owner: engineering team
- Axios configuration in centralized config may not be properly secured, exposing API credentials or tokens
  Mitigation: Use react-native-secure-storage for token persistence as evidenced in useAuth.ts; implement token refresh logic; audit axios interceptors for security vulnerabilities
  Owner: engineering team

## Implementation Notes

- Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data)
- Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders
- Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests
- Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation
- Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties

## Continuation Context


Verify commands:
- grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/
- grep -r "StyleSheet.create" template/src/screens/ template/src/components/
- grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules
- npx tsc --noEmit --project tsconfig.json

Accept when:
- All screen components and navigators use React hooks for state management and side effects
- StyleSheet.create is used consistently across all components with styles
- Axios HTTP client is used for all external API calls with centralized configuration
- TypeScript compilation succeeds without errors for all public API contracts

## Enforcement

- Verified by: ESLint with eslint-plugin-react-hooks enforcing hooks rules in CI pipeline
- Verified by: TypeScript compiler checking type safety of navigation parameters and component props
- Verified by: Code review checklist verifying hook usage patterns and dependency arrays
- Verified by: Automated grep-based verification in CI checking for consistent StyleSheet.create usage
- Violation handling: CI build fails if ESLint hooks rules are violated or TypeScript compilation errors occur
- Violation handling: Code review blocks merge if hook patterns deviate from established conventions without documented justification
- Violation handling: Runtime warnings in development mode for missing hook dependencies via React DevTools
- Exception process: Document exception rationale in code comments explaining why standard hook pattern cannot be used
- Exception process: Obtain approval from tech lead for deviations from standard authentication hook usage
- Exception process: Add ESLint disable comments only when absolutely necessary with accompanying explanation