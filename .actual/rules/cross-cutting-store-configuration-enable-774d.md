# Adopt Redux Toolkit with Thunk Middleware for State Management with Environment-Aware Configuration: Store Configuration Enable

These rules are ALWAYS ACTIVE for all Redux store configuration files in `template/src/store/` and related state management code.

### Rules

- **R-REDUX-001** MAY: Store configuration MAY enable Redux DevTools integration in non-production environments based on NODE_ENV checks

### Verify

```bash
# Verify Redux Toolkit and redux-thunk are imported in store configuration
grep -r "@reduxjs/toolkit" template/src/store/ && grep -r "redux-thunk" template/src/store/

# Verify NODE_ENV is accessed for environment-aware configuration
grep -r "process.env.NODE_ENV" template/src/store/index.ts

# Verify typed exports are present
grep -E "export.*(AppDispatch|AppThunk|store)" template/src/store/index.ts

# Verify rootReducer composition pattern
grep -r "rootReducer" template/src/store/
```

**Accept when:**
- All store configuration files import and use @reduxjs/toolkit's configureStore and redux-thunk middleware
- Store initialization accesses process.env.NODE_ENV for environment-aware configuration
- Store module exports include typed AppDispatch, AppThunk, and store contracts
- RootReducer composition pattern aggregates domain-specific reducers into unified store

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for @reduxjs/toolkit and redux-thunk imports is mandatory. TypeScript compilation enforcing AppDispatch and AppThunk type contract usage is mandatory. Code review checklist verification of rootReducer composition and environment variable access patterns is mandatory. Automated tests validating store behavior across different NODE_ENV values are mandatory.
</enforcement>