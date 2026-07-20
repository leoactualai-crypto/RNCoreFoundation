# Adopt Redux Toolkit with Thunk Middleware for State Management with Environment-Aware Configuration: Store Modules Export

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The application requires centralized state management with asynchronous action handling capabilities, as evidenced by the integration of @reduxjs/toolkit and redux-thunk in template/src/store/index.ts
- Runtime configuration must adapt to different deployment environments (development, production, test) through process.env.NODE_ENV access patterns
- The store exports typed dispatch (AppDispatch) and thunk (AppThunk) contracts, indicating a TypeScript-based architecture requiring type-safe state management patterns
- The rootReducer composition pattern suggests a modular state structure where multiple domain-specific reducers are combined into a single store

## Problem Statement

Applications require a consistent approach to state management that supports asynchronous operations, environment-specific configuration, and type safety while maintaining modularity across feature domains. Without standardized patterns for Redux store configuration and environment variable access, teams may implement inconsistent state management solutions that complicate testing, deployment, and cross-environment behavior.

## Decision

1. MUST: Store modules MUST export typed AppDispatch and AppThunk contracts for type-safe dispatch operations

## Policy Block

- MUST Store modules MUST export typed AppDispatch and AppThunk contracts for type-safe dispatch operations

## Rationale

- The evidence shows explicit usage of @reduxjs/toolkit and redux-thunk in template/src/store/index.ts, indicating a deliberate architectural choice for Redux Toolkit's simplified API and built-in thunk support
- The detection of process.env.NODE_ENV access patterns demonstrates environment-aware configuration requirements, enabling different store behaviors across development, testing, and production contexts
- The export of AppDispatch and AppThunk type contracts indicates a TypeScript-first architecture where type safety is enforced at compile time for all state operations
- The rootReducer composition pattern provides modularity and separation of concerns, allowing independent feature domains to manage their own state slices while maintaining a unified store

## Consequences

Positive:
- Standardized Redux Toolkit usage reduces boilerplate code and provides opinionated defaults for common Redux patterns including immutable updates and action creators
- Type-safe dispatch contracts (AppDispatch, AppThunk) prevent runtime errors by catching type mismatches at compile time
- Environment-aware configuration through NODE_ENV enables optimized builds and debugging tools in development while maintaining production performance
- Modular rootReducer composition supports independent feature development and testing without coupling between state domains

Negative:
- Redux Toolkit abstractions may obscure underlying Redux mechanics, potentially increasing learning curve for developers unfamiliar with the toolkit's conventions
- Dependency on process.env.NODE_ENV creates coupling to Node.js runtime environment variables, requiring build-time substitution for browser deployments
- Thunk middleware adds runtime overhead for all dispatched actions even when synchronous operations would suffice
- Centralized store configuration in a single file may become a bottleneck for large applications with many feature domains

## Alternatives

- Use plain Redux with manual action creators and reducer composition without Redux Toolkit abstractions (rejected)
  Rejected because: Plain Redux requires significantly more boilerplate code and lacks built-in TypeScript support, increasing maintenance burden and error potential
  When valid: Valid for applications requiring maximum control over Redux internals or with existing large Redux codebases predating Redux Toolkit
- Adopt Redux Saga for side effect management instead of redux-thunk (rejected)
  Rejected because: Redux Saga introduces additional complexity with generator functions and effects API, while the evidence shows thunk middleware already meets asynchronous operation requirements
  When valid: Valid for applications with complex async workflows requiring cancellation, debouncing, or sophisticated orchestration patterns
- Use React Context API with useReducer hooks for state management without Redux (rejected)
  Rejected because: Context API lacks middleware support, time-travel debugging, and the structured patterns that Redux Toolkit provides for large-scale state management
  When valid: Valid for small applications with simple state requirements and minimal cross-component state sharing

## Risks

- Environment variable misconfiguration (incorrect NODE_ENV values) could cause production builds to include development-only debugging tools or vice versa
  Mitigation: Implement build-time validation of NODE_ENV values and automated tests verifying environment-specific behavior in each deployment context
  Owner: engineering team
- Type contract drift between AppDispatch/AppThunk exports and actual store implementation could lead to runtime type mismatches despite TypeScript compilation
  Mitigation: Enforce strict TypeScript configuration with noImplicitAny and strictNullChecks, and implement integration tests validating dispatch type contracts
  Owner: engineering team
- RootReducer composition complexity may grow unbounded as feature domains proliferate, creating performance bottlenecks and maintenance challenges
  Mitigation: Establish reducer splitting guidelines, implement code splitting for lazy-loaded feature reducers, and monitor store performance metrics
  Owner: engineering team

## Implementation Notes

- Store initialization should occur in template/src/store/index.ts following the detected pattern, with configureStore from @reduxjs/toolkit and middleware configuration including thunk
- Export store instance, RootState type (derived from store.getState), AppDispatch type (derived from store.dispatch), and AppThunk type for typed thunk actions
- Access process.env.NODE_ENV only during store configuration for environment-specific middleware or DevTools setup, not within reducer logic
- Organize reducers by feature domain in separate files, then compose them in rootReducer using combineReducers or Redux Toolkit's slice pattern

## Continuation Context


Verify commands:
- grep -r "@reduxjs/toolkit" template/src/store/ && grep -r "redux-thunk" template/src/store/
- grep -r "process.env.NODE_ENV" template/src/store/index.ts
- grep -E "export.*(AppDispatch|AppThunk|store)" template/src/store/index.ts
- grep -r "rootReducer" template/src/store/

Accept when:
- All store configuration files import and use @reduxjs/toolkit's configureStore and redux-thunk middleware
- Store initialization accesses process.env.NODE_ENV for environment-aware configuration
- Store module exports include typed AppDispatch, AppThunk, and store contracts
- RootReducer composition pattern aggregates domain-specific reducers into unified store

## Enforcement

- Verified by: Static analysis scanning for @reduxjs/toolkit and redux-thunk imports in store configuration files
- Verified by: TypeScript compilation enforcing AppDispatch and AppThunk type contract usage
- Verified by: Code review checklist verifying rootReducer composition and environment variable access patterns
- Verified by: Automated tests validating store behavior across different NODE_ENV values
- Violation handling: CI pipeline fails if store configuration does not import required Redux Toolkit dependencies
- Violation handling: TypeScript compilation errors block merges when dispatch operations lack proper type annotations
- Violation handling: Code review flags direct Redux usage without Redux Toolkit abstractions for refactoring
- Violation handling: Runtime warnings in development mode when environment variables are accessed outside store initialization
- Exception process: Document technical justification for alternative state management approach in ADR format
- Exception process: Obtain architecture review approval for exceptions requiring different Redux patterns or libraries
- Exception process: Maintain exception registry tracking approved deviations with expiration dates and migration plans