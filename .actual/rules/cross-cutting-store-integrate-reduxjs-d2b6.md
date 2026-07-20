# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Store Integrate Reduxjs

These rules are ALWAYS ACTIVE for Redux store configuration files, particularly `template/src/store/index.ts`, to ensure type-safe dispatch operations, thunk integration, and environment-aware middleware setup.

### Rules

- **R-STORE-001** MUST: The store MUST integrate @reduxjs/toolkit configureStore with redux-thunk middleware for asynchronous action support.
- **R-STORE-002** MUST: Export AppDispatch as `typeof store.dispatch` to automatically synchronize with store configuration changes.
- **R-STORE-003** MUST: Define and export AppThunk as `ThunkAction<ReturnType, RootState, unknown, Action<string>>` to support parameterized async actions.
- **R-STORE-004** MUST: Use configureStore from @reduxjs/toolkit with devTools option set to `process.env.NODE_ENV !== "production"`.
- **R-STORE-005** MUST: Import and integrate rootReducer to maintain centralized reducer composition.
- **R-STORE-006** MUST: Export the store instance with TypeScript type annotations.
- **R-STORE-007** SHOULD: Derive types from store instance using `typeof` rather than manual type definitions to prevent type drift.
- **R-STORE-008** SHOULD: Document exported contracts in store module comments to guide consumer usage patterns.

### Verify

```bash
# Verify AppDispatch export
grep -r "export.*AppDispatch" template/src/store/index.ts

# Verify AppThunk export
grep -r "export.*AppThunk" template/src/store/index.ts

# Verify environment-aware configuration
grep -r "process\.env\.NODE_ENV" template/src/store/index.ts

# Verify @reduxjs/toolkit integration
grep -r "@reduxjs/toolkit" template/src/store/index.ts
```

**Accept when:**
- Store module exports AppDispatch, AppThunk, and store instance with TypeScript type annotations
- Store configuration reads process.env.NODE_ENV for environment-aware middleware setup
- Integration with @reduxjs/toolkit configureStore and redux-thunk is present in store configuration
- Type contracts are derived from store instance rather than manually defined where possible
- All grep verification commands return matches in template/src/store/index.ts

<enforcement>
Clause Code MUST NOT skip or defer verification. TypeScript compiler type checking in CI pipeline is mandatory. Pull requests MUST be blocked if store module does not export required contracts (AppDispatch, AppThunk, store). CI build MUST fail if TypeScript compilation errors occur due to missing or incorrect type contracts.
</enforcement>