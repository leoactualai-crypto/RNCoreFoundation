# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Style Definitions Use

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- React Native applications require a consistent approach to defining and applying component styles across screens, navigators, and reusable components
- The StyleSheet.create() API provides a structured mechanism for declaring style objects with validation and optimization benefits over inline style objects
- Evidence shows 13 files across the template codebase (screens, navigators, components) consistently use StyleSheet.create() to define component-level style dictionaries
- The pattern appears in all major UI layers: authentication screens (Login, Registration, Splash), feature screens (Chat, Products, Options), navigation components (TopicDrawerNavigator, MainStackNavigator), and utility components (Error, Loading, Heading, BottomTabs)
- This pattern establishes a data access convention where style definitions are treated as structured data objects accessed via named keys rather than computed or inline styles

## Problem Statement

React Native applications need a standardized approach to define, organize, and access component styles that provides type safety, performance optimization, and maintainability across a growing codebase with multiple screens, navigators, and reusable components.

## Decision

1. MUST: Style definitions MUST use React Native's supported style properties (flex, padding, fontSize, etc.) and MUST NOT use web-specific CSS properties

## Policy Block

- MUST Style definitions MUST use React Native's supported style properties (flex, padding, fontSize, etc.) and MUST NOT use web-specific CSS properties

In scope:
- All React Native screen components (Chat, Splash, Login, Registration, Products, Options)
- All React Native navigator components (TopicDrawerNavigator, MainStackNavigator)
- All reusable React Native UI components (Error, Loading, Heading, BottomTabs)
- Root application component (App.tsx) and any component rendering React Native View, Text, or other native components

Out of scope:
- Web-specific React components using CSS, CSS-in-JS libraries, or styled-components
- React Native components that exclusively use third-party component library theming (e.g., react-native-paper theme objects)
- Configuration files, utility modules, or non-UI code
- Test files and mock components

Exceptions:
- EXC-001: Component requires fully dynamic styles computed at runtime based on props, state, or external data where no static style properties exist
- EXC-002: Component uses StyleSheet.absoluteFill or other StyleSheet utility methods that return style objects rather than creating custom style dictionaries

## Rationale

- Evidence shows 13 files with 89.28% confidence consistently use StyleSheet.create() across all UI layers, indicating an established architectural pattern rather than isolated usage
- StyleSheet.create() provides performance optimization by validating and freezing style objects at creation time, reducing runtime overhead compared to inline style objects recreated on each render
- The pattern creates a clear data access convention where styles are treated as structured, immutable data objects accessed via named keys, improving code readability and maintainability
- Consistent use across screens (Chat, Login, Registration, Splash, Products, Options), navigators (TopicDrawerNavigator, MainStackNavigator), and components (Error, Loading, Heading, BottomTabs) demonstrates organization-wide adoption

## Consequences

Positive:
- Style objects are validated at creation time, catching invalid style properties during development rather than at runtime
- Performance optimization through style object reuse across component renders, reducing memory allocation and garbage collection overhead
- Improved code organization with clear separation between component logic and style definitions
- Better developer experience with IDE autocomplete and type checking for style property names and values
- Consistent pattern across codebase reduces cognitive load when navigating between different components and screens

Negative:
- Static style definitions limit flexibility for highly dynamic styling scenarios requiring runtime computation
- Additional boilerplate code required compared to inline styles, increasing file length for simple components
- Module-level style definitions cannot directly access component props or state, requiring style composition patterns for dynamic styling
- Learning curve for developers transitioning from web development where inline styles or CSS-in-JS are more common

## Alternatives

- Use inline style objects defined directly in JSX style props (rejected)
  Rejected because: Inline styles are recreated on every render, causing performance degradation and preventing React Native's style optimization. Evidence shows zero usage of this pattern in the 13 analyzed files.
  When valid: Only for truly one-off dynamic styles that cannot be precomputed
