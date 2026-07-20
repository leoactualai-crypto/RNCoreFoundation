# Adopt React Native with SoLoader for Android Application Initialization: Applications Use Com

These rules are ALWAYS ACTIVE for all Android applications using React Native framework, specifically MainApplication.java or equivalent application entry point classes that manage native library initialization and React Native runtime lifecycle.

### Rules

- **R-RN-001** MUST: Applications MUST use com.facebook.react.PackageList to declare and initialize React Native packages with their native dependencies.
- **R-RN-002** MUST: MainApplication class MUST extend Application and implement ReactApplication interface.
- **R-RN-003** MUST: SoLoader.init() MUST be called as the first operation in onCreate() method with explicit native exopackage configuration parameter.
- **R-RN-004** MUST: getUseDeveloperSupport() MUST return BuildConfig.DEBUG or equivalent build-time constant to ensure proper development/production separation.
- **R-RN-005** MUST: MainApplication MUST provide getReactNativeHost() method that returns a properly configured ReactNativeHost instance.
- **R-RN-006** MUST: Production builds MUST have developer support disabled (getUseDeveloperSupport() returns false in release configuration).

### Verify

```bash
# Verify SoLoader is initialized with explicit exopackage mode
grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'false' && echo 'SoLoader initialized with explicit exopackage mode'

# Verify MainApplication extends Application
grep -r 'extends Application' template/android/app/src/main/java/ | grep -q 'MainApplication' && echo 'MainApplication extends Application'

# Verify MainApplication implements ReactApplication
grep -r 'implements ReactApplication' template/android/app/src/main/java/ | grep -q 'MainApplication' && echo 'MainApplication implements ReactApplication'

# Verify PackageList is imported and used
grep -r 'PackageList' template/android/app/src/main/java/ | grep -q 'com.facebook.react.PackageList' && echo 'PackageList imported and used'

# Verify getReactNativeHost method exists
grep -r 'getReactNativeHost' template/android/app/src/main/java/ | grep -q 'MainApplication' && echo 'getReactNativeHost method present'

# Verify getUseDeveloperSupport method exists
grep -r 'getUseDeveloperSupport' template/android/app/src/main/java/ | grep -q 'MainApplication' && echo 'getUseDeveloperSupport method present'
```

**Accept when:**
- All verify commands execute successfully and confirm presence of required React Native initialization components
- SoLoader.init() is called with explicit native exopackage configuration parameter (false)
- MainApplication class extends Application, implements ReactApplication, and provides getReactNativeHost() and getUseDeveloperSupport() methods
- PackageList is imported from com.facebook.react and used to initialize React Native packages
- getUseDeveloperSupport() returns BuildConfig.DEBUG or equivalent build-time constant
- Production builds have developer support disabled in release configuration

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-RN-### rules MUST be verified before accepting MainApplication.java or equivalent React Native application entry point files. Violations of R-RN-001 through R-RN-006 MUST result in build failure or security review escalation.
</enforcement>