# Standardize Module Export Contracts for Public API Surfaces: Design System Modules

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts), theme configuration, state management exports, React components, context providers, and configuration files with exported contracts.

### Rules

- **R-EX-001** SHOULD: Design system modules (colors.ts, spacing.ts, typography.ts, buttons.ts) SHOULD export granular named constants (e.g., black, darkestGray, base, text) rather than single aggregate objects to enable tree-shaking.
- **R-EX-002** MUST: All TypeScript/JavaScript modules in src/ MUST use named exports for public API surfaces except where explicitly exempted (legacy components requiring default exports for dynamic import() or React.lazy() compatibility, or configuration files requiring module.exports for tool compatibility).
- **R-EX-003** SHOULD: React components SHOULD export both the component and its prop types using named exports (e.g., export { ChatScreen }; export type { ChatScreenProps }).
- **R-EX-004** SHOULD: Type-only exports SHOULD use TypeScript's export type syntax for better optimization (e.g., export type { AppDispatch, AppThunk }).
- **R-EX-005** SHOULD: Barrel files (index.ts) SHOULD be used sparingly and only for components/ and screens/ directories where they simplify imports without hiding implementation.
- **R-EX-006** SHOULD: Public API contracts SHOULD be documented in each module's header comment, listing exported names and their intended usage.

### Verify

```bash
# Count default exports in src/ (excluding config files)
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l

# Count named exports in src/
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Run ESLint with import/no-default-export rule
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in src/ directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule import/no-default-export passes for all files in src/ except explicitly allowed configuration files (documented in exceptions.md)

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint rule import/no-default-export MUST be configured in .eslintrc with error level for src/ directory. Code review MUST include verification that new exports follow named export pattern. CI pipeline MUST run verification commands and fail if ratio exceeds threshold or if default exports are detected in non-exempt files.
</enforcement>