# Adopt React Native with SoLoader for Android Application Bootstrap: Developer Support Mode

These rules are ALWAYS ACTIVE for Android applications using React Native framework, specifically MainApplication or equivalent application entry point classes that handle native library initialization during application bootstrap.

### Rules

- **R-RN-001** MUST: Call SoLoader.init() in Application.onCreate() method before React Native component initialization completes.
- **R-RN-002** MUST: Implement ReactApplication interface in the main Application class.
- **R-RN-003** MUST: Implement getReactNativeHost() method returning a valid ReactNativeHost configuration.
- **R-RN-004** SHOULD: Developer support mode SHOULD be configurable through getUseDeveloperSupport() method returning BuildConfig.DEBUG or equivalent.
- **R-RN-005** SHOULD: Use PackageList(this).getPackages() for automatic native module discovery unless custom modules require manual registration.
- **R-RN-006** MUST: Ensure SoLoader.init() is called before super.onCreate() completes.
- **R-RN-007** SHOULD: Implement proper error handling and logging around SoLoader initialization for production diagnostics.

### Verify

```bash
# Verify SoLoader.init() is called in onCreate method
grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'onCreate'

# Verify ReactApplication interface implementation
grep -r 'extends.*Application.*implements.*ReactApplication' template/android/app/src/main/java/

# Verify required React Native host methods are implemented
grep -r 'getReactNativeHost\|getUseDeveloperSupport' template/android/app/src/main/java/
```

**Accept when:**
- SoLoader.init() call is present in Application.onCreate() method before React Native component initialization
- MainApplication class extends android.app.Application and implements ReactApplication interface
- getReactNativeHost() and getUseDeveloperSupport() methods are implemented and return valid configurations
- getUseDeveloperSupport() returns BuildConfig.DEBUG or equivalent for developer support mode configuration

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting changes to Application.onCreate() or ReactApplication implementations. Violations MUST trigger CI pipeline failure and code review blocks.
</enforcement>