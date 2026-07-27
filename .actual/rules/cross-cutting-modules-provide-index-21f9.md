# Standardize Module Export Contracts for Public API Surfaces: Modules Provide Index

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules, theme configuration, state management exports, React components, context providers, and configuration files with exported contracts.

### Rules

- **R-MOD-001** MAY: Modules MAY provide `index.ts` barrel files to re-export public contracts from subdirectories (e.g., `components/CreateAction/index.ts`).
- **R-MOD-002** MUST: Design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) MUST use only named exports.
- **R-MOD-003** MUST: State management exports (store/index.ts) MUST export store, AppDispatch, and AppThunk as named exports.
- **R-MOD-004** SHOULD: React components in screens/ and components/ directories SHOULD export both the component and its prop types using named exports.
- **R-MOD-005** SHOULD: Modules SHOULD use TypeScript's `export type` syntax for type-only exports to enable better optimization.
- **R-MOD-006** MUST: Configuration files (.prettierrc.js, babel.config.js) that require tool compatibility MAY use `module.exports` as an exception to the named export standard.
- **R-MOD-007** MUST: React components requiring default exports for compatibility with existing `dynamic import()` or `React.lazy()` usage MAY use default exports as documented exceptions.

### Verify

```bash
# Count default exports in src/ (excluding config files)
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l

# Count named exports in src/
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Run ESLint rule for no default exports
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx

# Verify design system modules use only named exports
grep -l 'export default' src/theme/colors.ts src/theme/fonts.ts src/theme/palette.ts src/theme/spacing.ts src/theme/typography.ts src/theme/buttons.ts 2>/dev/null | wc -l
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in `src/` directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule `import/no-default-export` passes for all files in `src/` except explicitly allowed configuration files
- State management exports in store/index.ts export store, AppDispatch, and AppThunk as named exports

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST pass before accepting code that modifies module exports. Violations detected by ESLint MUST be resolved or documented as exceptions with tech lead approval.
</enforcement>