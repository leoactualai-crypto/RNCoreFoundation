# Adopt Axios for Direct External Authentication API Calls in React Hooks: Registration Requests Send

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase uses React with Redux Toolkit for state management and requires authentication capabilities for mobile and web applications
- Authentication operations (login, register) need to communicate with an external Strapi backend API at runtime
- The useAuth hook encapsulates authentication logic and must handle asynchronous HTTP requests with credential payloads
- React Native Secure Storage is used for credential persistence, requiring coordination between storage, HTTP client, and React lifecycle hooks
- The authentication flow involves multiple endpoints (/auth/local for login, /auth/local/register for registration) that share similar request patterns

## Problem Statement

React components and hooks require a consistent, promise-based HTTP client to communicate with external authentication APIs, handle request/response serialization, manage errors, and integrate with React lifecycle methods (useEffect, useMemo) without introducing excessive boilerplate or coupling to specific backend implementations.

## Decision

1. MUST: Registration requests MUST send username, email, and password fields in the request body

## Policy Block

- MUST Registration requests MUST send username, email, and password fields in the request body

In scope:
- Authentication operations (login, registration) in React and React Native applications
- Custom hooks that expose authentication contracts (useAuth)
- HTTP POST requests to /auth/local and /auth/local/register endpoints
- Integration with Redux Toolkit state management

Out of scope:
- Non-authentication API calls (data fetching, mutations outside auth domain)
- Server-side API implementations
- WebSocket or real-time communication patterns
- File upload or multipart form data requests

Exceptions:
- EXC-001: Alternative HTTP clients (fetch, ky, etc.) may be used if axios introduces bundle size constraints in production builds

## Rationale

- Evidence shows axios is explicitly imported and used for three distinct authentication POST operations in useAuth.ts, establishing it as the standard HTTP client
- The pattern of axios.post with BASE_URL concatenation and structured request bodies appears consistently across login and registration flows, indicating intentional standardization
- Integration with React hooks (useMemo, useEffect) and Redux Toolkit requires a promise-based client with predictable error handling, which axios provides
- The boundaries.external_clients facet captures direct HTTP calls to external services, and the evidence shows axios mediating all authentication boundary crossings

## Consequences

Positive:
- Consistent HTTP client interface across authentication operations reduces cognitive load and maintenance burden
- Promise-based API integrates naturally with React hooks and async/await patterns
- Axios provides built-in request/response interceptors for future cross-cutting concerns (logging, token refresh)
- Centralized BASE_URL configuration enables environment-specific endpoint management

Negative:
- Introduces axios as a required dependency, increasing bundle size by ~13KB (minified + gzipped)
- Tight coupling to axios API may complicate future migration to alternative HTTP clients
- Direct axios calls in hooks bypass potential API abstraction layers, reducing flexibility for mocking or testing
- Multiple identical axios.post calls suggest potential code duplication that could be refactored

## Alternatives

- Use native fetch API for authentication requests (rejected)
  Rejected because: Fetch requires more boilerplate for error handling, request/response transformation, and lacks built-in interceptor support needed for token management
  When valid: Valid for projects with strict bundle size constraints where axios overhead is prohibitive
- Create an abstracted API client service layer wrapping axios (deferred)
  Rejected because: Not rejected, but evidence shows direct axios usage; abstraction layer may be future refactoring
  When valid: Valid when multiple API clients or complex request/response transformations are needed
- Use Redux Toolkit Query (RTK Query) for authentication API calls (rejected)
  Rejected because: Evidence shows Redux Toolkit is present but authentication uses direct axios calls, suggesting RTK Query was not adopted for this use case
  When valid: Valid for projects requiring comprehensive data fetching, caching, and synchronization beyond authentication

## Risks

- Code duplication across multiple axios.post calls with similar structure may lead to inconsistent error handling or request formatting
  Mitigation: Refactor to extract common authentication request logic into reusable functions or create an authentication API service module
  Owner: Frontend engineering team
- Direct BASE_URL concatenation in multiple locations creates maintenance burden if endpoint structure changes
  Mitigation: Centralize endpoint definitions in a constants file or API configuration module with typed endpoint builders
  Owner: Frontend engineering team
- Axios dependency version drift or security vulnerabilities require coordinated updates across authentication flows
  Mitigation: Implement automated dependency scanning in CI/CD pipeline and maintain axios version in shared package.json
  Owner: DevOps and security teams

## Implementation Notes

- Ensure axios is declared in package.json dependencies with a specific version constraint (e.g., ^1.6.0)
- Configure axios defaults (timeout, headers) in a centralized config module imported by authentication hooks
- Implement error handling wrappers around axios calls to normalize error responses from authentication endpoints
- Consider extracting repeated axios.post patterns into a createAuthRequest helper function to reduce duplication
- Document the expected response structure from /auth/local and /auth/local/register endpoints for type safety

## Continuation Context


Verify commands:
- grep -r "axios.post.*auth/local" template/src/components/AuthHook/
- grep -r "import.*axios" template/src/ | grep -v node_modules
- npm list axios 2>/dev/null || echo 'axios not found in dependencies'

Accept when:
- All authentication POST requests in useAuth.ts use axios.post with BASE_URL-prefixed endpoints
- Axios is declared as a runtime dependency in package.json
- Authentication requests include required fields (identifier/password for login, username/email/password for registration)

## Enforcement

- Verified by: Code review checklist verifying axios usage in authentication hooks
- Verified by: Static analysis with ESLint rules enforcing consistent HTTP client usage
- Verified by: Integration tests validating authentication request structure and axios configuration
- Violation handling: Pull requests introducing alternative HTTP clients in authentication flows require architectural review
- Violation handling: Violations flagged during code review must be refactored before merge
- Violation handling: Existing violations should be tracked as technical debt items with prioritized remediation
- Exception process: Request exception via architecture review board with documented justification
- Exception process: Provide performance analysis or bundle size impact assessment for alternative approaches
- Exception process: Document approved exceptions in ADR amendments with expiration dates for re-evaluation