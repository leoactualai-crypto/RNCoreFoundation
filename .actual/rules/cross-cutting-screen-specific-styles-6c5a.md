# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Screen Specific Styles

These rules are ALWAYS ACTIVE for all React Native screen components that participate in navigation stacks, navigation stack definitions, custom hooks managing authentication or navigation state, and screen component exports serving as public API contracts.

### Rules

- **R-NAV-001** SHOULD: Screen-specific styles SHOULD be defined using StyleSheet.create with typed style objects.

### Verify

```bash
# Verify TypeScript navigation parameter lists are defined and exported
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Verify screen components export named functions or constants
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Verify TypeScript compilation succeeds without navigation-related type errors
npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation
```

**Accept when:**
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler checks during CI build pipeline (tsc --noEmit) are mandatory. Navigation parameter type mismatches must be flagged as violations blocking pull requests.
</enforcement>