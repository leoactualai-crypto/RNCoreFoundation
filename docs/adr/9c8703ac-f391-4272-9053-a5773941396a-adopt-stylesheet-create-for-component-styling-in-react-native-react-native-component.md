# Adopt StyleSheet.create() for Component Styling in React Native: React Native Component

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- React Native applications require a styling mechanism that bridges JavaScript component definitions with native rendering performance characteristics
- The codebase contains 10 files across navigators, screens, and components that consistently use StyleSheet.create() to define component styles
- Components span multiple architectural layers including navigation (TopicDrawerNavigator, MainStackNavigator), screens (Options, Products, Chat, Login), and reusable components (Loading, BottomTabs, DemoComponent02)
- The pattern appears in conjunction with React Navigation libraries (@react-navigation/drawer, @react-navigation/stack, @react-navigation/material-bottom-tabs) and React Native Paper UI framework
- Style definitions are co-located with component definitions, creating a consistent pattern for accessing visual presentation data within component modules

## Problem Statement

React Native components require a standardized approach to define, organize, and access styling data that optimizes for native rendering performance while maintaining developer ergonomics and type safety. Without a consistent styling pattern, components may use inline styles that prevent optimization, create inconsistent style organization across the codebase, and lack compile-time validation of style properties.

## Decision

1. MUST: All React Native component styles MUST be defined using StyleSheet.create() rather than plain JavaScript objects

## Policy Block

- MUST All React Native component styles MUST be defined using StyleSheet.create() rather than plain JavaScript objects

In scope:
- All React Native functional and class components
- Navigation components (drawer, stack, tab navigators)
- Screen components
- Reusable UI components
- Layout containers and wrappers

Out of scope:
- Web-only React components (non-React Native)
- Native module implementations in Objective-C or Java/Kotlin
- Third-party library components where styling is controlled externally
- Dynamic styles computed at runtime based on props or state (may be combined with StyleSheet styles)

Exceptions:
- EXC-001: Dynamic styles must be computed based on runtime props, state, or theme values that cannot be statically defined
- EXC-002: Prototyping or experimental components in development that have not reached code review

## Rationale

- StyleSheet.create() validates style properties at creation time, catching errors earlier in the development cycle and providing better developer feedback
- The React Native bridge optimizes StyleSheet-created styles by sending style references rather than full style objects across the JavaScript-to-native boundary, improving rendering performance
- Evidence from 10 files across navigators, screens, and components demonstrates consistent adoption of this pattern, indicating established team practice and architectural alignment
- Co-locating styles with components using StyleSheet.create() provides a clear, discoverable pattern for accessing component presentation data while maintaining separation from component logic

## Consequences

Positive:
- Improved rendering performance through style reference optimization across the React Native bridge
- Earlier error detection through style property validation at StyleSheet creation time
- Consistent code organization pattern across 10+ files spanning multiple architectural layers
- Better developer experience with autocomplete and type checking for style properties

Negative:
- Increased verbosity compared to inline style objects, requiring separate StyleSheet.create() declarations
- Limited flexibility for highly dynamic styles that depend on runtime values, requiring hybrid approaches
- Additional learning curve for developers transitioning from web React to React Native
- Potential for style definition bloat in files with many component variants

## Alternatives

- Use inline JavaScript style objects directly in component JSX without StyleSheet.create() (rejected)
  Rejected because: Inline styles bypass React Native's style optimization, sending full style objects across the bridge on every render, degrading performance. They also lack compile-time validation.
  When valid: Only for prototyping or when styles are highly dynamic and computed per-render based on complex runtime state
- Adopt a CSS-in-JS library like styled-components or emotion for React Native (rejected)
  Rejected because: No evidence of styled-components or emotion in the detected libraries. Introducing a new styling paradigm would create inconsistency with the established StyleSheet.create() pattern across 10 files.
  When valid: For new projects or major refactoring where advanced theming, dynamic styling, or component composition patterns justify the additional dependency
- Extract styles to separate style files or style modules (deferred)
  Rejected because: Current evidence shows co-location pattern. Separation could improve reusability but adds indirection.
  When valid: When style definitions become large enough to obscure component logic, or when styles need to be shared across multiple components

## Risks

- Developers may bypass StyleSheet.create() for convenience, especially when adding quick fixes or prototyping features, leading to inconsistent styling patterns
  Mitigation: Implement ESLint rule to detect inline style objects and enforce StyleSheet.create() usage. Include pattern in code review checklist.
  Owner: Engineering team
- Complex dynamic styling requirements may force awkward hybrid patterns that combine StyleSheet styles with inline computed styles, reducing code clarity
  Mitigation: Document approved patterns for dynamic styling. Consider utility functions or hooks for common dynamic style computations.
  Owner: Engineering team
- Large style definitions may bloat component files, making them harder to navigate and maintain
  Mitigation: Establish file size thresholds for extracting styles to separate modules. Document when style extraction is appropriate.
  Owner: Engineering team

## Implementation Notes

- Import StyleSheet from 'react-native' at the top of each component file that requires styling
- Define styles using StyleSheet.create() at the module level, typically at the bottom of the file after component definitions
- Reference styles in JSX using the style prop with the created stylesheet object (e.g., style={styles.container})
- For dynamic styles, use array syntax to combine static StyleSheet styles with computed style objects: style={[styles.base, dynamicStyle]}
- Use descriptive style keys that reflect component structure: container, title, button, text, icon, etc.
- Consider TypeScript typing for style objects to improve type safety and developer experience

## Continuation Context


Verify commands:
- grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" --include="*.jsx" --include="*.js" | wc -l
- grep -r "style={{" template/src --include="*.tsx" --include="*.jsx" | grep -v "StyleSheet" | wc -l
- find template/src -type f \( -name "*.tsx" -o -name "*.jsx" \) -exec grep -l "from 'react-native'" {} \; | xargs grep -L "StyleSheet.create" | wc -l

Accept when:
- StyleSheet.create() usage count is greater than or equal to the number of component files that import react-native
- Inline style object usage (style={{ }}) without StyleSheet.create() is less than 5% of total style definitions
- All screen components, navigator components, and reusable UI components use StyleSheet.create() for style definitions

## Enforcement

- Verified by: ESLint rule checking for inline style objects without StyleSheet.create()
- Verified by: Code review checklist item verifying StyleSheet.create() usage in new components
- Verified by: Automated grep-based verification in CI pipeline counting StyleSheet.create() usage
- Violation handling: ESLint violations block PR merge until resolved
- Violation handling: Code review feedback requests refactoring of inline styles to StyleSheet.create()
- Violation handling: CI pipeline warnings for files with high inline style usage ratios
- Exception process: Developer documents reason for inline style usage in code comment
- Exception process: Code reviewer approves exception based on technical justification (e.g., highly dynamic runtime-computed styles)
- Exception process: Exception is tracked in PR description and linked to this ADR for future reference