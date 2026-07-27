# Standardize Module Export Contracts for Public API Surfaces: Component Modules That

These rules are ALWAYS ACTIVE for all TypeScript/JavaScript modules in `src/` that export functionality consumed by other modules, including design system modules, theme configuration, state management exports, React components, context providers, and configuration files with exported contracts.

### Rules

- **R-EXP-001** MUST: Component modules that serve as screen entry points MUST export a single named component matching the file purpose (e.g., ChatScreen, LoginScreen, SplashScreen).
- **R-EXP-002** MUST: Design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) MUST use only named exports for design tokens and configuration objects.
- **R-EXP-003** MUST: State management exports from store/index.ts MUST include named exports for store, AppDispatch, and AppThunk.
- **R-EXP-004** MUST: React components in screens/ and components/ directories that serve as public interfaces MUST export both the component and its prop types using named exports.
- **R-EXP-005** SHOULD: Use barrel files (index.ts) sparingly and only for components/ and components/ directories where they simplify imports without hiding implementation.
- **R-EXP-006** SHOULD: Document public API contracts in each module's header comment, listing exported names and their intended usage.
- **R-EXP-007** MAY: Legacy components MAY use default exports for compatibility with existing dynamic import() statements or React.lazy() usage (EXC-001).
- **R-EXP-008** MAY: Configuration files (e.g., .prettierrc.js, babel.config.js) MAY use module.exports for tool compatibility (EXC-002).

### Verify

```bash
# Count default exports in src/ (excluding config files)
grep -r 'export default' src/ --include='*.ts' --include='*.tsx' --exclude='*.config.*' | wc -l

# Count named exports in src/
grep -r 'export {\|export const\|export function\|export type' src/ --include='*.ts' --include='*.tsx' | wc -l

# Run ESLint rule for no default exports
eslint src/ --rule 'import/no-default-export: error' --ext .ts,.tsx

# Verify design system modules use only named exports
grep -l 'export default' src/colors.ts src/fonts.ts src/palette.ts src/spacing.ts src/typography.ts src/buttons.ts 2>/dev/null | wc -l
```

**Accept when:**
- Named exports outnumber default exports by at least 10:1 ratio in src/ directory (excluding configuration files)
- All design system modules (colors.ts, fonts.ts, palette.ts, spacing.ts, typography.ts, buttons.ts) use only named exports
- ESLint rule import/no-default-export passes for all files in src/ except explicitly allowed configuration files
- All screen entry point components export a single named component matching their file purpose

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint rule import/no-default-export MUST be configured in .eslintrc with error level for src/ directory. Code review MUST include verification that new exports follow named export pattern. CI pipeline MUST run verification commands and fail if ratio exceeds threshold or default exports are detected in non-exempt files.
</enforcement>