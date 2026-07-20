# Adopt React Native with SoLoader for Android Application Initialization: Applications Expose Additional

These rules are ALWAYS ACTIVE for all Android applications using React Native framework, specifically MainApplication.java or equivalent application entry point classes that manage native library initialization and React Native runtime lifecycle.

### Rules

- **R-RN-001** MUST: Applications MUST extend Application class in MainApplication or equivalent entry point.
- **R-RN-002** MUST: Applications MUST implement ReactApplication interface in MainApplication.
- **R-RN-003** MUST: Applications MUST call SoLoader.init() in onCreate() method with explicit native exopackage configuration parameter.
- **R-RN-004** MUST: SoLoader.init() MUST be called before super.onCreate() if possible to establish native loading security before other initialization.
- **R-RN-005** MUST: Applications MUST provide getReactNativeHost() method implementation in MainApplication.
- **R-RN-006** MUST: Applications MUST provide getUseDeveloperSupport() method implementation in MainApplication.
- **R-RN-007** MUST: getUseDeveloperSupport() MUST return BuildConfig.DEBUG or equivalent build-time constant for proper development/production separation.
- **R-RN-008** MUST: Applications MUST use PackageList from com.facebook.react to initialize React Native packages.
- **R-RN-009** MUST: Production builds MUST have developer support disabled (getUseDeveloperSupport() returns false in release configuration).
- **R-RN-010** MAY: Applications MAY expose additional public API contracts beyond the minimum ReactApplication interface requirements for custom initialization logic.

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
- Production builds have developer support disabled through build-time configuration
- SoLoader.init() is positioned early in onCreate() lifecycle before other initialization

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-RN-001 through R-RN-009 rules are mandatory and must be verified before accepting Android application initialization code. R-RN-010 is permissive (MAY) and does not block acceptance. Violations of mandatory rules must trigger CI build failure and security review requirements.
</enforcement>