# Standardize Axios-Based External Authentication Client Boundaries: Authentication Credentials Stored

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The React application uses a custom authentication hook (useAuth) that directly manages external API communication for user authentication flows including login and registration
- Authentication operations are implemented using axios HTTP client to communicate with a remote authentication service at BASE_URL/auth endpoints
- The authentication boundary is encapsulated within a React hook that combines UI interaction patterns (useMemo, useEffect) with external service calls, creating a coupling between presentation and service layers
- State management is handled through @reduxjs/toolkit while secure credential storage uses react-native-secure-storage, indicating a mobile-first or cross-platform architecture
- The pattern demonstrates a direct client-to-service boundary without intermediate abstraction layers or API gateways

## Problem Statement

React components require a consistent, maintainable approach to communicate with external authentication services while managing credentials securely and coordinating UI state, but direct HTTP client usage within hooks can lead to scattered service boundaries, inconsistent error handling, and tight coupling between presentation logic and external API contracts.

## Decision

1. SHOULD: Authentication credentials SHOULD be stored using react-native-secure-storage for persistent secure storage across sessions

## Policy Block

- SHOULD Authentication credentials SHOULD be stored using react-native-secure-storage for persistent secure storage across sessions

In scope:
- All authentication operations (login, registration, token refresh) that communicate with external authentication services
- React components and hooks that require user authentication state or credential management
- Mobile and web clients built with React or React Native that access the authentication service at BASE_URL

Out of scope:
- Server-side authentication logic or backend service implementation
- Non-authentication API boundaries (data fetching, business logic endpoints)
- Third-party OAuth or social authentication providers that may use different client libraries
- Internal service-to-service authentication that does not involve client applications

Exceptions:
- EXC-001: Legacy authentication flows that predate this pattern and require gradual migration
- EXC-002: Platform-specific authentication mechanisms (biometric, device-native) that cannot use axios

## Rationale

- The evidence shows consistent use of axios for external authentication boundaries across login and registration flows, establishing it as the de facto HTTP client standard for this service boundary
- Encapsulating external client calls within the useAuth hook provides a stable public contract that isolates consuming components from changes to the authentication service API or HTTP implementation
- Integration with @reduxjs/toolkit and react-native-secure-storage demonstrates a coordinated approach to state management and credential persistence that should be maintained for consistency
- The pattern of combining UI lifecycle hooks (useMemo, useEffect) with external service calls reflects the React ecosystem's standard approach to managing asynchronous operations within component lifecycles

## Consequences

Positive:
- Consistent HTTP client usage (axios) across authentication boundaries simplifies debugging, error handling, and request/response interceptor configuration
- Centralized authentication logic in the useAuth hook reduces code duplication and provides a single point of maintenance for authentication service integration
- Integration with @reduxjs/toolkit enables predictable state management and facilitates testing of authentication flows
- Secure credential storage through react-native-secure-storage protects sensitive user data across application sessions

Negative:
- Tight coupling between the useAuth hook and axios creates a dependency that would require significant refactoring if the HTTP client needs to change
- Combining UI lifecycle management with external service calls in a single hook increases complexity and may make unit testing more difficult
- Direct service calls from the client layer without an intermediate API gateway or abstraction layer exposes the client to changes in the authentication service API contract
- The pattern may not scale well if authentication requirements become more complex (multiple auth providers, advanced token management, offline support)

## Alternatives

- Introduce a dedicated authentication service abstraction layer that separates HTTP client implementation from the useAuth hook (rejected)
  Rejected because: The current evidence shows direct axios usage within the hook, and introducing an abstraction layer would require refactoring without clear evidence of multiple HTTP client requirements or complex service orchestration needs
  When valid: Valid when multiple authentication providers are required, when the authentication service API changes frequently, or when the same authentication logic needs to support multiple HTTP client implementations
- Use native fetch API instead of axios to reduce external dependencies (rejected)
  Rejected because: The evidence demonstrates established axios usage with likely configured interceptors, error handling, and request/response transformations that would be lost with native fetch
  When valid: Valid for new projects with minimal HTTP client requirements, when bundle size is critical, or when targeting environments with excellent native fetch support
- Implement authentication logic in Redux middleware or thunks instead of React hooks (deferred)
  Rejected because: The current pattern uses hooks for UI lifecycle coordination, but moving to Redux middleware could improve separation of concerns and testability
  When valid: Valid when authentication logic needs to be triggered outside of component lifecycle, when complex authentication orchestration is required, or when authentication state management becomes more sophisticated

## Risks

- Changes to the external authentication service API contract (endpoint paths, request/response schemas) will require updates across all components using the useAuth hook
  Mitigation: Implement comprehensive integration tests for authentication flows, use TypeScript interfaces to define API contracts, and consider versioning the authentication API endpoints
  Owner: Engineering team and API platform team
- Direct axios usage in hooks without proper error handling or retry logic could result in poor user experience during network failures or service outages
  Mitigation: Implement axios interceptors for consistent error handling, add retry logic with exponential backoff, and provide clear user feedback for authentication failures
  Owner: Frontend engineering team
- Credential storage using react-native-secure-storage may have platform-specific limitations or security vulnerabilities that are not immediately apparent
  Mitigation: Conduct security review of credential storage implementation, implement credential rotation policies, and monitor for security advisories related to react-native-secure-storage
  Owner: Security team and mobile engineering team

## Implementation Notes

- Configure axios interceptors at application initialization to handle authentication tokens, request/response logging, and consistent error handling across all authentication API calls
- Define TypeScript interfaces for authentication request and response payloads to ensure type safety and document the API contract between client and service
- Implement comprehensive error handling in the useAuth hook to distinguish between network errors, authentication failures, and service errors, providing appropriate user feedback for each case
- Consider extracting the axios configuration and BASE_URL management into a separate configuration module to facilitate environment-specific configuration and testing

## Continuation Context


Verify commands:
- grep -r "axios.post.*auth/local" template/src/components/AuthHook/ | wc -l
- grep -r "import.*axios" template/src/components/AuthHook/useAuth.ts
- grep -r "react-native-secure-storage" template/src/components/AuthHook/useAuth.ts

Accept when:
- All authentication operations (login, registration) use axios.post with BASE_URL configuration and standardized endpoint paths
- The useAuth hook exports a public contract that abstracts axios implementation details from consuming components
- Credential storage integration with react-native-secure-storage is present in the authentication hook implementation

## Enforcement

- Verified by: Code review process verifies that new authentication operations follow the axios-based pattern and use the useAuth hook
- Verified by: Static analysis tools (ESLint rules) detect direct authentication API calls outside of the designated hook
- Verified by: Integration tests validate that authentication flows use the correct HTTP client and service endpoints
- Violation handling: Pull requests that introduce authentication logic outside the useAuth hook or use alternative HTTP clients are flagged during code review
- Violation handling: CI pipeline fails if static analysis detects authentication API calls that bypass the established pattern
- Violation handling: Architecture review is required for any proposed changes to the authentication client boundary pattern
- Exception process: Submit an exception request documenting the specific authentication requirement that cannot be met by the current pattern
- Exception process: Security team and architecture team review the exception request to assess security implications and architectural impact
- Exception process: Approved exceptions must be documented in the authentication service documentation with justification and any compensating controls