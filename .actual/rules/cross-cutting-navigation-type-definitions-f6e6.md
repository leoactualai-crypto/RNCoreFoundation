# Standardize Public API Contract Exports for React Native Components and Modules: Navigation Type Definitions

These rules are ALWAYS ACTIVE for React components in src/components, src/screens, and src/navigators directories; TypeScript type definitions for React Navigation param lists; custom React hooks in src/components and feature directories; React Context providers and context objects; Redux Toolkit slice reducers and actions; utility modules, constants, and configuration files in src/; and index files that aggregate and re-export module contracts.

### Rules

- **R-NAV-001** MUST: Navigation type definitions MUST be exported as named TypeScript types with ParamList suffix (e.g., RootStackParamList, MaterialBottomTabParamList, TopicDrawerStackParamList).
- **R-NAV-002** MUST: All React components in src/components, src/screens, and src/navigators MUST export named contracts matching component names using `export const ComponentName`.
- **R-NAV-003** MUST: All custom hooks MUST follow the 'use' prefix convention and be exported as named exports (e.g., `export const useAuth`).
- **R-NAV-004** MUST: React Navigation navigators MUST have corresponding TypeScript ParamList type exports from a central types.ts file per navigator.
- **R-NAV-005** SHOULD: Index files in major directories (components/, screens/, navigators/) SHOULD re-export public contracts while keeping implementation details private.
- **R-NAV-006** SHOULD: React Context providers and context objects SHOULD be exported as named exports with clear naming conventions (e.g., UserContext, AuthContext).
- **R-NAV-007** SHOULD: Redux Toolkit slice reducers and actions SHOULD be exported as named exports (e.g., profileSlice.reducer).

### Verify

```bash
# Count React components with named exports
grep -r "export const.*:.*React\.FC" template/src/components template/src/screens template/src/navigators --include="*.tsx" --include="*.ts" | wc -l

# Count navigation ParamList type exports
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
- TypeScript compiler strict mode enabled catches missing type exports for navigation param lists

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations block PR merge until resolved or explicitly exempted. Code review identifies missing or inconsistent contract exports and requests changes before approval. CI pipeline warnings for export pattern violations are reviewed in weekly architecture meetings.
</enforcement>