- Use CSS-in-JS libraries like styled-components or emotion (rejected)
  Rejected because: Adds external dependencies and abstracts away React Native's native styling system. Evidence shows exclusive use of StyleSheet.create() without any CSS-in-JS library imports.
  When valid: When building cross-platform React/React Native applications requiring shared styling logic
- Use centralized theme/style modules with exported style objects (rejected)
  Rejected because: Evidence shows component-level style encapsulation with each file defining its own StyleSheet.create() block, maintaining clear component boundaries and avoiding global style coupling.
  When valid: For truly shared design tokens (colors, spacing, typography) that should be consistent across all components

## Risks

- Developers may bypass StyleSheet.create() for convenience when implementing new components, creating inconsistency in the codebase
  Mitigation: Implement automated linting rules to detect inline style objects and enforce StyleSheet.create() usage. Add code review checklist item for style definition patterns.
  Owner: Engineering team, enforced via CI/CD pipeline
- Complex dynamic styling requirements may lead to convoluted style composition logic or workarounds that reduce code clarity
  Mitigation: Document approved patterns for dynamic styling (style arrays, conditional style merging) and provide component examples. Allow exceptions for genuinely dynamic cases with tech lead approval.
  Owner: Tech lead and senior engineers
- Large StyleSheet.create() blocks in complex components may reduce readability and make style maintenance difficult
  Mitigation: Establish guidelines for breaking down large components into smaller sub-components with focused style definitions. Consider style object size limits (e.g., max 15-20 style keys per StyleSheet).
  Owner: Engineering team during code review

## Implementation Notes

- Place StyleSheet.create() calls at the bottom of each component file, after the component definition but before the export statement, for consistent file structure
- Use TypeScript or Flow type annotations for style objects to enable IDE autocomplete and catch type errors: const styles: StyleSheet.NamedStyles<any> = StyleSheet.create({...})
- For dynamic styling needs, create style arrays combining static StyleSheet styles with computed inline styles: style={[styles.base, dynamicCondition && styles.variant]}
- When migrating existing components, use React Native's StyleSheet.flatten() to debug style composition and verify that StyleSheet.create() produces equivalent results to previous inline styles
- Document any exceptions to this pattern (EXC-001, EXC-002) directly in component file comments to maintain architectural decision visibility

## Continuation Context


Verify commands:
- grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" | wc -l
- grep -r "style={{" template/src --include="*.tsx" | grep -v "style={\[" | wc -l
- eslint template/src --rule 'react-native/no-inline-styles: error' --ext .tsx,.ts

Accept when:
- All React Native component files contain at least one StyleSheet.create() call for component-specific styles
- Inline style objects (style={{...}}) are only used in combination with StyleSheet-defined styles or for documented dynamic styling exceptions
- ESLint rule 'react-native/no-inline-styles' passes with zero violations across the codebase

## Enforcement

- Verified by: ESLint rule 'react-native/no-inline-styles' configured in .eslintrc to detect inline style objects
- Verified by: CI/CD pipeline runs linting checks on all pull requests and blocks merge on style violations
- Verified by: Code review checklist includes verification that new components use StyleSheet.create()
- Verified by: Automated grep-based verification in CI to count StyleSheet.create() usage vs inline styles
- Violation handling: CI build fails if ESLint detects inline style objects without approved exception comments
- Violation handling: Pull requests with style violations are automatically flagged for tech lead review
- Violation handling: Existing violations are tracked in technical debt backlog with priority based on component usage frequency
- Violation handling: Quarterly codebase audits identify and remediate any bypassed style patterns
- Exception process: Developer documents exception rationale in component file comments referencing EXC-001 or EXC-002
- Exception process: Tech lead reviews exception request during pull request, verifying that StyleSheet.create() genuinely cannot satisfy the requirement
- Exception process: Approved exceptions are logged in architecture decision log with component name, rationale, and approval date
- Exception process: Exceptions are revisited during major refactoring efforts to determine if new patterns or libraries enable migration to standard StyleSheet.create() approach