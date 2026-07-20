# Use process.env for Runtime Configuration in Redux Store Setup: Store Configuration Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The Redux store configuration in template/src/store/index.ts accesses runtime configuration through process.env.NODE_ENV
- The @reduxjs/toolkit and redux-thunk libraries are used as the core state management infrastructure
- Environment-based configuration determines store behavior such as middleware enablement and development tooling
- The store exports typed interfaces (AppDispatch, AppThunk) as public API contracts for application-wide state management
- Runtime configuration sources are read directly from the process environment rather than through abstraction layers

## Problem Statement

Redux store initialization requires environment-aware configuration to enable appropriate middleware and development tools, but direct access to process.env creates coupling between store setup and runtime environment variables that may contain sensitive configuration data.

## Decision

1. MAY: Store configuration MAY use process.env for feature flags and non-sensitive runtime switches

## Policy Block

- MAY Store configuration MAY use process.env for feature flags and non-sensitive runtime switches

In scope:
- Redux store initialization and configuration
- Middleware setup based on NODE_ENV
- Development tooling enablement (Redux DevTools, logging)
- Runtime mode detection for state management behavior

Out of scope:
- API keys, tokens, or authentication credentials
- Database connection strings or service URLs
- Encryption keys or signing secrets
- Third-party service credentials

Exceptions:
- EXC-001: Local development environment requires quick prototyping with mock credentials

## Rationale

- The evidence shows process.env.NODE_ENV is accessed in store/index.ts for runtime configuration, establishing a pattern of environment-based store behavior
- Using @reduxjs/toolkit with redux-thunk requires environment-aware middleware configuration to enable development tools without impacting production performance
- Direct process.env access in store setup creates a boundary where security-sensitive configuration must be carefully controlled
- The pattern of exporting typed interfaces (AppDispatch, AppThunk) indicates this is a foundational module accessed throughout the application

## Consequences

Positive:
- Clear separation between development and production store behavior through NODE_ENV detection
- Enables conditional middleware and tooling without runtime overhead in production
- Typed exports (AppDispatch, AppThunk) provide type-safe state management contracts
- Centralized store configuration makes environment-dependent behavior predictable

Negative:
- Direct process.env access creates potential for accidental exposure of sensitive environment variables
- Tight coupling between store initialization and Node.js process environment limits portability
- No abstraction layer for configuration makes testing and mocking more difficult
- Pattern may encourage other modules to access process.env directly without security review

## Alternatives

- Use a dedicated configuration module that validates and exposes only safe environment variables (rejected)
  Rejected because: Evidence shows direct process.env access is the established pattern in the codebase
  When valid: When refactoring for improved testability and security boundaries
- Inject configuration as parameters to store creation function (rejected)
  Rejected because: Would require changes to store initialization pattern across the application
  When valid: For new applications or major refactoring efforts prioritizing dependency injection
- Use build-time environment variable substitution instead of runtime access (rejected)
  Rejected because: NODE_ENV needs runtime detection for dynamic middleware configuration
  When valid: For static configuration that does not change between development and production builds

## Risks

- Developers may add access to sensitive environment variables in store configuration without security review
  Mitigation: Implement code review checklist for process.env access; use linting rules to flag new process.env usage
  Owner: Engineering team
- Store configuration may inadvertently log or expose environment variables in development tools
  Mitigation: Audit Redux DevTools configuration to ensure environment variables are not serialized; implement sanitization for logged state
  Owner: Security team
- Testing becomes difficult when store behavior depends on global process.env state
  Mitigation: Use test utilities to mock process.env; document testing patterns for environment-dependent store behavior
  Owner: Engineering team

## Implementation Notes

- Limit process.env access in store configuration to NODE_ENV and explicitly approved non-sensitive variables
- Document all environment variables accessed in store setup with security classification (safe/sensitive)
- Use TypeScript const assertions or enums for valid NODE_ENV values to prevent typos
- Consider wrapping process.env.NODE_ENV in a getter function to facilitate testing and future refactoring

## Continuation Context


Verify commands:
- grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV
- grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/
- npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts

Accept when:
- Store configuration only accesses process.env.NODE_ENV and no other environment variables
- No API keys, tokens, or credentials are read from process.env in store initialization
- Linting rules flag any new process.env access in store configuration files

## Enforcement

- Verified by: Code review checklist for changes to store configuration
- Verified by: Static analysis with ESLint rules restricting process.env access
- Verified by: Security audit of environment variable usage in state management layer
- Violation handling: Pull request blocked until process.env access is justified and approved
- Violation handling: Security team notified for review if sensitive variables are accessed
- Violation handling: Refactoring required to move sensitive configuration to dedicated config module
- Exception process: Submit exception request with justification to tech lead
- Exception process: Security team review required for any sensitive environment variable access
- Exception process: Document exception in code with expiration date and migration plan