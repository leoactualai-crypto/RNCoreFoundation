# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Navigation Stacks Define

These rules are ALWAYS ACTIVE for all React Native screen components, navigation stack definitions, route parameter type declarations, custom hooks that manage authentication or navigation state, and screen component exports that serve as public API contracts.

### Rules

- **R-NAV-001** MUST: All navigation stacks MUST define TypeScript parameter list types (e.g., RootStackParamList) that declare screen names and their parameter shapes.

### Verify

```bash
# Check for exported navigation parameter list types
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Count screen components with named exports
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Run TypeScript compiler to catch navigation type errors
npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation
```

**Accept when:**
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation checks during CI build pipeline (tsc --noEmit) are mandatory. Code review verification that new screens define parameter types and export named contracts is required. Navigation parameter type mismatches must be flagged as violations.
</enforcement>