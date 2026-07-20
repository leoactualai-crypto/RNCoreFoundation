# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Store Configuration Enable

These rules are ALWAYS ACTIVE for Redux store configuration files in `template/src/store/index.ts` and related store setup code that integrates @reduxjs/toolkit and redux-thunk for type-safe state management.

### Rules

- **R-STORE-001** MUST: Export `AppDispatch` type contract derived from `typeof store.dispatch` to enable type-safe dispatch operations across the application.
- **R-STORE-002** MUST: Export `AppThunk` type contract defined as `ThunkAction<ReturnType, RootState, unknown, Action<string>>` to support parameterized async actions.
- **R-STORE-003** MUST: Export the `store` instance with proper TypeScript type annotations from the store configuration module.
- **R-STORE-004** MUST: Use `configureStore` from @reduxjs/toolkit with `devTools` option conditionally set based on `process.env.NODE_ENV !== "production"`.
- **R-STORE-005** MUST: Import and integrate `rootReducer` to maintain centralized reducer composition in store configuration.
- **R-STORE-006** MAY: Store configuration MAY enable Redux DevTools extension conditionally based on NODE_ENV for development debugging.
- **R-STORE-007** SHOULD: Derive type contracts from store instance using `typeof` rather than manual type definitions to automatically synchronize with store configuration changes.
- **R-STORE-008** SHOULD: Document exported contracts in store module comments to guide consumer usage patterns.

### Verify

```bash
# Verify AppDispatch export
grep -r "export.*AppDispatch" template/src/store/index.ts

# Verify AppThunk export
grep -r "export.*AppThunk" template/src/store/index.ts

# Verify NODE_ENV environment-aware configuration
grep -r "process\.env\.NODE_ENV" template/src/store/index.ts

# Verify @reduxjs/toolkit integration
grep -r "@reduxjs/toolkit" template/src/store/index.ts
```

**Accept when:**
- Store module exports `AppDispatch`, `AppThunk`, and `store` instance with TypeScript type annotations
- Store configuration reads `process.env.NODE_ENV` for environment-aware middleware setup
- Integration with @reduxjs/toolkit `configureStore` and redux-thunk is present in store configuration
- Type contracts are derived from store instance rather than manually defined where possible
- TypeScript compilation succeeds without errors related to store type contracts
- Redux DevTools configuration is conditionally enabled based on NODE_ENV

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. TypeScript compiler type checking in CI pipeline, automated grep pattern verification, code review checklists, and unit tests across NODE_ENV values are mandatory. Pull requests MUST be blocked if store module does not export required contracts. All violations result in CI build failure.
</enforcement>