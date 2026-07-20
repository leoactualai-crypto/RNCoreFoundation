# Standardize Public API Contract Exports for React Native Components and Modules: Configuration Objects Exported

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains 43 files with explicit public API contract exports, indicating a systematic approach to defining module boundaries in a React Native application
- Components, navigators, screens, utilities, and type definitions consistently export named contracts that serve as integration points for other modules
- The pattern spans multiple architectural layers including UI components (TopicDrawerNavigator, OptionsScreen, DemoComponent02), authentication hooks (useAuth), state management (profileSlice), context providers (UserContext, AuthContext), and configuration modules (fonts, constants)
- React Navigation type definitions (RootStackParamList, MaterialBottomTabParamList, TopicDrawerStackParamList) establish typed navigation contracts across the application
- The presence of both TypeScript type exports and JavaScript module exports indicates a mixed codebase with gradual type adoption

## Problem Statement

Without standardized public API contract definitions, React Native applications face challenges in maintaining clear module boundaries, ensuring type safety across navigation and component integration, and preventing unintended coupling between architectural layers. The lack of explicit contract exports leads to implicit dependencies, difficult refactoring, and reduced code discoverability.

## Decision

1. MAY: Configuration objects MAY be exported as default exports when they represent a single cohesive configuration (e.g., .prettierrc.js module.exports)

## Policy Block

- MAY Configuration objects MAY be exported as default exports when they represent a single cohesive configuration (e.g., .prettierrc.js module.exports)

In scope:
- React components in src/components, src/screens, and src/navigators directories
- TypeScript type definitions for React Navigation param lists
- Custom React hooks in src/components and feature directories
- React Context providers and context objects
- Redux Toolkit slice reducers and actions
- Utility modules, constants, and configuration files in src/
- Index files that aggregate and re-export module contracts

Out of scope:
- Internal implementation details not intended for external consumption
- Private helper functions and utilities within component files
- Test files and test utilities
- Build configuration and tooling scripts
- Native Android/iOS platform code (except public Java/Kotlin/Swift APIs)
- Third-party library re-exports without local adaptation

Exceptions:
- EXC-001: Legacy JavaScript files being migrated to TypeScript may temporarily use module.exports instead of named exports
- EXC-002: Configuration files required by third-party tools (e.g., .prettierrc.js, babel.config.js) must follow tool-specific export conventions

## Rationale

- The detection of 43 files with explicit api.public.contracts facet demonstrates a consistent pattern of intentional contract definition across the codebase
- Named exports provide clear discoverability through IDE autocomplete and enable tree-shaking for optimized bundle sizes in React Native applications
- TypeScript type exports for navigation param lists enable compile-time type checking of navigation calls, preventing runtime navigation errors
- Consistent naming conventions (useAuth, UserContext, profileSlice.reducer) reduce cognitive load and improve code maintainability across team members

## Consequences

Positive:
- Clear module boundaries improve code discoverability and reduce time spent understanding component integration points
- Type-safe navigation contracts prevent runtime errors from invalid navigation parameters
- Named exports enable better tree-shaking and dead code elimination during bundling
- Consistent export patterns reduce onboarding time for new developers and improve code review efficiency
- Explicit contracts facilitate automated dependency analysis and architectural compliance checking

Negative:
- Additional boilerplate required for index files and type definition files increases initial development time
- Refactoring component names requires updating both the component and its exported contract identifier
- Mixed JavaScript/TypeScript codebase creates inconsistency in export patterns during migration period
- Strict naming conventions may feel restrictive for developers accustomed to flexible export patterns

## Alternatives

- Use default exports for all components and modules instead of named exports (rejected)
  Rejected because: Default exports reduce discoverability, prevent tree-shaking optimization, and make refactoring more error-prone as import names can diverge from actual component names
  When valid: Only valid for configuration files required by third-party tools that mandate default exports
