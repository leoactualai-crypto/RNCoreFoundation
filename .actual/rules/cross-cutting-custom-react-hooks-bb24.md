# Standardize Public API Contract Exports for React Native Components and Modules: Custom React Hooks

These rules are ALWAYS ACTIVE for custom React hooks in src/components, src/screens, and feature directories, as well as React components, TypeScript type definitions, React Context providers, Redux Toolkit slices, utility modules, and index files that aggregate module contracts.

### Rules

- **R-API-001** MUST: Custom React hooks MUST export a named contract using the 'use' prefix convention (e.g., useAuth).
- **R-API-002** MUST: React components in src/components, src/screens, and src/navigators directories MUST export named contracts matching component names.
- **R-API-003** MUST: React Navigation navigators MUST have corresponding TypeScript ParamList type exports.
- **R-API-004** MUST: React Context providers and context objects MUST be exported as named exports.
- **R-API-005** MUST: Redux Toolkit slice reducers and actions MUST be exported as named exports.
- **R-API-006** SHOULD: Index files in major directories (components/, screens/, navigators/) SHOULD re-export public contracts while keeping implementation details private.
- **R-API-007** MAY: Legacy JavaScript files being migrated to TypeScript MAY temporarily use module.exports instead of named exports (EXC-001).
- **R-API-008** MAY: Configuration files required by third-party tools (e.g., .prettierrc.js, babel.config.js) MAY follow tool-specific export conventions (EXC-002).

### Verify

```bash
# Count React components with named exports
grep -r "export const.*:.*React\.FC" template/src/components template/src/screens template/src/navigators --include="*.tsx" --include="*.ts" | wc -l

# Count React Navigation ParamList type exports
grep -r "export type.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Count custom hooks with 'use' prefix convention
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
- TypeScript compiler strict mode is enabled to catch missing type exports for navigation param lists

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations block PR merge until resolved or explicitly exempted with documented exception rationale. Code review must verify public contract exports for all new components and modules. CI pipeline must run verification commands and flag export pattern violations for weekly architecture review.
</enforcement>