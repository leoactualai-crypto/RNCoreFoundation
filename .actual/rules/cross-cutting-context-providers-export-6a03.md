# Standardize Public API Contract Exports for React Native Components and Modules: Context Providers Export

These rules are ALWAYS ACTIVE for React components in src/components, src/screens, and src/navigators directories; TypeScript type definitions for React Navigation param lists; custom React hooks in src/components and feature directories; React Context providers and context objects; Redux Toolkit slice reducers and actions; utility modules, constants, and configuration files in src/; and index files that aggregate and re-export module contracts.

### Rules

- **R-CTX-001** MUST: Context providers MUST export named context objects with 'Context' suffix (e.g., UserContext, AuthContext).

### Verify

```bash
# Count exported context providers with Context suffix
grep -r "export const.*Context" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify no default exports for context providers
grep -r "export default.*Context" template/src --include="*.ts" --include="*.tsx" | wc -l

# List all context provider exports for manual review
grep -r "export.*Context" template/src --include="*.ts" --include="*.tsx"
```

**Accept when:**
- All React Context providers export named context objects with 'Context' suffix
- No context providers use default exports
- Context objects are consistently named and discoverable via IDE autocomplete
- Index files re-export context providers without exposing internal implementation details

<enforcement>
Claude Code MUST NOT skip or defer verification of context provider export naming conventions. All context providers must follow the 'Context' suffix pattern before code review approval.
</enforcement>