# Standardize Module Export Contracts for Public API Surfaces: Context Providers Usercontext

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase exhibits a consistent pattern of exporting named constants, objects, and functions as public module contracts across 52 files with 88.05% confidence
- React Native application architecture requires stable public interfaces for components (ChatScreen, LoginScreen, SplashScreen), contexts (UserContext, AuthContext), theming (brandDarkTheme, darkTheme, palette), and configuration (store, fonts, colors)
- Module exports serve as the primary API boundary mechanism, with files like palette.ts, colors.ts, fonts.ts, and buttons.ts exporting named constants that form the design system contract
- Redux Toolkit store configuration, React Navigation themes, and React Native Paper theming all depend on predictable export contracts for cross-module integration
- The pattern spans configuration files (.prettierrc.js), asset definitions (fonts.ts, colors.ts), styling modules (themes/, buttons.ts), state management (store/index.ts), and UI components (screens/, components/)

## Problem Statement

Without standardized module export contracts, public API surfaces become inconsistent, making it difficult for consumers to discover and use exported functionality reliably. The codebase needs a consistent approach to defining what constitutes a public API surface versus internal implementation details, particularly for design system tokens, component interfaces, state management exports, and configuration objects.

## Decision

1. SHOULD: Context providers (UserContext, AuthContext) SHOULD be exported as named exports to maintain consistency with other public API patterns

## Policy Block

- SHOULD Context providers (UserContext, AuthContext) SHOULD be exported as named exports to maintain consistency with other public API patterns

In scope:
- All TypeScript/JavaScript modules in src/ that export functionality consumed by other modules
- Design system modules: colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts
- Theme configuration: themes/brandDarkTheme.ts, themes/dark.ts
- State management exports: store/index.ts (store, AppDispatch, AppThunk)
- React components: screens/*, components/* that serve as public interfaces
- Context providers: UserContext, AuthContext
- Configuration files with exported contracts: .prettierrc.js

Out of scope:
- Internal helper functions within modules that are not exported
- Test files and test utilities
- Build configuration files that do not export runtime contracts
- Native Android/iOS code (Java, Kotlin, Swift, Objective-C) with platform-specific export mechanisms
- Third-party library code in node_modules

Exceptions:
- EXC-001: Legacy components require default exports for compatibility with existing dynamic import() statements or React.lazy() usage
- EXC-002: Configuration files (e.g., .prettierrc.js, babel.config.js) require module.exports for tool compatibility

## Rationale

- The evidence shows 52 files consistently using named exports for public contracts (api.public.contracts detected across palette, colors, fonts, themes, components, store), indicating an established architectural pattern worth codifying
- Named exports provide better IDE autocomplete, refactoring support, and tree-shaking capabilities compared to default exports, particularly important for design system tokens used across multiple files
- Explicit export contracts create clear API boundaries between modules, making it easier to identify breaking changes during code review and enforce semantic versioning
- The pattern aligns with React Native and Redux Toolkit best practices, as evidenced by store/index.ts exporting AppDispatch and AppThunk types alongside the store instance

## Consequences

Positive:
- Improved discoverability: developers can use IDE autocomplete to discover available exports from design system and utility modules
- Better tree-shaking: bundlers can eliminate unused exports more effectively with named exports, reducing bundle size
- Clearer API contracts: explicit named exports make it obvious what is intended for public consumption versus internal implementation
- Easier refactoring: IDEs can reliably find all usages of named exports and safely rename them across the codebase

Negative:
- Increased verbosity: named exports require explicit import statements (import { palette } from './palette') rather than shorter default imports
- Migration cost: existing code using default exports would need refactoring to align with the standard
- Learning curve: new developers must understand the distinction between named and default exports and when to use each
- Potential for over-exporting: without discipline, developers might export too many internal details, creating unintended public API surface

## Alternatives

- Use default exports for all modules and rely on file naming conventions to communicate purpose (rejected)
  Rejected because: Default exports provide poor IDE support for discovery, make tree-shaking less effective, and the evidence shows the codebase has already standardized on named exports for 52 files
  When valid: Only valid for tool configuration files that require module.exports for compatibility
- Export a single aggregate object per module (e.g., export const Colors = { black, darkGray, ... }) instead of individual named exports (rejected)
  Rejected because: Aggregate objects prevent tree-shaking and the evidence shows granular exports (black, darkestGray, darkGray as separate exports) are the established pattern
  When valid: Valid for modules where all exports are always used together and bundle size is not a concern
- Use TypeScript namespace exports to group related functionality (rejected)
  Rejected because: Namespaces are a legacy TypeScript feature; ES6 modules with named exports are the modern standard and align with the detected pattern
  When valid: Only for ambient declaration files (.d.ts) that augment third-party library types

## Risks

- Inconsistent adoption: developers may continue using default exports in new code, creating a mixed codebase with two competing patterns
  Mitigation: Add ESLint rule (import/no-default-export) to enforce named exports in specified directories; provide clear examples in documentation
  Owner: Engineering team + tech lead
- Over-exporting: developers may export internal implementation details, creating unintended public API surface that becomes difficult to change
  Mitigation: Establish code review checklist item to verify exports are intentionally public; use TypeScript internal keyword for truly internal APIs
  Owner: Code reviewers
- Breaking changes during migration: refactoring existing default exports to named exports could break dependent code if not done atomically
  Mitigation: Use automated refactoring tools (jscodeshift) to migrate imports and exports together; test thoroughly before merging
  Owner: Developer performing migration

## Implementation Notes

- Start with design system modules (colors.ts, fonts.ts, palette.ts, buttons.ts) as they have the most consumers and benefit most from named exports
- Use barrel files (index.ts) sparingly and only for components/ and screens/ directories where they simplify imports without hiding implementation
- Document public API contracts in each module's header comment, listing exported names and their intended usage
- For React components, export both the component and its prop types: export { ChatScreen }; export type { ChatScreenProps }
- Consider using TypeScript's export type syntax for type-only exports to enable better optimization: export type { AppDispatch, AppThunk }

## Continuation Context


Verify commands:
- grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l
- grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l
- eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx

Accept when:
- Named exports outnumber default exports by at least 10:1 ratio in src/ directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule import/no-default-export passes for all files in src/ except explicitly allowed configuration files

## Enforcement

- Verified by: ESLint rule import/no-default-export configured in .eslintrc with error level for src/ directory
- Verified by: Code review checklist includes verification that new exports follow named export pattern
- Verified by: CI pipeline runs grep-based verification commands to count default vs named exports and fails if ratio exceeds threshold
- Violation handling: CI build fails if ESLint detects default exports in non-exempt files
- Violation handling: Code review blocks merge if new default exports are introduced without documented exception
- Violation handling: Quarterly audit identifies remaining default exports and creates migration tickets
- Exception process: Developer documents exception reason in PR description with reference to EXC-001 or EXC-002
- Exception process: Tech lead reviews and approves exception with inline comment in code explaining rationale
- Exception process: Exception is recorded in exceptions.md file with approval date and planned remediation timeline