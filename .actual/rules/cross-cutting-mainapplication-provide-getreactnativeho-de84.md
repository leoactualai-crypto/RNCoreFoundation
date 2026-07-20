# Adopt React Native with SoLoader for Android Application Initialization: Mainapplication Provide Getreactnativehost

These rules are ALWAYS ACTIVE for all Android applications using React Native framework, specifically for MainApplication.java or equivalent application entry point classes that manage native library initialization and React Native runtime lifecycle.

### Rules

- **R-RN-001** MUST: MainApplication MUST provide getReactNativeHost() method returning a configured ReactNativeHost instance that manages ReactInstanceManager lifecycle
- **R-RN-002** MUST: MainApplication MUST extend Application class
- **R-RN-003** MUST: MainApplication MUST implement ReactApplication interface
- **R-RN-004** MUST: SoLoader.init() MUST be called in onCreate() method with explicit native exopackage configuration parameter
- **R-RN-005** MUST: getUseDeveloperSupport() MUST return BuildConfig.DEBUG or equivalent build-time constant for proper development/production separation
- **R-RN-006** MUST: PackageList MUST be imported from com.facebook.react and used to initialize React Native packages
- **R-RN-007** MUST: SoLoader.init() MUST be called as the first operation in onCreate() method, before super.onCreate() if possible

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
- SoLoader.init() is placed as the first operation in onCreate() method
- getUseDeveloperSupport() returns BuildConfig.DEBUG or equivalent build-time constant

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-RN-### rules are mandatory and must be verified before accepting MainApplication implementations. CI/CD pipeline verification that production builds have developer support disabled is required. Static analysis warnings for missing or misconfigured SoLoader.init() must be escalated to errors.
</enforcement>