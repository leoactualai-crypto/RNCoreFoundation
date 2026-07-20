# Standardize Public API Contract Exports for React Native Components and Modules: React Components Intended

These rules are ALWAYS ACTIVE for all React components, hooks, contexts, type definitions, and utility modules in src/components, src/screens, src/navigators, and feature directories intended for external consumption.

### Rules

- **R-API-001** MUST: All React components intended for external consumption MUST export a named contract identifier matching the component name (e.g., TopicDrawerNavigator, OptionsScreen, DemoComponent01).
- **R-API-002** MUST: All React Navigation navigators MUST export corresponding TypeScript ParamList type definitions (e.g., RootStackParamList, MaterialBottomTabParamList).
- **R-API-003** MUST: All custom React hooks MUST follow the 'use' prefix convention and be exported as named exports (e.g., useAuth, useNavigation).
- **R-API-004** MUST: All React Context providers and context objects MUST be exported as named exports (e.g., UserContext, AuthContext).
- **R-API-005** MUST: All Redux Toolkit slice reducers and actions MUST be exported as named exports from their respective slice files.
- **R-API-006** SHOULD: Index files in major directories (components/, screens/, navigators/) SHOULD re-export public contracts while keeping implementation details private.
- **R-API-007** SHOULD: Utility modules, constants, and configuration files in src/ SHOULD export named contracts for public APIs.
- **R-API-008** MAY: Legacy JavaScript files being migrated to TypeScript MAY temporarily use module.exports instead of named exports (EXC-001).
- **R-API-009** MAY: Configuration files required by third-party tools (e.g., .prettierrc.js, babel.config.js) MAY follow tool-specific export conventions (EXC-002).

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
- No internal implementation details are exposed as public contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and CI pipeline validation. Violations block PR merge until resolved or explicitly exempted through the documented exception process.
</enforcement>