- Adopt barrel exports (index.ts) for all directories without explicit contract naming (rejected)
  Rejected because: Barrel exports without named contracts hide the public API surface and can lead to circular dependencies in React Native applications
  When valid: Valid when combined with explicit named exports to create clear module boundaries
- Generate TypeScript declaration files (.d.ts) automatically from JavaScript implementations (deferred)
  Rejected because: Automatic generation is valuable but does not replace the need for explicit contract definitions; deferred pending TypeScript migration completion
  When valid: Valid as a supplementary tool once full TypeScript migration is complete

## Risks

- Inconsistent adoption across the codebase during migration from JavaScript to TypeScript creates confusion about which export pattern to follow
  Mitigation: Establish clear migration guidelines with examples, use ESLint rules to enforce named exports in new TypeScript files, and prioritize high-traffic modules for migration
  Owner: Engineering team lead
- Over-exporting internal implementation details as public contracts increases coupling and makes future refactoring difficult
  Mitigation: Conduct code reviews focused on API surface area, document public vs private distinction in contributing guidelines, and use TypeScript private/internal JSDoc annotations
  Owner: Code reviewers and architecture team
- Breaking changes to exported contracts impact multiple dependent modules and require coordinated updates
  Mitigation: Implement semantic versioning for internal modules, use deprecation warnings before removing contracts, and maintain a CHANGELOG for public API changes
  Owner: Engineering team

## Implementation Notes

- Start by auditing existing exports using grep for 'export' patterns and documenting current public contracts in an API inventory
- Create ESLint rules to enforce named export conventions: require 'export const ComponentName' for React components, 'export type ParamList' for navigation types, and 'export const useHookName' for custom hooks
- Establish index.ts files in major directories (components/, screens/, navigators/) that re-export public contracts while keeping implementation details private
- For React Navigation integration, ensure all navigator param lists are exported from a central types.ts file per navigator to enable type-safe navigation throughout the app
- Document the contract naming conventions in CONTRIBUTING.md with examples from each category: components, hooks, contexts, slices, and utilities

## Continuation Context


Verify commands:
- grep -r "export const.*:.*React\.FC" template/src/components template/src/screens template/src/navigators --include="*.tsx" --include="*.ts" | wc -l
- grep -r "export type.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l
- grep -r "export const use[A-Z]" template/src --include="*.ts" --include="*.tsx" | wc -l
- find template/src/components template/src/screens -name "index.ts" -o -name "index.tsx" | wc -l

Accept when:
- All React components in src/components, src/screens, and src/navigators export named contracts matching component names
- All React Navigation navigators have corresponding TypeScript ParamList type exports
- All custom hooks follow the 'use' prefix convention and are exported as named exports
- Index files exist in major directories and re-export public contracts without exposing internal implementation details
- ESLint rules pass for named export conventions in TypeScript files

## Enforcement

- Verified by: ESLint rules configured to enforce named export patterns for React components, hooks, and TypeScript types
- Verified by: Code review checklist includes verification of public contract exports for new components and modules
- Verified by: CI pipeline runs grep-based verification commands to count and validate export patterns
- Verified by: TypeScript compiler strict mode enabled to catch missing type exports for navigation param lists
- Violation handling: ESLint violations block PR merge until resolved or explicitly exempted
- Violation handling: Code review identifies missing or inconsistent contract exports and requests changes before approval
- Violation handling: CI pipeline warnings for export pattern violations are reviewed in weekly architecture meetings
- Violation handling: Existing violations in legacy code are tracked in technical debt backlog with prioritized remediation plan
- Exception process: Developer documents exception rationale in code comment with reference to specific policy exception (EXC-001, EXC-002)
- Exception process: Technical lead reviews and approves exception with documented timeline for resolution if temporary
- Exception process: Exception is recorded in architecture decision log with justification and expiration date
- Exception process: Automatic exceptions apply for recognized configuration files (.prettierrc.js, babel.config.js) without additional approval