# Adopt Jest 'it' Block Syntax for React Native Test Suites: Test Files React

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native projects require a testing framework that supports TypeScript/TSX component testing with minimal configuration overhead
- The template codebase establishes baseline testing patterns that downstream projects inherit and extend
- Jest provides native support for React Native through react-test-renderer and integrates with the Metro bundler ecosystem
- The 'it' block syntax represents a BDD-style testing convention that improves test readability and aligns with JavaScript ecosystem standards

## Problem Statement

React Native projects need a consistent, discoverable testing syntax that balances readability with framework compatibility, while the template layer must establish patterns that scale across multiple derived projects without imposing excessive cognitive overhead on developers unfamiliar with testing DSL variations.

## Decision

1. MUST: Test files in React Native projects MUST use the 'it' block syntax for defining individual test cases

## Policy Block

- MUST Test files in React Native projects MUST use the 'it' block syntax for defining individual test cases

In scope:
- React Native component test files
- Template baseline test suites in __tests__ directories
- TypeScript and TSX test modules using Jest framework
- Unit and integration tests for React Native UI components

Out of scope:
- End-to-end tests using Detox or Appium frameworks
- Native module tests written in Java/Kotlin or Objective-C/Swift
- Performance benchmarking suites
- Snapshot tests using alternative frameworks

## Rationale

- The evidence shows explicit use of 'it' block syntax in template/src/__tests__/App-test.tsx, establishing this as the canonical pattern for the template layer
- React Native detection (libs.core.detected='react-native') confirms the framework context, where Jest is the de facto standard testing framework with built-in 'it' and 'test' aliases
- Template files serve as architectural blueprints that propagate patterns to derived projects, making syntax consistency at this layer critical for ecosystem coherence
- The minimal example implementation ('it("example", () => { return; })') demonstrates intentional simplicity, avoiding prescriptive assertion libraries while establishing structural conventions

## Consequences

Positive:
- Consistent test syntax across React Native projects reduces cognitive load when developers context-switch between codebases
- BDD-style 'it' blocks improve test readability by framing tests as behavioral specifications rather than procedural checks
- Alignment with Jest's default conventions minimizes configuration overhead and leverages extensive community documentation
- Template-level standardization enables automated tooling (linters, generators) to reliably parse and validate test structure

Negative:
- Teams with strong preferences for 'test' syntax may experience friction when adopting template-derived projects
- Minimal example tests provide limited guidance on assertion patterns, potentially leading to inconsistent test quality in derived projects
- Strict adherence to __tests__ directory structure may conflict with alternative organizational schemes (e.g., co-located .test.tsx files)
- Template simplicity may not adequately demonstrate advanced Jest features (describe blocks, hooks, mocking) that production tests require

## Alternatives

- Use 'test' block syntax instead of 'it' for explicit test nomenclature (rejected)
  Rejected because: While 'test' and 'it' are functionally equivalent Jest aliases, the evidence explicitly shows 'it' usage in the template, and BDD-style 'it' syntax is more prevalent in React Native ecosystem examples and documentation
  When valid: Valid for teams with established 'test' conventions in non-React Native codebases seeking consistency across polyglot projects
- Adopt Mocha or Jasmine as the primary testing framework with describe/it syntax (rejected)
  Rejected because: Jest is the standard testing framework for React Native with native Metro bundler integration, and switching frameworks would introduce configuration complexity without evidence of inadequacy
  When valid: Valid for projects with existing Mocha/Jasmine infrastructure or specific requirements for browser-based test execution
- Provide no template test files, allowing each project to establish its own testing conventions (rejected)
  Rejected because: Template files serve as architectural documentation and reduce time-to-first-test for new projects; absence of examples increases inconsistency across the ecosystem
  When valid: Valid for highly opinionated organizations with centralized testing standards enforced through separate tooling

## Risks

- Minimal example tests may be copied verbatim into production code, resulting in placeholder tests that provide no actual coverage
  Mitigation: Enhance template tests with inline comments explaining that examples should be replaced with meaningful assertions; integrate coverage thresholds in CI pipelines
  Owner: Engineering team, DevOps
- Ecosystem evolution (e.g., Jest alternatives like Vitest gaining React Native support) may render this pattern obsolete
  Mitigation: Monitor React Native testing ecosystem quarterly; establish deprecation criteria based on community adoption metrics and framework maintenance status
  Owner: Architecture review board
- Single-file evidence (1 file) provides limited confidence that this pattern represents a deliberate architectural decision versus incidental implementation
  Mitigation: Validate pattern across additional template files and derived projects; increase confidence threshold before enforcing as mandatory standard
  Owner: Detection pipeline maintainers

## Implementation Notes

- Configure Jest in package.json or jest.config.js with preset: 'react-native' to ensure proper TypeScript and TSX transformation
- Use @testing-library/react-native for component rendering and queries, as it provides better accessibility-focused selectors than react-test-renderer alone
- Structure test files to mirror source directory hierarchy (e.g., src/components/Button.tsx → src/__tests__/Button-test.tsx) for discoverability
- Include at least one meaningful assertion per 'it' block; avoid empty test bodies except in template examples explicitly marked as placeholders

## Continuation Context


Verify commands:
- grep -r "it('" template/src/__tests__/ || grep -r 'it("' template/src/__tests__/
- find . -path '*/__tests__/*-test.tsx' -o -path '*/__tests__/*-test.ts' | head -5
- grep -l 'react-native' package.json && grep -l 'jest' package.json

Accept when:
- At least one test file in __tests__ directory uses 'it' block syntax with a string description and callback function
- Jest is configured as a devDependency with react-native preset or equivalent transformation setup
- Test files follow the -test.tsx or -test.ts naming convention and are discoverable by Jest's default test match patterns

## Enforcement

- Verified by: ESLint rules checking for consistent 'it' vs 'test' usage across test files
- Verified by: CI pipeline test execution confirming Jest can discover and run tests in __tests__ directories
- Verified by: Code review checklist items verifying test file naming conventions and structure
- Violation handling: CI build warnings for test files not following __tests__/*-test.tsx naming pattern
- Violation handling: Linter errors for test blocks using non-standard syntax (e.g., raw function definitions instead of 'it' blocks)
- Violation handling: Pull request comments from automated tooling highlighting deviations from template patterns
- Exception process: Document alternative testing approach in project README with rationale for deviation from template standard
- Exception process: Obtain architecture review approval for projects using non-Jest frameworks or alternative directory structures
- Exception process: Update ESLint configuration to disable specific rules for exception cases, with inline comments explaining context