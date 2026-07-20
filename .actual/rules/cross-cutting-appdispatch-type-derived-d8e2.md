# Export Typed Redux Store Contracts for Type-Safe Dispatch and Thunk Integration: Appdispatch Type Derived

These rules are ALWAYS ACTIVE for Redux store configuration files, particularly `template/src/store/index.ts`, and any code that integrates with the Redux store, dispatch operations, and thunk creators.

### Rules

- **R-REDUX-001** SHOULD: AppDispatch type SHOULD be derived from `typeof store.dispatch` to ensure dispatch signatures remain synchronized with store configuration.

### Verify

```bash
# Verify AppDispatch is exported
grep -r "export.*AppDispatch" template/src/store/index.ts

# Verify AppThunk is exported
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

<enforcement>
Verification via TypeScript compiler type checking and automated grep patterns is mandatory. CI build MUST fail if TypeScript compilation errors occur due to missing or incorrect type contracts. Pull requests MUST be blocked if store module does not export required contracts (AppDispatch, AppThunk, store).
</enforcement>