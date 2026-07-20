# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Store Configuration Read

These rules are ALWAYS ACTIVE for Redux store configuration files in `template/src/store/index.ts` and related store setup code that exports type contracts for dispatch and thunk integration.

### Rules

- **R-STORE-001** MUST: Store configuration MUST read `process.env.NODE_ENV` to conditionally configure middleware and development tools based on runtime environment.
- **R-STORE-002** MUST: Export `AppDispatch` type contract derived from `typeof store.dispatch` to enable type-safe dispatch operations.
- **R-STORE-003** MUST: Export `AppThunk` type contract defined as `ThunkAction<ReturnType, RootState, unknown, Action<string>>` to support parameterized async actions.
- **R-STORE-004** MUST: Export the store instance with TypeScript type annotations from the store configuration module.
- **R-STORE-005** MUST: Use `configureStore` from `@reduxjs/toolkit` with `devTools` option set to `process.env.NODE_ENV !== "production"`.
- **R-STORE-006** MUST: Integrate `rootReducer` to maintain centralized reducer composition in store configuration.
- **R-STORE-007** SHOULD: Derive type contracts from store instance using `typeof` rather than manual type definitions to automatically synchronize with store configuration changes.
- **R-STORE-008** SHOULD: Document exported contracts in store module comments to guide consumer usage patterns.

### Verify

```bash
# Verify AppDispatch export
grep -r "export.*AppDispatch" template/src/store/index.ts

# Verify AppThunk export
grep -r "export.*AppThunk" template/src/store/index.ts

# Verify NODE_ENV environment check
grep -r "process\.env\.NODE_ENV" template/src/store/index.ts

# Verify @reduxjs/toolkit integration
grep -r "@reduxjs/toolkit" template/src/store/index.ts
```

**Accept when:**
- Store module exports `AppDispatch`, `AppThunk`, and store instance with TypeScript type annotations
- Store configuration reads `process.env.NODE_ENV` for environment-aware middleware setup
- Integration with `@reduxjs/toolkit` `configureStore` and `redux-thunk` is present in store configuration
- Type contracts are derived from store instance rather than manually defined where possible
- `devTools` option is conditionally set based on `NODE_ENV` value
- `rootReducer` is imported and integrated into store configuration

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler type checking in CI pipeline, automated grep patterns, code review checklists, and unit tests across NODE_ENV values are mandatory. CI build fails if TypeScript compilation errors occur or required contracts are missing. Pull requests are blocked if store module does not export required contracts.
</enforcement>