# Adopt 'it' Function as Standard Test Case Declaration in React Native Projects: Projects Use Test

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native projects require a consistent testing framework approach to ensure maintainability and developer familiarity across the codebase
- The 'it' function is a widely recognized test case declaration syntax in JavaScript/TypeScript testing ecosystems, particularly with Jest which is the default test runner for React Native
- Test files in the __tests__ directory follow a naming convention (App-test.tsx) indicating a structured approach to test organization
- The detected pattern shows explicit test case declaration using 'it' with string descriptors and callback functions, establishing a clear test structure

## Problem Statement

Without a standardized test case declaration syntax, React Native projects risk inconsistent test structure, reduced readability, and increased cognitive load for developers switching between different testing styles within the same codebase.

## Decision

1. MAY: Projects MAY use 'test' as an alias for 'it' where Jest is configured to support both syntaxes

## Policy Block

- MAY Projects MAY use 'test' as an alias for 'it' where Jest is configured to support both syntaxes

In scope:
- All React Native component test files
- Unit tests for React Native modules and utilities
- Integration tests within React Native applications
- Test files using Jest as the test runner

Out of scope:
- End-to-end tests using frameworks like Detox or Appium
- Native iOS/Android test files written in Swift/Kotlin
- Performance benchmarking tests
- Manual testing procedures

## Rationale

- The evidence shows explicit usage of 'it' function syntax in template/src/__tests__/App-test.tsx, indicating this is the established pattern for test case declaration
- React Native's default testing setup with Jest provides 'it' as a first-class test declaration function, making it the natural choice for consistency with framework conventions
- The __tests__ directory structure and -test.tsx naming convention align with Jest's default configuration and React Native best practices
- Standardizing on 'it' reduces cognitive overhead and improves code readability across the testing surface

## Consequences

Positive:
- Consistent test structure across all React Native components improves maintainability and reduces onboarding time for new developers
- Alignment with Jest and React Native ecosystem conventions ensures compatibility with tooling, documentation, and community resources
- Clear test case declarations with descriptive labels improve test output readability and debugging efficiency
- Standardized file naming and directory structure enables automated test discovery and IDE integration

Negative:
- Teams familiar with alternative test declaration styles (e.g., 'test', 'describe') may require adjustment period
- Strict enforcement may require refactoring existing tests that use different declaration patterns
- Additional linting rules or CI checks needed to enforce consistency across the codebase

## Alternatives

- Use 'test' function instead of 'it' for test case declarations (rejected)
  Rejected because: While 'test' is functionally equivalent in Jest, the evidence explicitly shows 'it' usage in the template structure, indicating this is the established convention for the project
  When valid: Valid in projects where 'test' has already been adopted as the standard or where BDD-style syntax is explicitly avoided
- Allow mixed usage of both 'it' and 'test' functions based on developer preference (rejected)
  Rejected because: Mixed syntax reduces consistency and increases cognitive load when reading tests, undermining the goal of standardization
  When valid: Valid only during migration periods with a defined timeline to converge on a single standard
- Adopt describe/it nesting structure for all test files (deferred)
  Rejected because: Current evidence shows simple 'it' usage without nested describe blocks; more complex organization may be needed as test suites grow
  When valid: Valid when test files contain multiple related test cases that benefit from logical grouping under describe blocks

## Risks

- Existing test files using alternative declaration patterns may be overlooked during enforcement, creating inconsistency
  Mitigation: Implement automated linting rules (ESLint with jest plugin) to detect and flag non-compliant test declarations during development and CI
  Owner: engineering team
- Developers unfamiliar with 'it' syntax may write less descriptive test labels, reducing test clarity
  Mitigation: Provide documentation and examples of effective test descriptions; include test quality checks in code review guidelines
  Owner: engineering team
- Future framework changes or migration away from Jest could require widespread test refactoring
  Mitigation: Document the dependency on Jest; evaluate alternative frameworks for compatibility with 'it' syntax before migration decisions
  Owner: engineering team

## Implementation Notes

- Configure ESLint with jest/consistent-test-it rule set to 'it' to enforce the standard automatically
- Update project documentation and testing guidelines to explicitly specify 'it' as the required test declaration function
- Create test file templates or snippets in the development environment that generate properly structured 'it' declarations
- During code reviews, verify that new test files follow the __tests__/<ComponentName>-test.tsx naming convention and use 'it' declarations

## Continuation Context


Verify commands:
- grep -r "^[[:space:]]*it('" template/src/__tests__/ || echo 'No it() declarations found'
- find . -path '*/__tests__/*-test.tsx' -type f | head -5
- npx eslint --print-config template/src/__tests__/App-test.tsx | grep -A 5 'jest/consistent-test-it' || echo 'Linting rule not configured'

Accept when:
- All test files in __tests__ directories use 'it' function for test case declarations
- ESLint configuration includes jest/consistent-test-it rule enforcing 'it' syntax
- Test files follow the naming pattern <ComponentName>-test.tsx and are located in __tests__ directories

## Enforcement

- Verified by: ESLint with jest/consistent-test-it rule in CI pipeline
- Verified by: Code review checklist verification for test file structure
- Verified by: Automated grep-based checks in pre-commit hooks or CI
- Violation handling: CI pipeline fails on ESLint violations related to test declaration syntax
- Violation handling: Code review feedback requests changes to non-compliant test declarations
- Violation handling: Automated tooling flags violations with actionable error messages
- Exception process: Exceptions require documentation in test file comments explaining the rationale
- Exception process: Technical lead approval required for deviations from standard 'it' syntax
- Exception process: Exception cases logged in project ADR amendments for future reference