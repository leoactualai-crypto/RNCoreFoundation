# Standardize React Navigation with TypeScript Type-Safe Route Contracts: Authentication Aware Navigation

These rules are ALWAYS ACTIVE for all React Native screen components, navigation stack definitions, route parameter type declarations, custom hooks that manage authentication or navigation state, and screen component exports that serve as public API contracts.

### Rules

- **R-NAV-001** SHOULD: Authentication-aware navigation SHOULD integrate with Redux state via useSelector and custom hooks (e.g., useAuth).
- **R-NAV-002** MUST: Define a central navigation types file (e.g., types/navigation.ts) that exports all stack parameter lists (RootStackParamList, AuthStackParamList) for reuse across navigators and screens.
- **R-NAV-003** MUST: Use React Navigation's TypeScript guide to properly type useNavigation and useRoute hooks with stack-specific parameter lists.
- **R-NAV-004** MUST: Export screen components as named exports (not default) to create explicit public API contracts.
- **R-NAV-005** MUST: Integrate navigation type checking into CI pipeline using tsc --noEmit to catch type errors before merge.
- **R-NAV-006** SHOULD: Document navigation structure and parameter contracts in component JSDoc comments or separate architecture documentation.

### Verify

```bash
# Check for exported navigation parameter list types
grep -r "export.*ParamList" template/src --include="*.ts" --include="*.tsx" | wc -l

# Verify React Navigation dependencies are present
grep -r "@react-navigation" template/src/package.json || grep -r "@react-navigation" template/package.json

# Count screen components with named exports
find template/src/screens -name "*.tsx" -exec grep -l "export.*Screen\|export.*Navigator" {} \; | wc -l

# Run TypeScript compiler to check for navigation-related type errors
npx tsc --noEmit --project template/tsconfig.json 2>&1 | grep -i navigation
```

**Accept when:**
- At least one TypeScript navigation parameter list type (e.g., RootStackParamList) is defined and exported
- All screen components in template/src/screens export named functions or constants as public contracts
- TypeScript compilation succeeds without navigation-related type errors
- Navigation dependencies (@react-navigation/native, @react-navigation/stack) are present in package.json

<enforcement>
Verified by: TypeScript compiler checks during CI build pipeline (tsc --noEmit)
Verified by: Code review verification that new screens define parameter types and export named contracts
Verified by: Automated linting rules that enforce named exports for screen components
Violation handling: CI build fails if TypeScript compilation detects navigation type errors
Violation handling: Pull requests are blocked if new screens lack proper type definitions or named exports
Violation handling: Navigation parameter type mismatches are flagged as high-priority bugs requiring immediate fix
Exception process: Exceptions for untyped navigation may be granted for rapid prototyping branches that will not merge to main
Exception process: Technical debt tickets must be created for any temporary type suppressions (e.g., @ts-ignore) in navigation code
Exception process: Architecture review required for alternative navigation patterns that deviate from React Navigation
Claude Code MUST NOT skip or defer verification.
</enforcement>