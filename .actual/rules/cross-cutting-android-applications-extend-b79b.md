# Adopt React Native with SoLoader for Android Application Initialization: Android Applications Extend

These rules are ALWAYS ACTIVE for all Android application initialization code, specifically MainApplication.java or equivalent application entry point classes that integrate React Native framework.

### Rules

- **R-ANDROID-001** MUST: Android applications MUST extend android.app.Application and implement com.facebook.react.ReactApplication interface to provide React Native runtime integration.
- **R-ANDROID-002** MUST: Call SoLoader.init() as the first operation in onCreate() method, before super.onCreate() if possible, to establish native loading security before any other initialization.
- **R-ANDROID-003** MUST: Configure getUseDeveloperSupport() to return BuildConfig.DEBUG or equivalent build-time constant to ensure proper development/production separation.
- **R-ANDROID-004** MUST: Use PackageList constructor with application context to automatically discover and initialize all React Native packages declared in package.json.
- **R-ANDROID-005** MUST: Verify that native exopackage mode parameter in SoLoader.init() matches your build configuration and deployment strategy.
- **R-ANDROID-006** MUST: Ensure SoLoader.init() is called with explicit native exopackage configuration parameter.
- **R-ANDROID-007** MUST: Production builds MUST have developer support disabled (getUseDeveloperSupport() returns false in release configuration).

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
- Production builds have developer support disabled
- SoLoader and React Native versions are current and free of known vulnerabilities

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-ANDROID rules are mandatory and must be verified before accepting Android application initialization code. CI/CD pipeline verification that production builds have developer support disabled is required. Dependency scanning to ensure SoLoader and React Native versions are current is mandatory.
</enforcement>