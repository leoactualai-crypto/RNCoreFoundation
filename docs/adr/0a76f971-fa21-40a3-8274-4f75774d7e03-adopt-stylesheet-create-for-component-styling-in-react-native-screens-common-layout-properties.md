# Adopt StyleSheet.create for Component Styling in React Native Screens: Common Layout Properties

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- React Native applications require a styling mechanism that provides type safety, performance optimization, and consistency across components
- The codebase contains multiple screen components (Chat, Login, Registration, App) that each define their own visual presentation and layout
- StyleSheet.create provides compile-time validation and runtime optimization by creating immutable style objects that can be referenced efficiently
- Screen components use React hooks (useState, useCallback) for state management and require coordinated styling that responds to component lifecycle and user interactions
- The application integrates react-native-paper for Material Design components alongside custom-styled elements, requiring a consistent styling approach

## Problem Statement

React Native screen components require a standardized approach to defining and applying styles that ensures performance, maintainability, and consistency across the application while supporting both custom components and third-party UI libraries.

## Decision

1. MUST: Common layout properties (flex, justifyContent, padding, margin) MUST be defined in StyleSheet objects rather than inline

## Policy Block

- MUST Common layout properties (flex, justifyContent, padding, margin) MUST be defined in StyleSheet objects rather than inline

In scope:
- All React Native screen components in the /screens directory
- Custom components that define visual presentation
- Layout containers and view hierarchies
- Typography and text styling
- Spacing and positioning properties

Out of scope:
- Dynamic styles that depend on runtime props or state values
- Third-party component internal styling
- Platform-specific styles that require conditional logic
- Animation styles that change during component lifecycle

Exceptions:
- EXC-001: Styles must be computed dynamically based on props, state, or runtime conditions
- EXC-002: Integrating third-party libraries that require inline style objects

## Rationale

- StyleSheet.create provides performance optimization by validating and freezing style objects at creation time, reducing runtime overhead during re-renders
- The pattern is consistently observed across 4 screen components (Chat.tsx, Login.tsx, Registration.tsx, App.tsx) with 86.40% confidence, indicating an established architectural convention
- Centralized style definitions improve maintainability by separating presentation concerns from component logic and enabling easier refactoring
- Type safety and validation at style creation time catch errors earlier in the development cycle compared to inline style objects

## Consequences

Positive:
- Improved rendering performance through style object optimization and immutability
- Enhanced code maintainability with clear separation between component logic and presentation
- Better developer experience with type checking and autocomplete for style properties
- Consistent styling patterns across the application reducing cognitive load for developers

Negative:
- Additional boilerplate code required for each component with StyleSheet.create declarations
- Dynamic styling scenarios require workarounds or exceptions to the pattern
- Learning curve for developers unfamiliar with React Native's styling approach
- Potential for style duplication across components without additional abstraction layers

## Alternatives

- Use inline style objects directly on components without StyleSheet.create (rejected)
  Rejected because: Inline styles lack performance optimization, type validation, and create new objects on every render causing unnecessary re-renders
  When valid: Only for truly dynamic styles that must be computed per render based on props or state
- Adopt styled-components or emotion for CSS-in-JS styling (rejected)
  Rejected because: Adds additional dependencies and runtime overhead; React Native's StyleSheet is the platform-native solution with better performance characteristics
  When valid: When sharing styling logic with React web applications or requiring advanced theming capabilities
- Use external stylesheet files or CSS modules (rejected)
  Rejected because: React Native does not support traditional CSS files; StyleSheet API is the standard platform approach
  When valid: Not applicable for React Native applications

## Risks

- Style duplication across components leading to inconsistent design and maintenance burden
  Mitigation: Introduce shared style constants or theme objects for common values (colors, spacing, typography); consider a design token system
  Owner: Frontend engineering team
- Developers may circumvent StyleSheet.create for convenience, degrading performance over time
  Mitigation: Implement linting rules to detect inline style objects; provide clear documentation and examples of proper usage
  Owner: Engineering team leads
- Complex dynamic styling requirements may lead to awkward code patterns or performance issues
  Mitigation: Document approved patterns for dynamic styling; consider memoization strategies for computed styles
  Owner: Architecture team

## Implementation Notes

- Define StyleSheet.create() calls at the bottom of each screen component file, after the component definition
- Use descriptive style names that clearly indicate the component or element they apply to (e.g., 'container', 'title', 'button')
- For shared styles across multiple components, extract common style definitions into a separate theme or styles module
- When dynamic styling is necessary, compute style values outside the render method and combine with StyleSheet-defined base styles using array syntax

## Continuation Context


Verify commands:
- grep -r 'StyleSheet.create' template/src/screens/ | wc -l
- grep -r 'style={{' template/src/screens/ --include='*.tsx' | wc -l
- find template/src/screens -name '*.tsx' -exec grep -L 'StyleSheet.create' {} \;

Accept when:
- All screen components in /screens directory contain at least one StyleSheet.create() declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- Linting passes without violations of style definition rules

## Enforcement

- Verified by: ESLint rules checking for StyleSheet.create usage in screen components
- Verified by: Code review checklist items for new screen components
- Verified by: Automated static analysis in CI pipeline
- Violation handling: CI build warnings for missing StyleSheet.create in new screen components
- Violation handling: Code review feedback requiring refactoring of inline styles to StyleSheet definitions
- Violation handling: Documentation of exceptions in component comments when dynamic styling is required
- Exception process: Developer documents the technical reason for exception in code comments
- Exception process: Component owner or tech lead reviews and approves the exception
- Exception process: Exception is tracked in component documentation with justification