# Standardize Public API Contract Exports for React Native Components and Modules: Index Files Export

These rules are ALWAYS ACTIVE for all React components, navigators, screens, custom hooks, context providers, Redux slices, utilities, and index files in the React Native application that establish public API contracts.

### Rules

- **R-API-001** SHOULD: Index files (index.ts, index.js) SHOULD re-export public contracts from their directory to establish clear module boundaries.
- **R-API-002** SHOULD: React components in src/components, src/screens, and src/navigators directories SHOULD use named exports matching component names.
- **R-API-003** SHOULD: TypeScript type definitions for React Navigation param lists SHOULD be exported as named exports from a central types.ts file per navigator.
- **R-API-004** SHOULD: Custom React hooks SHOULD follow the 'use' prefix convention and be exported as named exports.
- **R-API-005** SHOULD: React Context providers and context objects SHOULD be exported as named exports from their defining modules.
- **R-API-006** SHOULD: Redux Toolkit slice reducers and actions SHOULD be exported as named exports.
- **R-API-007** SHOULD: Utility modules, constants, and configuration files SHOULD export public contracts as named exports.
- **R-API-008** MUST NOT: Internal implementation details not intended for external consumption MUST NOT be exported from index files.
- **R-API-009** MUST NOT: Private helper functions and utilities within component files MUST NOT be re-exported from index files.

### Verify

```bash
# Count React components with named exports
grep -r "export const.*:.*React\.FC" template/src/components template/src/screens template/src/navigators --include="*.tsx" --include="*.ts" | wc -l

# Count React Navigation ParamList type exports
grep -r "export type.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Count custom hooks with named exports
grep -r "export const use[A-Z]" template/src --include="*.ts" --include="*.tsx" | wc -l

# Count index files in major directories
find template/src/components template/src/screens -name "index.ts" -o -name "index.tsx" | wc -l
```

**Accept when:**
- All React components in src/components, src/screens, and src/navigators export named contracts matching component names
- All React Navigation navigators have corresponding TypeScript ParamList type exports
- All custom hooks follow the 'use' prefix convention and are exported as named exports
- Index files exist in major directories and re-export public contracts without exposing internal implementation details
- ESLint rules pass for named export conventions in TypeScript files
- No private helper functions or internal utilities are re-exported from index files

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST be executed and pass before accepting changes to files in scope. Code review MUST include verification of public contract exports for new components and modules. ESLint violations MUST block PR merge until resolved or explicitly exempted with documented exception rationale.
</enforcement>