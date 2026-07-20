# Adopt Redux Toolkit with Thunk Middleware for State Management with Environment-Aware Configuration: Store Configuration Access

These rules are ALWAYS ACTIVE for all Redux store configuration files and state management implementations across the application.

### Rules

- **R-REDUX-001** MUST: Store configuration MUST access runtime environment through process.env.NODE_ENV for environment-specific behavior

### Verify

```bash
# Verify Redux Toolkit and redux-thunk are imported in store configuration
grep -r "@reduxjs/toolkit" template/src/store/ && grep -r "redux-thunk" template/src/store/

# Verify process.env.NODE_ENV is accessed in store initialization
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
Claude Code MUST NOT skip or defer verification. All Redux store configuration files MUST comply with R-REDUX-001 before acceptance.
</enforcement>