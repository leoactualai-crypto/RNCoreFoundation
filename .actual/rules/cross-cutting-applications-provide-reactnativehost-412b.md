# Adopt React Native with SoLoader for Android Application Bootstrap: Applications Provide Reactnativehost

These rules are ALWAYS ACTIVE for Android applications using React Native framework, specifically MainApplication or equivalent application entry point classes that manage native library initialization during application bootstrap.

### Rules

- **R-RNSOLO-001** MUST: Applications MUST provide a ReactNativeHost instance through getReactNativeHost() method for managing React instance lifecycle.
- **R-RNSOLO-002** MUST: SoLoader.init() call MUST be present in Application.onCreate() method before React Native component initialization.
- **R-RNSOLO-003** MUST: MainApplication class MUST extend android.app.Application and implement ReactApplication interface.
- **R-RNSOLO-004** MUST: getUseDeveloperSupport() method MUST be implemented and return valid configuration based on BuildConfig.DEBUG or equivalent flag.
- **R-RNSOLO-005** MUST: SoLoader.init() MUST be called before super.onCreate() completes and before any ReactInstanceManager operations.
- **R-RNSOLO-006** SHOULD: Use PackageList(this).getPackages() for automatic package discovery unless custom native modules require manual registration.
- **R-RNSOLO-007** SHOULD: Implement proper error handling and logging around SoLoader initialization to diagnose failures in production environments.

### Verify

```bash
# Verify SoLoader.init() is called in onCreate method
grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'onCreate'

# Verify MainApplication extends Application and implements ReactApplication
grep -r 'extends.*Application.*implements.*ReactApplication' template/android/app/src/main/java/

# Verify getReactNativeHost and getUseDeveloperSupport methods are implemented
grep -r 'getReactNativeHost\|getUseDeveloperSupport' template/android/app/src/main/java/
```

**Accept when:**
- SoLoader.init() call is present in Application.onCreate() method before React Native component initialization
- MainApplication class extends android.app.Application and implements ReactApplication interface
- getReactNativeHost() and getUseDeveloperSupport() methods are implemented and return valid configurations
- Error handling and logging are present around SoLoader initialization

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for SoLoader.init() presence in Application.onCreate() lifecycle method is mandatory. Code review verification of ReactApplication interface implementation and required method overrides is mandatory. CI pipeline MUST fail if SoLoader initialization is missing or occurs after React Native component instantiation.
</enforcement>