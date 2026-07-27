# Standardize React Hooks for UI State Management in React Native Screens: Screen Components Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native application screens consistently use React hooks (useState, useCallback, useMemo, useEffect, useSelector) for managing component state and side effects across 7 files
- The codebase integrates react-native-paper for UI components, @react-navigation/stack for navigation, react-redux for global state, and react-native-gifted-chat for chat functionality
- Screen components (Chat.tsx, Login.tsx, Registration.tsx) follow a pattern of local state management with useState combined with StyleSheet.create for styling
- Authentication logic is centralized in a custom useAuth hook that coordinates axios HTTP calls, react-native-secure-storage for token persistence, and Redux Toolkit for state management
- The application structure separates concerns between screen components, navigation stacks, and reusable hooks while maintaining consistent interaction patterns

## Problem Statement

React Native applications require a consistent approach to managing UI state, side effects, and user interactions across multiple screen components. Without standardized patterns for state management hooks, teams risk inconsistent data flow, duplicated logic, and difficulty maintaining component behavior as the application scales.

## Decision

1. MUST: Screen components MUST use React hooks (useState, useCallback, useMemo, useEffect) for managing local component state and side effects

## Policy Block

- MUST Screen components MUST use React hooks (useState, useCallback, useMemo, useEffect) for managing local component state and side effects

In scope:
- React Native screen components in the /screens directory
- Custom hooks in the /components or /hooks directories
- Navigation components using @react-navigation/stack
- Components requiring global state access via react-redux

Out of scope:
- Pure utility functions without UI concerns
- Redux reducers and action creators
- Native module implementations
- Third-party library internals

Exceptions:
- EXC-001: Legacy class components that have not yet been migrated to functional components with hooks

## Rationale

- The pattern emerges from 7 files with 84.76% confidence, demonstrating consistent adoption of React hooks for UI state management across Chat, Login, Registration screens and navigation components
- React hooks provide a functional programming model that simplifies state management, reduces boilerplate compared to class components, and enables better code reuse through custom hooks like useAuth
- Centralizing authentication logic in useAuth demonstrates the value of custom hooks for coordinating multiple concerns (axios HTTP calls, react-native-secure-storage, Redux state) in a single reusable interface
- The consistent use of StyleSheet.create alongside hooks indicates an established pattern for co-locating component logic and styling definitions

## Consequences

Positive:
- Consistent state management patterns across screen components improve code readability and reduce onboarding time for new developers
- Custom hooks like useAuth enable reusable logic that can be tested independently and shared across multiple components
- React hooks integrate seamlessly with react-redux (useSelector) and react-navigation, creating a cohesive ecosystem for React Native development
- Functional components with hooks generally produce smaller bundle sizes and better performance than equivalent class components

Negative:
- Teams must understand React hooks lifecycle and rules (no conditional hooks, dependency arrays) to avoid subtle bugs
- Overuse of useMemo and useCallback can lead to premature optimization and increased code complexity without measurable performance gains
- Custom hooks that coordinate multiple side effects (HTTP, storage, state) can become difficult to test and debug if not properly decomposed
- Migration from existing class components to hooks requires refactoring effort and potential introduction of regressions

## Alternatives

- Continue using React class components with lifecycle methods (componentDidMount, componentDidUpdate, setState) (rejected)
  Rejected because: Class components require more boilerplate, have less intuitive lifecycle management, and are not the recommended approach in modern React documentation. The evidence shows zero class components in the detected files.
  When valid: Only for legacy components that have not yet been migrated, with explicit migration plan
- Use MobX or other observable-based state management instead of Redux with hooks (rejected)
  Rejected because: The codebase has already standardized on react-redux with useSelector as evidenced in AuthStackNavigator.tsx. Introducing a second state management paradigm would create inconsistency.
  When valid: For new projects without existing Redux infrastructure, or when observable patterns better match domain requirements
- Implement state management using React Context API exclusively without Redux (rejected)
  Rejected because: Evidence shows AuthContext is used alongside Redux, indicating both are needed. Context API alone lacks Redux DevTools, middleware support, and time-travel debugging capabilities.
  When valid: For simple applications with minimal global state requirements and no need for advanced debugging tools

## Risks

- Developers unfamiliar with React hooks may violate Rules of Hooks (conditional calls, incorrect dependency arrays) leading to runtime errors or infinite loops
  Mitigation: Enable eslint-plugin-react-hooks with exhaustive-deps rule, provide team training on hooks patterns, and enforce code review checklist for hook usage
  Owner: Engineering team lead
- Custom hooks that coordinate multiple side effects (useAuth with axios, storage, Redux) may become difficult to test and maintain as complexity grows
  Mitigation: Establish testing patterns for custom hooks using @testing-library/react-hooks, limit each custom hook to a single responsibility, and document hook contracts with TypeScript interfaces
  Owner: Engineering team
- Performance issues may arise from unnecessary re-renders if useState and useEffect are not properly optimized with useMemo and useCallback
  Mitigation: Use React DevTools Profiler to identify performance bottlenecks, apply memoization only when profiling shows measurable benefit, and establish performance budgets for screen render times
  Owner: Engineering team

## Implementation Notes

- Install and configure eslint-plugin-react-hooks to enforce Rules of Hooks at lint time, preventing common mistakes like conditional hook calls or missing dependencies
- Create a /hooks directory for shared custom hooks and document each hook's purpose, parameters, and return values using JSDoc or TypeScript interfaces
- For custom hooks performing async operations (HTTP, storage), follow the pattern established in useAuth.ts: return loading state, error state, and data/methods in a consistent object structure
- When migrating class components to hooks, start with simple components using only useState, then progressively adopt useEffect, useCallback, and useMemo as needed based on profiling data

## Continuation Context


Verify commands:
- grep -r "useState\|useCallback\|useMemo\|useEffect\|useSelector" template/src/screens/ --include="*.tsx" --include="*.ts"
- grep -r "class.*extends.*Component" template/src/screens/ --include="*.tsx" --include="*.ts" | wc -l
- npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'

Accept when:
- All screen components in /screens directory use React hooks (useState, useEffect, etc.) with no class components present
- ESLint reports zero violations of react-hooks/rules-of-hooks and react-hooks/exhaustive-deps rules
- Custom hooks like useAuth are present in dedicated hooks directory and follow consistent return value patterns (loading, error, data/methods)

## Enforcement

- Verified by: ESLint with eslint-plugin-react-hooks enforced in CI pipeline
- Verified by: Code review checklist requiring verification of hook usage patterns and dependency arrays
- Verified by: Automated tests for custom hooks using @testing-library/react-hooks
- Violation handling: CI build fails if eslint-plugin-react-hooks reports errors
- Violation handling: Code review blocks merge if hooks are used conditionally or with incorrect dependencies
- Violation handling: Performance regression tests fail if screen render times exceed established budgets
- Exception process: Legacy class components require technical lead approval with documented migration plan and target date
- Exception process: Exceptions to hook patterns require architecture review and must be documented in component file header
- Exception process: Performance optimizations bypassing standard patterns require profiling data demonstrating measurable improvement