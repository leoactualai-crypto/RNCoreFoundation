# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Screens Use React

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all React Native mobile application development within the codebase.

## Context

- The codebase contains 29 files implementing a React Native mobile application with consistent navigation and UI component patterns across screens, navigators, and feature modules.
- Navigation architecture uses @react-navigation/stack, @react-navigation/drawer, and @react-navigation/material-bottom-tabs to coordinate screen transitions and user flows across authentication, main content, and topic-specific sections.
- UI components consistently use react-native-paper for Material Design theming and react-native-vector-icons/MaterialCommunityIcons for iconography, establishing a unified visual language.
- The application separates concerns through typed navigation parameter lists (TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList) that define public contracts between navigators and screens.
- StyleSheet.create() patterns appear consistently across 15 files, indicating standardized styling practices for layout, typography, and component presentation.

## Problem Statement

Mobile applications require consistent navigation patterns, UI component libraries, and styling approaches to maintain code quality, developer productivity, and user experience coherence across multiple screens and feature areas. Without standardized choices for navigation frameworks and UI libraries, teams face fragmentation in implementation patterns, increased maintenance burden, and inconsistent user interfaces.

## Decision

1. MAY: Screens MAY use @react-navigation/native hooks (useNavigation, useFocusEffect) for navigation interactions within functional components.

## Policy Block

- MAY Screens MAY use @react-navigation/native hooks (useNavigation, useFocusEffect) for navigation interactions within functional components.

In scope:
- All React Native mobile application screens and components
- Navigation flows including authentication, main content, drawer menus, and bottom tab navigation
- UI component implementations requiring Material Design theming
- TypeScript type definitions for navigation parameter lists and screen props
- Style definitions for layout, typography, spacing, and visual presentation

Out of scope:
- Web-based React applications using react-router or other web navigation libraries
- Native iOS or Android code in platform-specific modules
- Third-party library internal implementations
- Backend API implementations or service layer code

Exceptions:
- EXC-001: Custom native navigation components are required for platform-specific features not supported by @react-navigation
- EXC-002: Alternative UI component libraries are needed for specialized components not available in react-native-paper

## Rationale

- Evidence shows 29 files consistently using react, react-native, @react-navigation libraries, and react-native-paper, indicating an established architectural pattern with broad adoption across the codebase.
- Typed navigation parameter lists (TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList) appear in multiple files, demonstrating intentional API contract design for navigation boundaries.
- StyleSheet.create() usage across 15 files with consistent property patterns (container, flex, padding, margin, fontSize) indicates standardized styling practices that improve performance and maintainability.
- The combination of @react-navigation/stack, @react-navigation/drawer, and @react-navigation/material-bottom-tabs provides comprehensive navigation patterns covering the primary mobile UI paradigms needed for the application.

## Consequences

Positive:
- Consistent navigation patterns reduce cognitive load for developers moving between different parts of the codebase and enable code reuse across screens.
- TypeScript-typed navigation contracts provide compile-time safety for screen parameters, preventing runtime navigation errors and improving refactoring confidence.
- Material Design theming through react-native-paper ensures visual consistency and reduces custom styling effort while maintaining platform-appropriate UI patterns.
- StyleSheet.create() optimization enables React Native to optimize style objects and improve rendering performance compared to inline styles.

Negative:
- Dependency on @react-navigation and react-native-paper creates coupling to these specific libraries, requiring migration effort if architectural needs change.
- Material Design patterns may not align with all brand requirements, potentially requiring custom theming or component overrides that increase complexity.
- Learning curve for developers unfamiliar with @react-navigation's API surface and navigation paradigms may slow initial development velocity.
- StyleSheet.create() requires separate style object definitions, increasing boilerplate compared to inline styles for simple components.

## Alternatives

- Use React Navigation v4 or earlier versions with different API patterns (rejected)
  Rejected because: Evidence shows consistent use of modern @react-navigation packages with scoped imports (@react-navigation/stack, @react-navigation/drawer) indicating v5+ adoption, which provides improved TypeScript support and composition patterns.
  When valid: Only valid for legacy codebases requiring compatibility with older React Navigation versions
- Use alternative UI libraries such as React Native Elements, NativeBase, or custom component libraries (rejected)
  Rejected because: Evidence shows consistent react-native-paper usage across multiple screens (Options, Products, TopicDrawerNavigator) indicating established Material Design theming choice and component library standardization.
  When valid: Valid for applications with brand requirements incompatible with Material Design or requiring component features not available in react-native-paper
- Use inline styles or styled-components instead of StyleSheet.create() (rejected)
  Rejected because: Evidence shows StyleSheet.create() used consistently across 15 files with structured style objects, indicating intentional choice for performance optimization and style organization.
  When valid: Valid for prototypes or simple components where performance optimization is not critical

## Risks

- Breaking changes in @react-navigation major version upgrades may require significant refactoring across 29+ files using navigation patterns.
  Mitigation: Pin @react-navigation dependencies to specific major versions, establish migration testing strategy, and monitor release notes for breaking changes before upgrading.
  Owner: Mobile architecture team
- Material Design patterns from react-native-paper may conflict with evolving brand design requirements, requiring extensive custom theming or component replacement.
  Mitigation: Establish theme customization layer early, document brand-specific overrides, and evaluate react-native-paper theming capabilities against brand guidelines quarterly.
  Owner: UI/UX team and mobile architecture team
- TypeScript navigation type definitions may become inconsistent across navigators if not maintained centrally, leading to type safety gaps.
  Mitigation: Centralize navigation type definitions in dedicated types.ts files per navigator, enforce type checking in CI, and establish code review guidelines for navigation contract changes.
  Owner: Engineering team

## Implementation Notes

- Create new screens by defining TypeScript navigation param list types first, then implementing screen components with typed navigation props using StackScreenProps or similar types.
- Organize navigator components in dedicated directories (e.g., navigators/TopicDrawerNavigator/, navigators/MainStackNavigator/) with co-located types.ts files for navigation contracts.
- Use react-native-paper's Provider component at the app root to enable theming, and access theme values through useTheme() hook for consistent styling.
- Define StyleSheet objects at the bottom of component files using StyleSheet.create() with descriptive property names that reflect component structure (container, title, button, etc.).
- Import MaterialCommunityIcons from react-native-vector-icons/MaterialCommunityIcons and use consistent icon naming conventions across the application.

## Continuation Context


Verify commands:
- grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l
- grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l
- grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l
- find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l

Accept when:
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).

## Enforcement

- Verified by: Automated CI checks using grep patterns to verify @react-navigation and react-native-paper imports in screen components
- Verified by: TypeScript compilation enforcing navigation parameter list types at build time
- Verified by: Code review checklist items verifying StyleSheet.create() usage and navigation contract definitions
- Verified by: ESLint rules detecting inline styles or non-standard navigation patterns
- Violation handling: CI build failures for missing TypeScript navigation types or incorrect import patterns
- Violation handling: Code review rejection for screens not following navigation contract patterns or StyleSheet.create() conventions
- Violation handling: Automated linting warnings for inline styles or alternative UI library usage without documented exceptions
- Violation handling: Architecture review required for any introduction of alternative navigation or UI component libraries
- Exception process: Submit exception request to mobile architecture team with justification for alternative approach and impact analysis
- Exception process: Document approved exceptions in ADR supplement with specific scope, rationale, and time-bound review period
- Exception process: Add ESLint disable comments with exception ID reference for approved deviations from standard patterns
- Exception process: Review all active exceptions quarterly to determine if they can be resolved or require continued approval