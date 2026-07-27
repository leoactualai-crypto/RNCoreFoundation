# Adopt React Native with SoLoader for Android Application Bootstrap: Android Applications React

These rules are ALWAYS ACTIVE for Android applications using React Native framework, specifically MainApplication or equivalent application entry point classes that handle native library initialization during application bootstrap.

### Rules

- **R-ANDROID-REACT-001** MUST: Android applications using React Native MUST initialize SoLoader in the Application.onCreate() method before any React Native components are instantiated

### Verify

```bash
# Verify SoLoader.init() is called in onCreate() method
grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'onCreate'

# Verify MainApplication extends Application and implements ReactApplication
grep -r 'extends.*Application.*implements.*ReactApplication' template/android/app/src/main/java/

# Verify ReactApplication interface methods are implemented
grep -r 'getReactNativeHost\|getUseDeveloperSupport' template/android/app/src/main/java/
```

**Accept when:**
- SoLoader.init() call is present in Application.onCreate() method before React Native component initialization
- MainApplication class extends android.app.Application and implements ReactApplication interface
- getReactNativeHost() and getUseDeveloperSupport() methods are implemented and return valid configurations

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for SoLoader.init() presence in Application.onCreate() lifecycle method is mandatory. Code review verification of ReactApplication interface implementation and required method overrides is mandatory. Automated testing of application startup sequence on multiple Android API levels and device configurations is mandatory.
</enforcement>