# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Http Requests Authentication

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase implements authentication functionality using React hooks pattern with state management through Redux Toolkit
- Authentication operations require secure credential storage using react-native-secure-storage for mobile platform compatibility
- HTTP communication with backend authentication endpoints is handled through axios client library
- The useAuth hook serves as the public contract for authentication operations, exposing login, logout, and registration capabilities
- UI interactions are optimized using React's useMemo and useEffect hooks to manage side effects and memoization

## Problem Statement

Authentication logic needs to be encapsulated in a reusable, testable component that integrates with external authentication services while maintaining secure credential storage and providing a consistent interface for UI components across the application.

## Decision

1. MUST: HTTP requests to authentication endpoints MUST use axios library with POST method to BASE_URL/auth/local for login and BASE_URL/auth/local/register for registration

## Policy Block

- MUST HTTP requests to authentication endpoints MUST use axios library with POST method to BASE_URL/auth/local for login and BASE_URL/auth/local/register for registration

In scope:
- Authentication hooks in template/src/components/AuthHook/
- Login and registration operations using axios HTTP client
- Secure storage operations for credentials
- Redux Toolkit state management for authentication state

Out of scope:
- Non-authentication API calls
- Server-side authentication logic
- OAuth or third-party authentication providers
- Session management beyond initial authentication

## Rationale

- The pattern leverages React's hooks ecosystem to provide a declarative, composable authentication interface that integrates naturally with React component lifecycle
- Redux Toolkit provides predictable state management with reduced boilerplate compared to vanilla Redux, suitable for managing authentication state across the application
- Axios offers a promise-based HTTP client with interceptor support, enabling centralized error handling and request/response transformation for authentication flows
- React-native-secure-storage provides platform-specific secure storage mechanisms, essential for protecting sensitive authentication credentials on mobile devices

## Consequences

Positive:
- Reusable authentication logic encapsulated in a custom hook reduces code duplication across components
- Type-safe state management through Redux Toolkit improves maintainability and reduces runtime errors
- Axios interceptors enable centralized authentication token injection and error handling
- Secure storage integration protects credentials using platform-native security mechanisms

Negative:
- Tight coupling to React ecosystem limits portability to non-React frameworks
- Multiple library dependencies (React, Redux Toolkit, axios, react-native-secure-storage) increase bundle size and maintenance surface
- Mobile-specific secure storage dependency may complicate web-only deployments
- Redux Toolkit adds complexity for simple authentication scenarios that might not require global state management

## Alternatives

- Use React Context API instead of Redux Toolkit for state management (rejected)
  Rejected because: Context API lacks the middleware, devtools integration, and time-travel debugging capabilities that Redux Toolkit provides for complex authentication flows
  When valid: Valid for applications with simple authentication state that doesn't require advanced debugging or middleware
- Use native fetch API instead of axios for HTTP requests (rejected)
  Rejected because: Fetch API requires more boilerplate for request/response transformation and lacks built-in interceptor support needed for authentication token management
  When valid: Valid for applications prioritizing minimal dependencies over developer experience and interceptor functionality
- Implement authentication logic in class components instead of hooks (rejected)
  Rejected because: Class components have more verbose syntax and lack the composability benefits of hooks, making authentication logic harder to share across components
  When valid: Valid only for legacy codebases that have not migrated to React hooks

## Risks

- Dependency on react-native-secure-storage may cause compatibility issues with web-only deployments or future React Native versions
  Mitigation: Implement platform detection and provide fallback storage mechanisms for web environments; monitor react-native-secure-storage maintenance status and have migration plan ready
  Owner: engineering team
- Hardcoded authentication endpoint paths (/auth/local, /auth/local/register) create tight coupling to specific backend API structure
  Mitigation: Extract endpoint paths to configuration layer; implement API versioning strategy; use OpenAPI specification to generate client code
  Owner: engineering team
- Multiple library dependencies increase vulnerability surface and require ongoing security patch management
  Mitigation: Implement automated dependency scanning in CI/CD pipeline; establish regular dependency update cadence; maintain security audit log
  Owner: engineering team

## Implementation Notes

- Import React hooks (useMemo, useEffect) and Redux Toolkit utilities at the top of authentication hook modules
- Configure axios base URL through centralized config module (../../config) to enable environment-specific endpoint configuration
- Structure authentication requests with consistent payload format: identifier/email and password for login, username/email/password for registration
- Wrap authentication state selectors in useMemo to prevent unnecessary re-renders when authentication state hasn't changed
- Use useEffect for side effects such as persisting tokens to secure storage after successful authentication

## Continuation Context


Verify commands:
- grep -r "import.*react.*from 'react'" template/src/components/AuthHook/
- grep -r "@reduxjs/toolkit" template/src/components/AuthHook/
- grep -r "axios.post.*auth/local" template/src/components/AuthHook/
- grep -r "react-native-secure-storage" template/src/components/AuthHook/
- grep -r "export.*useAuth" template/src/components/AuthHook/

Accept when:
- All authentication hook files import React and use hooks pattern (useMemo, useEffect)
- Redux Toolkit is imported and used for state management in authentication modules
- Axios is used for POST requests to /auth/local and /auth/local/register endpoints
- React-native-secure-storage is imported for credential storage
- useAuth hook is exported as the public API contract

## Enforcement

- Verified by: Static analysis tools scanning for required imports (React, Redux Toolkit, axios, react-native-secure-storage)
- Verified by: Code review checklist verifying authentication hook structure and public API contracts
- Verified by: Integration tests validating authentication flow with mocked axios requests
- Verified by: Dependency analysis ensuring all required libraries are present in package.json
- Violation handling: CI pipeline fails if required dependencies are missing from authentication modules
- Violation handling: Code review blocks merge if authentication hooks don't follow the established pattern
- Violation handling: Linting rules flag non-compliant authentication implementations
- Violation handling: Architecture review required for any deviation from the standard authentication stack
- Exception process: Document technical justification for alternative authentication implementation approach
- Exception process: Obtain approval from tech lead and security team for deviations affecting credential storage or HTTP client
- Exception process: Create ADR documenting the exception rationale and scope
- Exception process: Add exception to architecture decision log with expiration date for review