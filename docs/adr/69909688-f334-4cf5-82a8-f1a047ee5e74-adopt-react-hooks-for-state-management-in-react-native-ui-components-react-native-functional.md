# Adopt React Hooks for State Management in React Native UI Components: React Native Functional

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all React Native UI component development within the codebase.

## Context

- The codebase contains multiple React Native screen components (Chat, Login, Registration) that require local state management for user interactions such as form inputs, loading states, and UI feedback.
- React hooks (useState, useCallback, useEffect, useMemo, useSelector) are consistently used across 7 files to manage component lifecycle, state transitions, and side effects in functional components.
- The application integrates with external authentication services via axios and requires coordinated state management between UI components and Redux store for authentication flows.
- React Native Paper components are used throughout the UI, requiring consistent interaction patterns for Material Design-compliant user experiences.
- The navigation stack (@react-navigation/stack) requires state coordination between screens and navigation context, necessitating a standardized approach to state management in navigators and screens.

## Problem Statement

React Native applications require a consistent, maintainable approach to managing UI state, user interactions, and component lifecycle across multiple screens and navigation contexts. Without a standardized pattern, components may use inconsistent state management approaches, leading to unpredictable behavior, difficult debugging, and increased cognitive load for developers working across different parts of the codebase.

## Decision

1. MUST: All React Native functional components managing local UI state MUST use the useState hook for state declarations.

## Policy Block

- MUST All React Native functional components managing local UI state MUST use the useState hook for state declarations.

In scope:
- All React Native screen components (Chat, Login, Registration, etc.)
- Navigation components (AuthStackNavigator, RootStackNavigator)
- Custom hooks for shared stateful logic (useAuth)
- Components using react-native-paper UI elements
- Components managing form inputs, loading states, or user feedback

Out of scope:
- Redux store configuration and slice definitions
- Pure presentational components with no internal state
- Third-party library components
- Native module implementations
- Legacy class components scheduled for refactoring

Exceptions:
- EXC-001: Integrating third-party libraries that require class-based components or legacy lifecycle methods
- EXC-002: Performance-critical components where hooks introduce measurable overhead (must be profiled)

## Rationale

- Evidence shows consistent usage of React hooks (useState, useCallback, useEffect, useMemo, useSelector) across 7 files with 84.76% confidence, indicating an established pattern in the codebase.
- Functional components with hooks provide better code reusability through custom hooks (e.g., useAuth) and reduce boilerplate compared to class-based components with HOCs.
- The pattern aligns with React and React Native best practices as of React 16.8+, ensuring compatibility with modern tooling, documentation, and community support.
- Hooks enable better separation of concerns by allowing stateful logic to be extracted into custom hooks, as demonstrated by the useAuth hook that encapsulates authentication state and API interactions.

## Consequences

Positive:
- Consistent state management patterns across all UI components reduce cognitive load and improve developer productivity when navigating the codebase.
- Custom hooks like useAuth enable reusable stateful logic that can be shared across multiple components without prop drilling or HOC nesting.
- Functional components with hooks are more concise and easier to test than class-based components, reducing maintenance burden.
- Better integration with modern React tooling (React DevTools, Fast Refresh) and TypeScript type inference for hook-based components.

Negative:
- Developers unfamiliar with React hooks face a learning curve understanding hook rules (e.g., dependency arrays, closure behavior).
- Incorrect dependency arrays in useEffect or useCallback can lead to subtle bugs such as stale closures or infinite render loops.
- Mixing hooks with legacy class components during migration creates temporary inconsistency in the codebase.
- Complex hook compositions may become difficult to debug without proper tooling or understanding of React's rendering model.

## Alternatives

- Continue using class-based components with lifecycle methods and connect HOC for Redux (rejected)
  Rejected because: Class components require more boilerplate, are harder to test, and do not align with modern React best practices. Evidence shows the codebase has already adopted hooks across multiple components.
  When valid: Only for maintaining existing legacy components until they can be refactored
- Use external state management libraries (MobX, Zustand) instead of React hooks and Redux (rejected)
  Rejected because: The codebase already has Redux integrated with useSelector hooks. Introducing another state management paradigm would create inconsistency and require significant refactoring.
  When valid: For new greenfield projects where Redux has not been adopted
- Mix class components and functional components based on developer preference (rejected)
  Rejected because: Inconsistent patterns increase cognitive load, make code reviews harder, and complicate onboarding. Evidence shows a clear preference for hooks across the codebase.
  When valid: Never; consistency is critical for maintainability

## Risks

- Developers may misuse hooks by omitting dependencies in useEffect/useCallback, leading to stale closures and hard-to-debug runtime errors.
  Mitigation: Enable eslint-plugin-react-hooks with exhaustive-deps rule enforced in CI. Provide team training on hook dependency management and common pitfalls.
  Owner: Engineering team lead
- Complex custom hooks may become difficult to test and maintain if they encapsulate too much logic or have unclear responsibilities.
  Mitigation: Establish guidelines for custom hook design: single responsibility, clear input/output contracts, and comprehensive unit tests. Review custom hooks in code review.
  Owner: Engineering team
- Performance issues may arise from excessive re-renders if useState and useEffect are not optimized with useCallback and useMemo.
  Mitigation: Use React DevTools Profiler to identify performance bottlenecks. Apply memoization strategically only where profiling shows benefit. Document performance considerations in component comments.
  Owner: Engineering team

## Implementation Notes

- Use TypeScript with React hooks to leverage type inference for state and props, reducing runtime errors and improving IDE support.
- Follow the Rules of Hooks: only call hooks at the top level of functional components, never inside loops, conditions, or nested functions.
- When creating custom hooks, prefix the function name with 'use' (e.g., useAuth, useForm) to signal hook usage and enable linter checks.
- For complex state logic with multiple sub-values or interdependent state transitions, consider using useReducer instead of multiple useState calls.
- Document custom hooks with JSDoc comments explaining parameters, return values, and side effects to improve discoverability and maintainability.

## Continuation Context


Verify commands:
- grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src --include="*.tsx" --include="*.ts" | wc -l
- grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" | wc -l
- npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'

Accept when:
- All new React Native functional components use hooks (useState, useEffect, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.

## Enforcement

- Verified by: ESLint with eslint-plugin-react-hooks enforced in CI pipeline
- Verified by: Code review checklist requiring hooks usage verification for new components
- Verified by: TypeScript type checking ensuring correct hook usage patterns
- Violation handling: CI build fails if eslint-plugin-react-hooks rules are violated
- Violation handling: Code review blocks merge if new class components are introduced without documented exception
- Violation handling: Automated PR comments flag missing dependencies in useEffect/useCallback for reviewer attention
- Exception process: Developer documents exception rationale in component file with TODO comment and tracking issue
- Exception process: Tech lead reviews exception request with consideration for migration path and timeline
- Exception process: Exception is recorded in architecture decision log with expiration date for re-evaluation