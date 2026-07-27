# Adopt React Native with SoLoader for Android Application Bootstrap: Applications Extend Reactinstancemanager

These rules are ALWAYS ACTIVE for Android applications using React Native framework, specifically MainApplication or equivalent application entry point classes that initialize native libraries during application bootstrap.

### Rules

- **R-ANDROID-RN-001** MUST: Call SoLoader.init() in Application.onCreate() method before React Native component initialization completes.
- **R-ANDROID-RN-002** MUST: Implement ReactApplication interface in the main Application class.
- **R-ANDROID-RN-003** MUST: Implement getReactNativeHost() method returning a valid ReactNativeHost configuration.
- **R-ANDROID-RN-004** MUST: Implement getUseDeveloperSupport() method that returns appropriate debug/release configuration.
- **R-ANDROID-RN-005** SHOULD: Use PackageList(this).getPackages() for automatic native module discovery unless custom modules require manual registration.
- **R-ANDROID-RN-006** SHOULD: Implement error handling and logging around SoLoader.init() to diagnose failures in production environments.
- **R-ANDROID-RN-007** MAY: Applications MAY extend ReactInstanceManager configuration for custom native module registration beyond PackageList defaults.

### Verify

```bash
# Verify SoLoader.init() is called in onCreate() method
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
- Native library loading occurs deterministically during application startup without race conditions

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for SoLoader.init() presence in Application.onCreate() lifecycle method is mandatory. Code review verification of ReactApplication interface implementation and required method overrides is mandatory. CI pipeline MUST fail if SoLoader initialization is missing or occurs after React Native component instantiation.
</enforcement>