# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Screen Specific Styles

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native applications require navigation between screens with type-safe parameter passing to prevent runtime errors and improve developer experience
- The codebase uses @react-navigation/native and @react-navigation/stack for navigation management across 5 files with consistent patterns
- Public API contracts (RootStackParamList, AuthStackNavigator, LoginScreen, ChatScreen) define navigation boundaries and screen component interfaces
- Authentication flows require coordination between navigation state, Redux state management, and secure storage via react-native-secure-storage
- UI interaction patterns consistently use React hooks (useState, useEffect, useMemo, useCallback, useSelector) for state management and side effects

## Problem Statement

React Native applications need a standardized approach to define navigation structure, screen parameters, and component contracts that ensures type safety across navigation boundaries while maintaining consistency with authentication state and UI interaction patterns. Without explicit type contracts, navigation parameter mismatches and screen interface violations occur at runtime rather than compile time.

## Decision

1. SHOULD: Screen-specific styles SHOULD be defined using StyleSheet.create with typed style objects

## Policy Block

- SHOULD Screen-specific styles SHOULD be defined using StyleSheet.create with typed style objects

In scope:
- All React Native screen components that participate in navigation stacks
- Navigation stack definitions and route parameter type declarations
- Custom hooks that manage authentication or navigation state
- Screen component exports that serve as public API contracts

Out of scope:
- Non-navigational UI components (buttons, inputs, cards) that do not define screen-level contracts
- Utility functions and helpers that do not interact with navigation state
- Third-party library components used within screens but not exported as contracts
- Backend API contracts and service definitions (covered by boundaries.service_definitions)

## Rationale

- Evidence shows consistent use of TypeScript navigation types (RootStackParamList) and exported screen contracts (LoginScreen, ChatScreen, AuthStackNavigator) across 5 files with 85.66% confidence
- The pattern coordinates navigation structure with authentication state (useAuth hook, Redux useSelector) and secure storage, requiring explicit contracts to maintain type safety across these boundaries
- React Navigation's TypeScript support enables compile-time verification of navigation parameters when proper type contracts are defined, reducing runtime navigation errors
- Standardizing on named exports for screen components creates clear public API boundaries that support refactoring and testing

## Consequences

Positive:
- Compile-time type checking prevents navigation parameter mismatches and screen prop errors
- Explicit screen component exports create clear public API boundaries for testing and documentation
- TypeScript navigation types enable IDE autocomplete and refactoring support for navigation calls
- Consistent hook-based state management patterns improve code readability and maintainability

Negative:
- Requires TypeScript configuration and type maintenance overhead for navigation parameter lists
- Developers must learn React Navigation's TypeScript patterns and type declaration conventions
- Refactoring screen parameters requires updates to both type definitions and component implementations
- Additional boilerplate for navigation type definitions increases initial setup complexity

## Alternatives

- Use untyped navigation with any or implicit types, relying on runtime validation (rejected)
  Rejected because: Runtime-only validation misses navigation parameter errors until execution, increasing debugging time and production risk. Evidence shows the codebase already uses TypeScript types (RootStackParamList), indicating preference for compile-time safety.
  When valid: Prototyping or proof-of-concept projects where type safety is not a priority
- Use React Router or alternative navigation libraries instead of React Navigation (rejected)
  Rejected because: Evidence explicitly shows @react-navigation/native and @react-navigation/stack in use across multiple files. React Router lacks native mobile navigation primitives (stack, tab, drawer) required for React Native.
  When valid: Web-only React applications or when migrating from an existing React Router codebase
- Define screen contracts using PropTypes instead of TypeScript interfaces (rejected)
  Rejected because: PropTypes provide only runtime validation without IDE support or compile-time checking. Evidence shows TypeScript usage (RootStackParamList type), indicating TypeScript is already adopted.
  When valid: Legacy JavaScript codebases without TypeScript migration path

## Risks

- Navigation type definitions may drift out of sync with actual screen implementations, causing type errors or incorrect assumptions
  Mitigation: Implement automated tests that verify navigation flows and parameter passing. Use strict TypeScript configuration to catch type mismatches at compile time.
  Owner: engineering team
- Complex nested navigation structures may create deeply nested type definitions that are difficult to maintain
  Mitigation: Limit navigation stack depth and use composition patterns. Document navigation architecture and provide examples for common patterns.
  Owner: engineering team
- Third-party navigation library updates may introduce breaking changes to type definitions or navigation APIs
  Mitigation: Pin React Navigation versions and test upgrades in isolated branches. Monitor React Navigation changelog and migration guides.
  Owner: engineering team

## Implementation Notes

- Define a central navigation types file (e.g., types/navigation.ts) that exports all stack parameter lists (RootStackParamList, AuthStackParamList) for reuse across navigators and screens
- Use React Navigation's TypeScript guide to properly type useNavigation and useRoute hooks with stack-specific parameter lists
- Export screen components as named exports (not default) to create explicit public API contracts that are easier to track and refactor
- Integrate navigation type checking into CI pipeline using tsc --noEmit to catch type errors before merge
- Document navigation structure and parameter contracts in component JSDoc comments or separate architecture documentation

## Continuation Context


Verify commands:
- grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l
- grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json
- find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l
- npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation

Accept when:
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

## Enforcement

- Verified by: TypeScript compiler checks during CI build pipeline (tsc --noEmit)
- Verified by: Code review verification that new screens define parameter types and export named contracts
- Verified by: Automated linting rules that enforce named exports for screen components
- Violation handling: CI build fails if TypeScript compilation detects navigation type errors
- Violation handling: Pull requests are blocked if new screens lack proper type definitions or named exports
- Violation handling: Navigation parameter type mismatches are flagged as high-priority bugs requiring immediate fix
- Exception process: Exceptions for untyped navigation may be granted for rapid prototyping branches that will not merge to main
- Exception process: Technical debt tickets must be created for any temporary type suppressions (e.g., @ts-ignore) in navigation code
- Exception process: Architecture review required for alternative navigation patterns that deviate from React Navigation