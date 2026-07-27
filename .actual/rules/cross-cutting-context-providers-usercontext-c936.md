# Standardize Module Export Contracts for Public API Surfaces: Context Providers Usercontext

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules, theme configuration, state management exports, React components, context providers, and configuration files with exported contracts.

### Rules

- **R-EXP-001** SHOULD: Context providers (UserContext, AuthContext) SHOULD be exported as named exports to maintain consistency with other public API patterns.
- **R-EXP-002** SHOULD: Design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) SHOULD use only named exports for public API surfaces.
- **R-EXP-003** SHOULD: State management exports (store/index.ts) SHOULD export store, AppDispatch, and AppThunk as named exports.
- **R-EXP-004** SHOULD: React components in screens/ and components/ directories SHOULD export both the component and its prop types as named exports.
- **R-EXP-005** MAY: Legacy components MAY use default exports for compatibility with existing dynamic import() statements or React.lazy() usage (EXC-001).
- **R-EXP-006** MAY: Configuration files (.prettierrc.js, babel.config.js) MAY use module.exports for tool compatibility (EXC-002).

### Verify

```bash
# Count default exports in src/ (excluding config files)
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l

# Count named exports in src/
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Run ESLint rule to detect default exports
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx

# Verify design system modules use only named exports
grep -l 'export default' src/colors.ts src/fonts.ts src/palette.ts src/spacing.ts src/typography.ts src/buttons.ts 2>/dev/null | wc -l
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in src/ directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule import/no-default-export passes for all files in src/ except explicitly allowed configuration files
- Context providers (UserContext, AuthContext) are exported as named exports

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint rule import/no-default-export must be configured in .eslintrc with error level for src/ directory. CI pipeline must run verification commands and fail if ratio exceeds threshold or if default exports are detected in non-exempt files.
</enforcement>