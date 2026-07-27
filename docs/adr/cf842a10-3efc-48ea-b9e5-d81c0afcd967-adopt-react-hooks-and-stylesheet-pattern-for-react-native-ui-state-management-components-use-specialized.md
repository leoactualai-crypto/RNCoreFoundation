# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Components Use Specialized

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all React Native UI components and screens within the template application.

## Context

- The codebase uses React Native as the mobile application framework, requiring a consistent approach to UI state management and styling across screens including Chat, Login, Registration, and navigation components.
- React hooks (useState, useCallback, useEffect, useMemo, useSelector) are detected across 7 files as the primary mechanism for managing component state and side effects, replacing class-based component patterns.
- StyleSheet.create() is consistently used across screen components to define component-specific styles with typed style objects, providing performance optimization through style sheet registration.
- The application integrates react-native-paper for Material Design components, react-navigation for routing, react-redux for global state, and react-native-gifted-chat for specialized chat UI, requiring coordination between multiple UI interaction paradigms.
- Authentication flows use custom hooks (useAuth) that combine React hooks with external service boundaries (axios HTTP clients) and secure storage, demonstrating the pattern extends beyond pure UI concerns to cross-cutting application logic.

## Problem Statement

React Native applications require a standardized approach to managing UI interaction state, side effects, and styling that balances developer ergonomics, performance, type safety, and consistency across screens while integrating with third-party UI libraries and global state management solutions.

## Decision

1. MAY: Components MAY use specialized third-party interaction libraries (e.g., react-native-gifted-chat) for domain-specific UI patterns when they provide significant functionality beyond basic components.

## Policy Block

- MAY Components MAY use specialized third-party interaction libraries (e.g., react-native-gifted-chat) for domain-specific UI patterns when they provide significant functionality beyond basic components.

In scope:
- All React Native screen components in template/src/screens/
- All React Native custom hooks in template/src/components/
- Navigation components using @react-navigation/stack
- Components integrating with react-redux global state
- Authentication and authorization UI flows

Out of scope:
- Non-React Native codebases or web-specific React applications
- Native iOS/Android modules written in Swift/Kotlin/Java
- Backend API services and server-side rendering logic
- Build-time code generation or static site generation
- Test utilities and mock components that intentionally violate patterns for testing purposes

Exceptions:
- EXC-001: Third-party library components require class-based component integration or do not support hooks API
- EXC-002: Performance profiling demonstrates that inline styles provide measurable benefit for highly dynamic styling scenarios

## Rationale

- Evidence shows consistent adoption of React hooks across 7 files with 84.76% confidence, indicating this is an established pattern rather than experimental usage, with hooks appearing in screens (Chat.tsx, Login.tsx, Registration.tsx), navigators (AuthStackNavigator.tsx), custom hooks (useAuth.ts), and application root (App.tsx, index.tsx).
- StyleSheet.create() appears in every screen component with structured style definitions (container, title, button, textinput patterns), demonstrating deliberate performance optimization through style sheet registration and providing type safety for style properties.
- The pattern integrates multiple UI libraries (react-native-paper, react-navigation, react-redux, react-native-gifted-chat) through hooks-based APIs, showing the approach successfully coordinates diverse interaction paradigms within a unified component model.
- Custom hooks like useAuth demonstrate the pattern extends beyond UI state to cross-cutting concerns (authentication, HTTP clients, secure storage), providing a consistent abstraction for stateful logic reuse across the application architecture.

## Consequences

Positive:
- Consistent state management pattern across all UI components reduces cognitive load and enables developers to quickly understand component behavior through familiar hooks API.
- StyleSheet.create() provides performance optimization by registering styles once and referencing them by ID, reducing memory overhead and enabling native-side style caching.
- Hooks enable fine-grained composition of stateful logic through custom hooks, improving code reuse without wrapper hell from higher-order components or render props patterns.
- Type safety for hooks and StyleSheet definitions provides compile-time validation of state updates and style properties, catching errors before runtime.

Negative:
- Hooks introduce rules of hooks constraints (must be called at top level, must be called in same order) that can be violated accidentally, causing runtime errors that are difficult to debug.
- StyleSheet.create() requires all styles to be defined statically, making dynamic theming and runtime style composition more complex compared to inline style objects.
- Multiple state management paradigms (local useState, global useSelector, custom hooks) create potential confusion about where state should live and how to coordinate updates across boundaries.
- Deep integration with react-native-paper and other UI libraries creates vendor lock-in, making migration to alternative component libraries or design systems costly.

## Alternatives

