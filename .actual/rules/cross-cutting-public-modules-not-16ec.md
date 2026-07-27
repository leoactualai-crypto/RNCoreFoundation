# Standardize Module Export Contracts for Public API Surfaces: Public Modules Not

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules, theme configuration, state management exports, React components, context providers, and configuration files with exported contracts.

### Rules

- **R-EXPORT-001** MUST NOT: Public API modules MUST NOT export internal implementation details, helper functions, or intermediate computation results unless explicitly intended for reuse.

### Verify

```bash
# Count default exports in src/ (excluding config files)
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l

# Count named exports in src/
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Run ESLint rule for default exports
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in `src/` directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule `import/no-default-export` passes for all files in `src/` except explicitly allowed configuration files (per EXC-001 and EXC-002)

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint rule `import/no-default-export` must be configured in `.eslintrc` with error level for `src/` directory. Code review must verify that new exports follow named export pattern. CI pipeline must run verification commands and fail if ratio exceeds threshold or ESLint detects violations in non-exempt files.
</enforcement>