# Adopt React Hooks for State and Effect Management in UI Components: Components Use Useeffect

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase uses React and React Native for building cross-platform mobile applications with shared component logic
- UI components require local state management for user interactions, form inputs, and dynamic rendering without introducing heavy state management libraries for component-level concerns
- React Hooks (useState, useEffect, useCallback, useMemo) provide a functional programming model for managing component lifecycle, side effects, and memoization
- The pattern appears consistently across 6 files including App.tsx, authentication hooks, navigation components, and screen components (Chat, Login)
- Integration with external libraries (react-redux, react-native-paper, react-native-gifted-chat) requires hook-based composition patterns for accessing context and selectors

## Problem Statement

UI components in React Native applications need a consistent, composable mechanism for managing local state, side effects, memoized computations, and callback stability without relying on class-based lifecycle methods or introducing unnecessary complexity for component-level interactions.

## Decision

1. MUST: Components MUST use useEffect for side effects including data fetching, subscriptions, and DOM/native module interactions with proper cleanup functions

## Policy Block

- MUST Components MUST use useEffect for side effects including data fetching, subscriptions, and DOM/native module interactions with proper cleanup functions

In scope:
- All functional React and React Native components
- Custom hooks for authentication, navigation, and shared UI logic
- Screen components (Login, Chat, etc.) managing user interactions
- Navigation components requiring state or context access
- Components integrating with third-party libraries (react-redux, react-native-paper)

Out of scope:
- Class-based React components (legacy code only)
- Pure presentational components with no state or effects
- Global state management logic (handled by Redux/toolkit)
- Native module implementations outside React component tree

Exceptions:
- EXC-001: Legacy class components that have not yet been migrated to functional components with hooks

## Rationale

- Evidence shows consistent use of useState, useEffect, useCallback, and useMemo across 6 files with 84.72% confidence, indicating an established pattern
- React Hooks enable functional component composition without class-based complexity, improving code readability and reducing boilerplate
- Custom hooks like useAuth demonstrate successful extraction of reusable stateful logic for authentication flows across multiple components
- Integration with react-redux (useSelector) and other libraries shows hooks provide a unified composition model for accessing external state and context

## Consequences

Positive:
- Consistent functional programming model across all UI components reduces cognitive load and improves maintainability
- Custom hooks enable reusable stateful logic without render props or higher-order components, improving code organization
- Better performance optimization through useCallback and useMemo prevents unnecessary re-renders in component trees
- Simplified testing as hooks can be tested independently and components remain pure functions

Negative:
- Developers must understand React's Rules of Hooks and dependency arrays to avoid subtle bugs with stale closures
- Overuse of useMemo and useCallback can lead to premature optimization and increased complexity without measurable performance gains
- Migration of existing class components to hooks requires refactoring effort and careful testing of lifecycle behavior
- Debugging hook dependencies and effect execution order can be challenging without proper tooling and experience

## Alternatives

- Continue using class-based components with lifecycle methods (componentDidMount, componentDidUpdate, etc.) (rejected)
  Rejected because: Class components require more boilerplate, make logic reuse difficult without HOCs or render props, and are not the recommended approach in modern React
  When valid: Only for legacy components not yet migrated or third-party library constraints
- Use external state management (Redux, MobX) for all component state including local UI state (rejected)
  Rejected because: Introduces unnecessary complexity and global state pollution for transient UI interactions that should remain component-local
  When valid: Only for truly global application state that needs to be shared across multiple screens or persisted
- Mix class components and functional components with hooks based on developer preference (rejected)
  Rejected because: Creates inconsistent codebase with multiple patterns for the same concerns, increasing maintenance burden and onboarding complexity
  When valid: During migration period with clear timeline to converge on hooks-based approach

## Risks

- Incorrect dependency arrays in useEffect and useMemo can cause infinite loops, stale closures, or missing updates
  Mitigation: Enable eslint-plugin-react-hooks with exhaustive-deps rule, conduct code reviews focused on hook dependencies, provide team training on closure behavior
  Owner: Engineering team
- Premature optimization with useCallback and useMemo adds complexity without measurable performance improvement
  Mitigation: Profile components before optimization, establish guidelines for when memoization is warranted (e.g., expensive computations, large lists), document performance rationale
  Owner: Engineering team
- Custom hooks may become overly complex or tightly coupled to specific components, reducing reusability
  Mitigation: Review custom hooks for single responsibility, ensure proper abstraction boundaries, write unit tests for custom hooks independently
  Owner: Engineering team

## Implementation Notes

- Install and configure eslint-plugin-react-hooks with 'react-hooks/rules-of-hooks': 'error' and 'react-hooks/exhaustive-deps': 'warn' in ESLint configuration
- Create custom hooks in dedicated files (e.g., useAuth.ts) with clear naming convention (use* prefix) and export them for reuse across components
- Document dependency arrays in useEffect and useMemo with inline comments explaining why each dependency is included or intentionally omitted
- Use React DevTools Profiler to identify unnecessary re-renders before applying useCallback or useMemo optimizations
- For complex state logic with multiple sub-values or interdependent updates, consider useReducer as an alternative to multiple useState calls

## Continuation Context


Verify commands:
- grep -r "useState\|useEffect\|useCallback\|useMemo" template/src --include="*.tsx" --include="*.ts" | wc -l
- eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
- grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" || echo 'No class components found'

Accept when:
- All functional components use hooks (useState, useEffect, etc.) for state and effects with no violations of Rules of Hooks
- ESLint validation passes with react-hooks plugin rules enabled showing no errors for hooks usage
- Custom hooks follow naming conventions (use* prefix) and are properly extracted for reusable stateful logic
- No new class-based components are introduced except with documented exception approval

## Enforcement

- Verified by: ESLint pre-commit hooks with react-hooks/rules-of-hooks and react-hooks/exhaustive-deps rules enabled
- Verified by: Code review checklist verifying proper hook usage, dependency arrays, and custom hook abstractions
- Verified by: CI pipeline running ESLint validation on all TypeScript/TSX files with hooks-specific rules
- Violation handling: ESLint errors for Rules of Hooks violations block CI pipeline and prevent merge
- Violation handling: Missing or incorrect dependency arrays trigger warnings requiring reviewer acknowledgment and justification
- Violation handling: New class components without exception approval are rejected in code review
- Exception process: Developer documents exception rationale in PR description with specific technical justification
- Exception process: Tech lead reviews exception request and approves only for legacy migration or third-party constraints
- Exception process: Approved exceptions are documented in component file header with migration plan and timeline