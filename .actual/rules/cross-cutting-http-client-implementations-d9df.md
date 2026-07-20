# Standardize React Native with React Navigation and Material Design Components: Http Client Implementations

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, and HTTP client configurations within the project.

### Rules

- **R-HTTP-001** SHOULD: HTTP client implementations SHOULD use 'axios' for API communication and request/response handling.

### Verify

```bash
# Verify axios is present in HTTP client implementations
grep -r "axios" template/src --include='*.tsx' --include='*.ts' | grep -E "(import|require)" | wc -l

# Verify no alternative HTTP clients are used
grep -r "fetch\|XMLHttpRequest\|got\|node-fetch" template/src --include='*.tsx' --include='*.ts' | grep -v "node_modules" | wc -l

# Verify axios configuration exists
find template/src -name '*axios*' -o -name '*http*' -o -name '*api*' | grep -E "\.(ts|tsx)$" | wc -l

# Verify package.json includes axios dependency
grep -r "axios" template/src/package.json
```

**Accept when:**
- axios is imported and used in HTTP client modules for API communication
- No alternative HTTP client libraries (fetch, XMLHttpRequest, got, node-fetch) are used for API calls
- axios is listed as a dependency in package.json
- axios base URL and interceptors are configured in a centralized configuration module
- All API requests use the centralized axios instance rather than creating new instances

<enforcement>
Claude Code MUST NOT skip or defer verification of axios usage in HTTP client implementations. All HTTP communication MUST route through axios as the standardized client library.
</enforcement>