- Use class-based React components with lifecycle methods (componentDidMount, componentDidUpdate, etc.) for state management (rejected)
  Rejected because: Class-based components require more boilerplate, make stateful logic reuse difficult without higher-order components or render props, and are no longer the recommended pattern in React ecosystem as of React 16.8+
  When valid: When integrating with legacy third-party libraries that only provide class-based APIs or when maintaining existing class-based codebases
- Use inline style objects or styled-components library for component styling instead of StyleSheet.create() (rejected)
  Rejected because: Inline styles create new style objects on every render causing performance overhead, and styled-components adds additional runtime dependency and CSS-in-JS parsing overhead not optimized for React Native's bridge architecture
  When valid: For highly dynamic styles that change on every render based on props, or when sharing styling logic with React web applications using styled-components
- Use MobX or Zustand for global state management instead of react-redux with useSelector (deferred)
  Rejected because: Not rejected; evidence shows react-redux is currently adopted but alternative state management libraries may be evaluated in future
  When valid: When redux boilerplate becomes excessive for application complexity, or when simpler state management with less ceremony is preferred

## Risks

- Developers unfamiliar with hooks rules may violate hooks constraints (conditional calls, calls in loops) causing subtle runtime bugs that are difficult to diagnose
  Mitigation: Enable eslint-plugin-react-hooks with rules of hooks enforcement, provide team training on hooks patterns, and include hooks usage guidelines in code review checklist
  Owner: Frontend team lead
- Over-reliance on useEffect for side effects may create complex dependency arrays and infinite render loops when dependencies are incorrectly specified
  Mitigation: Establish patterns for common side effect scenarios (data fetching, subscriptions), use exhaustive-deps ESLint rule, and prefer custom hooks that encapsulate complex effect logic
  Owner: Engineering team
- StyleSheet.create() static style definitions may not accommodate dynamic theming requirements as application grows, requiring refactoring to support runtime theme switching
  Mitigation: Establish theme context provider pattern early, use theme-aware style functions that accept theme parameters, and document theming approach in style guide
  Owner: UI/UX team and frontend team

## Implementation Notes

- Use TypeScript interfaces to define component props and state shapes, enabling type inference for useState and other hooks to catch type errors at compile time.
- Extract custom hooks for any stateful logic used in more than one component, following the useXxx naming convention and documenting hook parameters and return values.
- Organize StyleSheet.create() definitions at the bottom of component files with descriptive names (container, title, button, textinput) that reflect semantic purpose rather than visual properties.
- When using useEffect, always specify complete dependency arrays and use ESLint exhaustive-deps rule to validate dependencies; prefer useCallback and useMemo to stabilize dependencies.
- For components accessing global state, use useSelector with selector functions that extract only the specific state slice needed to minimize re-renders when unrelated state changes.
- Integrate react-native-safe-area-context for proper safe area handling on iOS devices, as evidenced in Chat.tsx usage of SafeAreaView.

## Continuation Context


Verify commands:
- grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l
- grep -r "StyleSheet.create" template/src/screens/ --include="*.tsx" | wc -l
- grep -r "class.*extends.*Component" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l
- npx eslint template/src/ --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn' --format json

Accept when:
- All screen components in template/src/screens/ use React hooks (useState, useEffect, etc.) with zero class-based components detected
- All screen components define styles using StyleSheet.create() with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow useXxx naming convention and are extracted to reusable modules in template/src/components/ or template/src/hooks/

## Enforcement

- Verified by: ESLint with eslint-plugin-react-hooks enforcing rules-of-hooks and exhaustive-deps rules in CI pipeline
- Verified by: Code review checklist requiring verification of hooks usage patterns and StyleSheet.create() for new components
- Verified by: TypeScript compilation enforcing type safety for hooks and style definitions
- Verified by: Automated grep-based verification in CI checking for class-based component patterns and inline style objects
- Violation handling: CI build fails if ESLint hooks rules report errors, blocking merge until violations are resolved
- Violation handling: Code review requires explicit justification and exception approval for any class-based components or inline styles
- Violation handling: TypeScript compilation errors for incorrect hook usage or style definitions must be resolved before deployment
- Violation handling: Quarterly architecture review audits component patterns and identifies technical debt for refactoring
- Exception process: Developer documents technical constraint requiring exception (e.g., third-party library limitation) in ADR exception log
- Exception process: Frontend team lead reviews exception request with architecture justification and approves or requests alternative approach
- Exception process: Approved exceptions are documented in component file with TODO comment linking to exception ID and future remediation plan
- Exception process: Exception log is reviewed quarterly to identify patterns requiring ADR updates or library migrations