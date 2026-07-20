# Adopt React Native with SoLoader for Android Application Initialization: Applications Implement Getusedevelopersupport

These rules are ALWAYS ACTIVE for all Android applications using React Native framework, specifically MainApplication.java or equivalent application entry point classes that manage native library initialization and React Native runtime lifecycle.

### Rules

- **R-RN-001** MUST: Applications MUST extend Application class in MainApplication or equivalent entry point.
- **R-RN-002** MUST: Applications MUST implement ReactApplication interface in MainApplication.
- **R-RN-003** MUST: Applications MUST call SoLoader.init() in onCreate() method with explicit native exopackage configuration parameter.
- **R-RN-004** MUST: Applications MUST implement getReactNativeHost() method to provide ReactNativeHost instance.
- **R-RN-005** SHOULD: Applications SHOULD implement getUseDeveloperSupport() method to control developer mode features and debugging capabilities based on build configuration.
- **R-RN-006** SHOULD: Applications SHOULD configure getUseDeveloperSupport() to return BuildConfig.DEBUG or equivalent build-time constant.
- **R-RN-007** MUST: Applications MUST use PackageList from com.facebook.react to initialize React Native packages.
- **R-RN-008** MUST: SoLoader.init() MUST be called before any other React Native component initialization.
- **R-RN-009** MUST: Production builds MUST have developer support disabled (getUseDeveloperSupport() returns false).

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

# Verify getUseDeveloperSupport method exists
grep -r 'getUseDeveloperSupport' template/android/app/src/main/java/ | grep -q 'MainApplication' && echo 'getUseDeveloperSupport method implemented'
```

**Accept when:**
- MainApplication class extends Application
- MainApplication class implements ReactApplication interface
- SoLoader.init() is called with explicit native exopackage configuration parameter (false)
- SoLoader.init() is called in onCreate() method before other React Native initialization
- getReactNativeHost() method is implemented and returns ReactNativeHost instance
- getUseDeveloperSupport() method is implemented and returns BuildConfig.DEBUG or equivalent
- PackageList is imported from com.facebook.react and instantiated with application context
- All verify commands execute successfully and confirm presence of required React Native initialization components
- Production builds have developer support disabled

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-RN-00X rules marked MUST are mandatory and must be verified before accepting the implementation. Rules marked SHOULD are strongly recommended and should be verified unless documented exceptions exist.
</enforcement>