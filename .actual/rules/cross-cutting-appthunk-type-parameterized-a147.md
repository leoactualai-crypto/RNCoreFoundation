# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Appthunk Type Parameterized

These rules are ALWAYS ACTIVE for Redux store configuration files, particularly `template/src/store/index.ts`, and all code that integrates with the Redux store, dispatch operations, and thunk creators.

### Rules

- **R-REDUX-001** SHOULD: AppThunk type SHOULD be parameterized to accept return type and argument types for flexible async action creators.
- **R-REDUX-002** MUST: Export AppDispatch as `typeof store.dispatch` to automatically synchronize with store configuration changes.
- **R-REDUX-003** MUST: Define AppThunk as `ThunkAction<ReturnType, RootState, unknown, Action<string>>` to support parameterized async actions.
- **R-REDUX-004** MUST: Use configureStore from @reduxjs/toolkit with devTools option set to `process.env.NODE_ENV !== "production"`.
- **R-REDUX-005** MUST: Import and integrate rootReducer to maintain centralized reducer composition.
- **R-REDUX-006** MUST: Export store instance, AppDispatch, and AppThunk contracts from the store module with TypeScript type annotations.
- **R-REDUX-007** SHOULD: Derive types from store instance using `typeof` rather than manual type definitions to prevent type drift.

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
- AppThunk type is parameterized to accept return type and argument types
- All grep verification commands return matches in the store module

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler type checking in CI pipeline is mandatory. Pull requests MUST be blocked if store module does not export required contracts (AppDispatch, AppThunk, store). All violations result in CI build failure.
</enforcement>