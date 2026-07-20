# Adopt React Native with SoLoader for Android Application Initialization: Applications Initialize Soloader

These rules are ALWAYS ACTIVE for all Android applications using React Native framework, specifically MainApplication.java or equivalent application entry point classes that manage native library initialization and React Native runtime lifecycle.

### Rules

- **R-SOLOADER-001** MUST: Applications MUST initialize SoLoader during onCreate lifecycle method using SoLoader.init() to establish secure native library loading before any native code execution.

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
```

**Accept when:**
- All verify commands execute successfully and confirm presence of required React Native initialization components
- SoLoader.init() is called with explicit native exopackage configuration parameter
- MainApplication class extends Application, implements ReactApplication, and provides getReactNativeHost() and getUseDeveloperSupport() methods
- PackageList is imported from com.facebook.react and used to initialize React Native packages
- SoLoader.init() is called as the first operation in onCreate() method, before any other initialization
- getUseDeveloperSupport() returns BuildConfig.DEBUG or equivalent build-time constant for proper development/production separation

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting the implementation. Static analysis scanning of MainApplication.java MUST confirm required React Native initialization patterns. Production builds MUST be verified to have developer support disabled. Dependency scanning MUST ensure SoLoader and React Native versions are current and free of known vulnerabilities.
</enforcement>