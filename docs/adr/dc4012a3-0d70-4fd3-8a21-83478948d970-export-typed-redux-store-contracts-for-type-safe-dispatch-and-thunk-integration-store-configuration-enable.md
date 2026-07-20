# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Store Configuration Enable

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The store configuration in template/src/store/index.ts integrates @reduxjs/toolkit and redux-thunk for state management with asynchronous action support
- Type-safe dispatch and thunk patterns require exported TypeScript contracts (AppDispatch, AppThunk, store) to enable compile-time verification across the application
- Environment-aware store configuration reads process.env.NODE_ENV to conditionally enable Redux DevTools and middleware in development versus production
- The rootReducer import establishes a centralized reducer composition pattern that requires consistent typing at the store boundary

## Problem Statement

Without exported type contracts for the Redux store, dispatch operations, and thunk creators, application code cannot leverage TypeScript's type system to verify action creators, async thunks, and state selectors at compile time, leading to runtime errors and reduced developer productivity when integrating state management across components and middleware.

## Decision

1. MAY: Store configuration MAY enable Redux DevTools extension conditionally based on NODE_ENV for development debugging

## Policy Block

- MAY Store configuration MAY enable Redux DevTools extension conditionally based on NODE_ENV for development debugging

In scope:
- Redux store configuration and initialization in template/src/store/index.ts
- Exported type contracts: AppDispatch, AppThunk, store instance
- Environment-based middleware and DevTools configuration
- Integration with @reduxjs/toolkit and redux-thunk

Out of scope:
- Individual reducer implementations within rootReducer
- Component-level useDispatch and useSelector hook usage
- Action creator implementations in feature slices
- State shape and normalization strategies

## Rationale

- Exporting AppDispatch and AppThunk contracts enables TypeScript to verify dispatch calls and thunk creators at compile time, preventing type mismatches that would only surface at runtime
- Environment-aware configuration through process.env.NODE_ENV allows optimized production builds while preserving development tooling, balancing debuggability with performance
- Centralizing store configuration with typed exports establishes a single source of truth for state management contracts, reducing integration errors across the codebase
- The pattern observed in template/src/store/index.ts demonstrates a mature Redux Toolkit integration with 87.50% confidence based on explicit exports and environment handling

## Consequences

Positive:
- Type-safe dispatch operations catch action type mismatches at compile time rather than runtime
- Consistent thunk typing enables IDE autocomplete and refactoring support for async actions
- Environment-aware configuration optimizes production bundles while preserving development debugging capabilities
- Exported store contracts enable testability through typed mock stores and dispatch functions

Negative:
- Requires TypeScript knowledge and discipline to properly utilize exported type contracts
- Additional boilerplate for type definitions increases initial setup complexity
- Environment variable dependency (NODE_ENV) creates implicit runtime configuration that may differ between environments if not properly managed
- Type contracts must be manually updated if store configuration changes significantly

## Alternatives

- Use untyped Redux store without exported contracts, relying on runtime validation only (rejected)
  Rejected because: Eliminates compile-time type safety, increasing runtime errors and reducing developer productivity in TypeScript codebases
  When valid: Valid only in pure JavaScript projects without TypeScript
- Generate type contracts automatically using code generation tools from store configuration (deferred)
  Rejected because: Adds build complexity and tooling dependencies; manual exports provide sufficient type safety for current scale
  When valid: Valid for large-scale applications with frequently changing store configurations requiring automated type synchronization
- Use Context API with useReducer instead of Redux for state management (rejected)
  Rejected because: Does not provide middleware support for async actions (redux-thunk) or the DevTools integration already established in the codebase
  When valid: Valid for simpler applications without complex async state management requirements

## Risks

- Type contracts may drift from actual store implementation if not maintained during refactoring
  Mitigation: Derive types from store instance using typeof rather than manual type definitions; enforce type checking in CI pipeline
  Owner: engineering team
- Environment variable (NODE_ENV) misconfiguration could enable development tools in production, impacting performance
  Mitigation: Validate NODE_ENV at build time; use strict environment checks; document required environment variables
  Owner: engineering team
- Redux Toolkit and redux-thunk version updates may introduce breaking changes to type contracts
  Mitigation: Pin major versions in package.json; test type contracts after dependency updates; maintain upgrade documentation
  Owner: engineering team

## Implementation Notes

- Export AppDispatch as 'typeof store.dispatch' to automatically synchronize with store configuration changes
- Define AppThunk as 'ThunkAction<ReturnType, RootState, unknown, Action<string>>' to support parameterized async actions
- Use configureStore from @reduxjs/toolkit with devTools option set to 'process.env.NODE_ENV !== "production"'
- Import and integrate rootReducer to maintain centralized reducer composition
- Document exported contracts in store module comments to guide consumer usage patterns

## Continuation Context


Verify commands:
- grep -r "export.*AppDispatch" template/src/store/index.ts
- grep -r "export.*AppThunk" template/src/store/index.ts
- grep -r "process\.env\.NODE_ENV" template/src/store/index.ts
- grep -r "@reduxjs/toolkit" template/src/store/index.ts

Accept when:
- Store module exports AppDispatch, AppThunk, and store instance with TypeScript type annotations
- Store configuration reads process.env.NODE_ENV for environment-aware middleware setup
- Integration with @reduxjs/toolkit configureStore and redux-thunk is present in store configuration
- Type contracts are derived from store instance rather than manually defined where possible

## Enforcement

- Verified by: TypeScript compiler type checking in CI pipeline
- Verified by: Automated grep patterns verifying exported contracts in store module
- Verified by: Code review checklist ensuring type contracts are exported and properly typed
- Verified by: Unit tests verifying store configuration behavior across NODE_ENV values
- Violation handling: CI build fails if TypeScript compilation errors occur due to missing or incorrect type contracts
- Violation handling: Pull requests blocked if store module does not export required contracts (AppDispatch, AppThunk, store)
- Violation handling: Linting rules flag usage of untyped dispatch or thunk patterns
- Violation handling: Documentation updated to reflect current store contract requirements
- Exception process: Exception requests must document why typed contracts cannot be used for specific use case
- Exception process: Architecture review required for any store configuration that bypasses standard type contracts
- Exception process: Temporary exceptions must include migration plan and timeline to adopt standard pattern
- Exception process: All exceptions logged in ADR amendments with justification and expiration date