# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Screen Components Export

These rules are ALWAYS ACTIVE for all React Native screen components that participate in navigation stacks, navigation stack definitions, custom hooks managing authentication or navigation state, and screen component exports serving as public API contracts.

### Rules

- **R-NAV-001** MUST: Screen components MUST export named functions or constants as public API contracts (e.g., LoginScreen, ChatScreen, AuthStackNavigator).

### Verify

```bash
# Check for TypeScript navigation parameter list exports
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Count screen components with named exports
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Run TypeScript compiler to check for navigation type errors
npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation
```

**Accept when:**
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

<enforcement>
Clause R-NAV-001 verification is mandatory. TypeScript compiler checks during CI build pipeline (tsc --noEmit) MUST pass. Code review MUST verify that new screens define parameter types and export named contracts. Pull requests are blocked if new screens lack proper type definitions or named exports. Claude Code MUST NOT skip or defer verification.
</enforcement>