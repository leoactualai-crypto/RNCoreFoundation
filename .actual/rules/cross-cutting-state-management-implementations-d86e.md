# Adopt Redux Toolkit with Thunk Middleware for State Management with Environment-Aware Configuration: State Management Implementations

These rules are ALWAYS ACTIVE for all state management implementations, store configuration files, and Redux-based state logic throughout the application.

### Rules

- **R-STATE-001** MUST: State management implementations MUST use @reduxjs/toolkit as the primary Redux abstraction layer.
- **R-STATE-002** MUST: Store initialization MUST occur in template/src/store/index.ts using configureStore from @reduxjs/toolkit with middleware configuration including thunk.
- **R-STATE-003** MUST: Store module MUST export store instance, RootState type (derived from store.getState), AppDispatch type (derived from store.dispatch), and AppThunk type for typed thunk actions.
- **R-STATE-004** MUST: Access to process.env.NODE_ENV MUST occur only during store configuration for environment-specific middleware or DevTools setup, not within reducer logic.
- **R-STATE-005** SHOULD: Organize reducers by feature domain in separate files, then compose them in rootReducer using combineReducers or Redux Toolkit's slice pattern.
- **R-STATE-006** SHOULD: Establish reducer splitting guidelines and implement code splitting for lazy-loaded feature reducers to prevent rootReducer composition complexity from growing unbounded.

### Verify

```bash
# Verify Redux Toolkit and redux-thunk imports in store configuration
grep -r "@reduxjs/toolkit" template/src/store/ && grep -r "redux-thunk" template/src/store/

# Verify environment-aware configuration
grep -r "process.env.NODE_ENV" template/src/store/index.ts

# Verify typed exports
grep -E "export.*(AppDispatch|AppThunk|store)" template/src/store/index.ts

# Verify rootReducer composition pattern
grep -r "rootReducer" template/src/store/
```

**Accept when:**
- All store configuration files import and use @reduxjs/toolkit's configureStore and redux-thunk middleware
- Store initialization accesses process.env.NODE_ENV for environment-aware configuration
- Store module exports include typed AppDispatch, AppThunk, and store contracts
- RootReducer composition pattern aggregates domain-specific reducers into unified store
- TypeScript compilation succeeds with strict type checking for all dispatch operations
- Automated tests validate store behavior across different NODE_ENV values

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for @reduxjs/toolkit and redux-thunk imports is mandatory. TypeScript compilation enforcing AppDispatch and AppThunk type contract usage is mandatory. Code review must verify rootReducer composition and environment variable access patterns. Automated tests validating store behavior across NODE_ENV values are mandatory.
</enforcement>