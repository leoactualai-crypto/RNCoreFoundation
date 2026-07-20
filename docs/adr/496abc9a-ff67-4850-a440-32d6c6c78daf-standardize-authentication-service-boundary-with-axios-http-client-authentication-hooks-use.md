# Standardize Authentication Service Boundary with Axios HTTP Client: Authentication Hooks Use

Status: proposed
Date: 2025-01-20
Deciders: Detection Pipeline (automated)

## Context

- The application uses React with @reduxjs/toolkit for state management and requires authentication capabilities across multiple user flows including login and registration
- Authentication operations are performed against a remote HTTP API endpoint at BASE_URL/auth/local with distinct paths for login and registration
- The useAuth custom hook encapsulates authentication logic using React hooks (useMemo, useEffect) and axios for HTTP communication, providing a single public contract for authentication operations
- Credentials are managed using react-native-secure-storage, indicating a mobile or cross-platform React Native application context
- The service boundary is defined by axios.post calls to specific authentication endpoints with structured request payloads containing identifier/email and password fields

## Problem Statement

Authentication operations require a consistent service boundary definition that isolates HTTP client concerns, standardizes request/response contracts, and provides a stable interface for UI components while maintaining flexibility to evolve the underlying authentication service implementation.

## Decision

1. SHOULD: Authentication hooks SHOULD use React useMemo and useEffect for optimized state management and side effect handling

## Policy Block

- SHOULD Authentication hooks SHOULD use React useMemo and useEffect for optimized state management and side effect handling

In scope:
- All authentication operations (login, registration) in React/React Native applications
- Custom hooks that expose authentication functionality to UI components
- HTTP client configuration for authentication service endpoints
- Request payload structure for authentication API contracts

Out of scope:
- Authorization and permission checking logic beyond initial authentication
- Session management and token refresh mechanisms
- Password validation rules and complexity requirements
- Server-side authentication service implementation details

## Rationale

- The pattern establishes a clear service boundary using axios as the HTTP client, providing consistent error handling, request/response transformation, and interceptor capabilities across authentication operations
- Encapsulating authentication logic within a custom useAuth hook creates a stable public API contract that isolates UI components from service implementation details and enables independent evolution of both layers
- The evidence shows repeated axios.post patterns to /auth/local and /auth/local/register endpoints with structured payloads, indicating an established convention that should be formalized to prevent divergence
- Integration with React hooks (useMemo, useEffect) and @reduxjs/toolkit demonstrates alignment with modern React patterns for state management and side effects, supporting maintainability and developer familiarity

## Consequences

Positive:
- Clear service boundary definition enables independent testing of authentication logic without UI component dependencies
- Standardized axios usage provides consistent error handling, request/response transformation, and interceptor capabilities across all authentication operations
- Custom hook pattern (useAuth) creates a stable public API that UI components can depend on, reducing coupling to service implementation details
- Externalized configuration (BASE_URL) supports multiple deployment environments without code changes

Negative:
- Tight coupling to axios as the HTTP client library makes migration to alternative HTTP clients (fetch, ky, etc.) more costly
- Custom hook encapsulation adds an abstraction layer that may obscure authentication flow details during debugging
- Hardcoded endpoint paths (/auth/local, /auth/local/register) in service boundary definitions require code changes if API versioning or path conventions evolve
- React Native specific dependencies (react-native-secure-storage) limit portability to web-only React applications without adaptation

## Alternatives

- Use native fetch API instead of axios for authentication service boundary (rejected)
  Rejected because: Axios provides superior error handling, request/response interceptors, and automatic JSON transformation that would require manual implementation with fetch, increasing maintenance burden
  When valid: When minimizing bundle size is critical and authentication requirements are simple enough that fetch's limited feature set is sufficient
- Implement authentication service boundary using GraphQL mutations instead of REST endpoints (rejected)
  Rejected because: Evidence shows established REST API patterns with /auth/local endpoints; migrating to GraphQL would require coordinated backend changes and does not address the immediate service boundary definition need
  When valid: When the backend authentication service adopts GraphQL and the application requires more flexible query capabilities across multiple resources
- Expose raw axios instances to UI components without custom hook encapsulation (rejected)
  Rejected because: Direct axios usage in components creates tight coupling to HTTP implementation details, makes testing more difficult, and prevents centralized authentication logic evolution
  When valid: For prototype or proof-of-concept code where abstraction overhead outweighs maintainability benefits

## Risks

- Axios version updates may introduce breaking changes to request/response handling that affect authentication service boundary behavior
  Mitigation: Pin axios to specific minor versions, maintain comprehensive integration tests for authentication flows, and review axios changelog before upgrades
  Owner: engineering team
- Hardcoded endpoint paths in service boundary definitions become outdated if backend API versioning or path conventions change
  Mitigation: Externalize endpoint path configuration alongside BASE_URL, implement API version negotiation headers, and establish backend API deprecation policies with advance notice
  Owner: engineering team
- Custom hook abstraction may hide authentication errors or network failures from UI components, degrading user experience
  Mitigation: Implement explicit error state management in useAuth hook return values, provide detailed error types for different failure modes, and ensure error boundaries capture authentication failures
  Owner: engineering team

## Implementation Notes

- Create a centralized authentication service module that exports typed axios instances configured with BASE_URL and common headers/interceptors
- Define TypeScript interfaces for authentication request payloads (LoginRequest, RegisterRequest) and response types to enforce contract consistency
- Implement error handling within the useAuth hook that maps axios errors to domain-specific authentication error types (InvalidCredentials, NetworkError, ServerError)
- Add integration tests that mock axios responses to verify useAuth hook behavior across success and failure scenarios without requiring live backend services

## Continuation Context


Verify commands:
- grep -r "axios\.post.*auth/local" template/src/components/AuthHook/
- grep -r "useAuth" template/src/components/ | grep -c "export"
- grep -r "BASE_URL" template/src/config/ | grep -v node_modules

Accept when:
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code

## Enforcement

- Verified by: Code review checklist requiring authentication changes to use useAuth hook and axios for service boundaries
- Verified by: Static analysis rules detecting direct authentication endpoint calls outside of designated service boundary modules
- Verified by: Integration test suite validating authentication flows through useAuth hook contract
- Violation handling: CI pipeline fails if grep patterns detect authentication endpoint calls outside of AuthHook module
- Violation handling: Pull requests introducing authentication logic bypass are flagged for architectural review
- Violation handling: Quarterly codebase audits identify and remediate authentication service boundary violations
- Exception process: Document technical justification for alternative authentication service boundary approach in ADR format
- Exception process: Obtain approval from tech lead and architecture review board before merging exception
- Exception process: Tag exception code with comments referencing approved ADR and expiration/review date