# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Interaction State Managed

These rules are ALWAYS ACTIVE for all React Native screen components that participate in navigation stacks, navigation stack definitions and route parameter type declarations, custom hooks that manage authentication or navigation state, and screen component exports that serve as public API contracts.

### Rules

- **R-NAV-001** SHOULD: UI interaction state SHOULD be managed using React hooks (useState, useEffect, useMemo, useCallback, useSelector) rather than class component lifecycle methods.
- **R-NAV-002** MUST: Define a central navigation types file (e.g., types/navigation.ts) that exports all stack parameter lists (RootStackParamList, AuthStackParamList) for reuse across navigators and screens.
- **R-NAV-003** MUST: Use React Navigation's TypeScript support to properly type useNavigation and useRoute hooks with stack-specific parameter lists.
- **R-NAV-004** MUST: Export screen components as named exports (not default) to create explicit public API contracts.
- **R-NAV-005** MUST: Ensure TypeScript compilation succeeds without navigation-related type errors via tsc --noEmit.

### Verify

```bash
# Verify navigation parameter list types are defined and exported
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Verify screen components export named contracts
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Verify TypeScript compilation succeeds
npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation
```

**Accept when:**
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation checks MUST pass during CI build pipeline. Code review MUST verify that new screens define parameter types and export named contracts. Navigation parameter type mismatches MUST be flagged as high-priority bugs requiring immediate fix.
</enforcement>