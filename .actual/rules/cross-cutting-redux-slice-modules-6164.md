# Standardize Public API Contract Exports for React Native Components and Modules: Redux Slice Modules

These rules are ALWAYS ACTIVE for Redux Toolkit slice modules, React components in src/components, src/screens, and src/navigators directories, TypeScript type definitions for React Navigation param lists, custom React hooks, React Context providers, utility modules, constants, configuration files in src/, and index files that aggregate and re-export module contracts.

### Rules

- **R-REDUX-001** SHOULD: Redux slice modules SHOULD export the reducer as a named export with '.reducer' suffix (e.g., profileSlice.reducer).

### Verify

```bash
# Count Redux slice reducer exports with .reducer suffix
grep -r "export.*\.reducer" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify Redux slices follow naming convention
grep -r "Slice.*export" template/src --include="*.ts" --include="*.tsx" | grep -E "(profileSlice|authSlice|.*Slice)"

# Check for named exports in Redux modules
grep -r "export const.*Slice" template/src --include="*.ts" --include="*.tsx" | wc -l
```

**Accept when:**
- All Redux Toolkit slice modules export the reducer as a named export with '.reducer' suffix
- Redux slice exports follow consistent naming conventions matching slice names
- No default exports are used for Redux slice reducers
- Index files re-export Redux slice contracts without exposing internal implementation details

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint rules configured to enforce named export patterns for Redux slices MUST pass. Code review checklist MUST include verification of Redux slice contract exports. CI pipeline MUST run verification commands to validate export patterns. TypeScript compiler strict mode MUST be enabled to catch missing type exports.
</enforcement>