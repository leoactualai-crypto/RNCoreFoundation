# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Screens Use React

These rules are ALWAYS ACTIVE for all React Native screen components, navigation stack definitions, route parameter type declarations, custom hooks managing authentication or navigation state, and screen component exports that serve as public API contracts.

### Rules

- **R-NAV-001** MUST: Define a central navigation types file (e.g., types/navigation.ts) that exports all stack parameter lists (RootStackParamList, AuthStackParamList) for reuse across navigators and screens.
- **R-NAV-002** MUST: Use React Navigation's TypeScript support to properly type useNavigation and useRoute hooks with stack-specific parameter lists.
- **R-NAV-003** MUST: Export screen components as named exports (not default) to create explicit public API contracts that are easier to track and refactor.
- **R-NAV-004** MUST: Integrate navigation type checking into CI pipeline using tsc --noEmit to catch type errors before merge.
- **R-NAV-005** SHOULD: Document navigation structure and parameter contracts in component JSDoc comments or separate architecture documentation.
- **R-NAV-006** MAY: Screens MAY use react-native-paper components for Material Design consistency or other UI libraries as appropriate.

### Verify

```bash
# Verify navigation parameter lists are defined and exported
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Verify screen components export named contracts
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Verify TypeScript compilation succeeds without navigation errors
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