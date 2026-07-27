# Standardize Module Export Contracts for Public API Surfaces: Configuration Modules Store

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules, theme configuration, state management exports, React components, and context providers.

### Rules

- **R-EXP-001** MUST: Configuration modules (store, themes, palette, colors, fonts) MUST export stable named contracts that can be imported by multiple consumers without breaking changes.
- **R-EXP-002** MUST: Design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) MUST use only named exports.
- **R-EXP-003** MUST: State management exports from store/index.ts MUST include named exports for store, AppDispatch, and AppThunk.
- **R-EXP-004** MUST: React components in screens/ and components/ directories MUST export both the component and its prop types using named exports.
- **R-EXP-005** SHOULD: Use TypeScript's export type syntax for type-only exports to enable better optimization.
- **R-EXP-006** SHOULD: Document public API contracts in each module's header comment, listing exported names and their intended usage.
- **R-EXP-007** MAY: Use barrel files (index.ts) sparingly and only for components/ and screens/ directories where they simplify imports without hiding implementation.
- **R-EXP-008** MUST: Configuration files (.prettierrc.js, babel.config.js) MAY use module.exports for tool compatibility as an exception to the named export requirement.
- **R-EXP-009** MUST: Legacy components requiring default exports for compatibility with dynamic import() statements or React.lazy() usage MUST document the exception with reference to EXC-001.

### Verify

```bash
# Count default exports vs named exports
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Verify ESLint rule enforcement
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx

# Verify design system modules use only named exports
grep -l 'export default' src/colors.ts src/fonts.ts src/palette.ts src/spacing.ts src/typography.ts src/buttons.ts 2>/dev/null | wc -l

# Verify store exports
grep 'export.*store\|export.*AppDispatch\|export.*AppThunk' src/store/index.ts
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in src/ directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule import/no-default-export passes for all files in src/ except explicitly allowed configuration files
- State management exports from store/index.ts include named exports for store, AppDispatch, and AppThunk
- React component exports in screens/ and components/ include both component and prop type exports

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint rule import/no-default-export MUST be configured in .eslintrc with error level for src/ directory. Code review MUST verify that new exports follow the named export pattern. CI pipeline MUST run verification commands and fail if default exports exceed the threshold or if design system modules violate the rule.
</enforcement>