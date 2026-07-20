# Define Public API Contracts for Authentication-Aware React Native Navigation: Navigation Type Contracts

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The application uses React Native with @react-navigation/native and @react-navigation/stack for navigation management
- Authentication state is managed through a useAuth() hook pattern, indicating centralized authentication context
- The RootStackParamList type contract defines the navigation structure and parameter types for the application's routing
- The App component serves as the root entry point and coordinates authentication state with navigation configuration
- StyleSheet-based styling patterns indicate a React Native mobile application architecture with standardized visual components

## Problem Statement

React Native applications integrating authentication with navigation require explicit type contracts to ensure type-safe routing, prevent runtime navigation errors, and maintain clear boundaries between authenticated and unauthenticated navigation flows. Without formalized public API contracts like RootStackParamList, navigation parameter mismatches and authentication state inconsistencies can propagate through the component tree.

## Decision

1. SHOULD: Navigation type contracts SHOULD be exported as public API contracts to enable type-safe navigation calls from any component in the application

## Policy Block

- SHOULD Navigation type contracts SHOULD be exported as public API contracts to enable type-safe navigation calls from any component in the application

In scope:
- React Native mobile applications using @react-navigation/native and @react-navigation/stack
- Components that require authentication state or perform navigation operations
- Root-level App components that initialize navigation and authentication context
- Type definitions for navigation parameters and route configurations

Out of scope:
- Web-based React applications not using React Native
- Applications using alternative navigation libraries (React Router, etc.)
- Server-side authentication logic or API authentication endpoints
- Third-party authentication provider SDKs or OAuth flows

Exceptions:
- EXC-001: Legacy screens or components are being incrementally migrated to the typed navigation system

## Rationale

- The evidence shows explicit use of RootStackParamList and App as public API contracts, indicating a deliberate architectural choice to expose typed navigation interfaces
- The useAuth() hook pattern demonstrates centralized authentication state management, which is essential for coordinating authentication checks across navigation boundaries
- React Navigation's stack navigator requires type contracts to provide compile-time safety for navigation operations, preventing runtime errors from invalid route parameters
- The StyleSheet.create pattern with standardized Colors tokens indicates a mature component architecture that benefits from consistent public API contracts

## Consequences

Positive:
- Type-safe navigation operations prevent runtime errors from invalid route names or parameter mismatches
- Centralized authentication state through useAuth() enables consistent authentication checks across all navigation flows
- Public API contracts (RootStackParamList, App) provide clear integration points for feature development and testing
- Standardized styling patterns with Colors tokens improve visual consistency and maintainability

Negative:
- Type contracts require maintenance overhead when adding or modifying navigation routes
- Centralized authentication hook creates a single point of failure if the authentication context is not properly initialized
- Public API contracts increase coupling between navigation structure and consuming components
- StyleSheet definitions can become verbose and repetitive across multiple components

## Alternatives

- Use untyped navigation without RootStackParamList contracts (rejected)
  Rejected because: Eliminates compile-time type safety, leading to runtime navigation errors and parameter mismatches that are difficult to debug in production
  When valid: Only acceptable for prototype or proof-of-concept applications with minimal navigation complexity
- Implement authentication checks directly in individual screen components without centralized useAuth() hook (rejected)
  Rejected because: Creates inconsistent authentication logic across screens, increases code duplication, and makes it difficult to update authentication behavior globally
  When valid: May be acceptable for isolated screens with unique authentication requirements that differ significantly from the standard flow
- Use React Router instead of React Navigation for routing (rejected)
  Rejected because: React Router is designed for web applications and lacks native mobile navigation patterns (stack, tab, drawer) that React Navigation provides for React Native
  When valid: Valid for React web applications or React Native Web projects targeting browser environments

## Risks

- Type contract drift where RootStackParamList becomes outdated as navigation routes are added or modified, leading to type safety gaps
  Mitigation: Implement CI checks that verify all navigation calls reference routes defined in RootStackParamList, and enforce code review for navigation changes
  Owner: Engineering team
- Authentication context initialization failures could break navigation for the entire application if useAuth() hook is not properly provided
  Mitigation: Add error boundaries around the App component and implement fallback authentication states with clear error messaging
  Owner: Engineering team
- Public API contracts may expose internal navigation structure, making it difficult to refactor navigation architecture without breaking changes
  Mitigation: Version the navigation contracts and provide migration guides when making breaking changes to RootStackParamList
  Owner: Engineering team

## Implementation Notes

- Define RootStackParamList in a dedicated types file (e.g., navigation.types.ts) and export it as a public contract for use across the application
- Implement the useAuth() hook in a context provider (e.g., AuthContext.tsx) and wrap the App component with the provider to ensure authentication state is available throughout the navigation tree
- Use TypeScript's NavigationProp and RouteProp types from @react-navigation/native with RootStackParamList to type-check navigation props in screen components
- Document the navigation structure and authentication flow in the project README, including examples of how to add new routes and integrate authentication checks

## Continuation Context


Verify commands:
- grep -r "RootStackParamList" template/src --include="*.tsx" --include="*.ts" | wc -l
- grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts"
- npx tsc --noEmit --project tsconfig.json 2>&1 | grep -i "navigation\|route" || echo "No navigation type errors"

Accept when:
- RootStackParamList type contract is defined and exported, and grep verification shows it is referenced in multiple files
- useAuth() hook is implemented and used in the App component or navigation configuration
- TypeScript compilation succeeds with no navigation-related type errors

## Enforcement

- Verified by: TypeScript compiler checks during CI/CD pipeline execution
- Verified by: Code review verification that new navigation routes are added to RootStackParamList
- Verified by: Automated linting rules that enforce useAuth() hook usage for authentication-dependent screens
- Violation handling: TypeScript compilation failures block pull request merges until navigation type errors are resolved
- Violation handling: Code review comments require updates to RootStackParamList when new routes are detected
- Violation handling: Runtime warnings in development mode when navigation calls reference undefined routes
- Exception process: Request exception through pull request description with justification for why standard navigation contracts cannot be used
- Exception process: Tech lead reviews exception request and approves only for valid cases (legacy migration, third-party library integration)
- Exception process: Document approved exceptions in ADR updates or architecture decision log with expiration timeline