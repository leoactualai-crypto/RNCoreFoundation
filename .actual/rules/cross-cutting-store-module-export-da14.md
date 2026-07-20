# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Store Module Export

These rules are ALWAYS ACTIVE for all Redux store configuration files, particularly `template/src/store/index.ts`, to ensure type-safe dispatch operations, thunk integration, and environment-aware middleware setup.

### Rules

- **R-STORE-001** MUST: The store module MUST export typed contracts including AppDispatch, AppThunk, and the store instance to enable type-safe integration across the application.
- **R-STORE-002** MUST: Export AppDispatch as `typeof store.dispatch` to automatically synchronize with store configuration changes.
- **R-STORE-003** MUST: Define AppThunk as `ThunkAction<ReturnType, RootState, unknown, Action<string>>` to support parameterized async actions.
- **R-STORE-004** MUST: Use `configureStore` from @reduxjs/toolkit with devTools option set to `process.env.NODE_ENV !== "production"`.
- **R-STORE-005** MUST: Import and integrate rootReducer to maintain centralized reducer composition.
- **R-STORE-006** SHOULD: Derive types from store instance using `typeof` rather than manual type definitions to prevent type drift.
- **R-STORE-007** SHOULD: Document exported contracts in store module comments to guide consumer usage patterns.

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
- TypeScript compilation succeeds without errors related to store type contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. All grep patterns and TypeScript type checks MUST pass before accepting store module changes. Pull requests MUST be blocked if required contracts are missing or incorrectly typed.
</enforcement>