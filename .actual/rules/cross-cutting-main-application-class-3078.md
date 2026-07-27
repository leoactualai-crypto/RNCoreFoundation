# Adopt React Native with SoLoader for Android Application Bootstrap: Main Application Class

These rules are ALWAYS ACTIVE for Android application entry point classes using React Native framework, specifically MainApplication or equivalent classes that bootstrap the React Native runtime.

### Rules

- **R-RN-ANDROID-001** MUST: The main application class MUST extend `android.app.Application` and implement `com.facebook.react.ReactApplication` interface.

### Verify

```bash
# Verify SoLoader.init() is called in onCreate() method
grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'onCreate'

# Verify MainApplication extends Application and implements ReactApplication
grep -r 'extends.*Application.*implements.*ReactApplication' template/android/app/src/main/java/

# Verify required React Native host methods are implemented
grep -r 'getReactNativeHost\|getUseDeveloperSupport' template/android/app/src/main/java/
```

**Accept when:**
- SoLoader.init() call is present in Application.onCreate() method before React Native component initialization
- MainApplication class extends `android.app.Application` and implements `ReactApplication` interface
- `getReactNativeHost()` and `getUseDeveloperSupport()` methods are implemented and return valid configurations

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for SoLoader.init() presence in Application.onCreate() lifecycle method is mandatory. Code review verification of ReactApplication interface implementation and required method overrides is mandatory. CI pipeline MUST fail if SoLoader initialization is missing or occurs after React Native component instantiation.
</enforcement>