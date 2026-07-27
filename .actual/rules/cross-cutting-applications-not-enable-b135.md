# Adopt React Native with SoLoader for Android Application Bootstrap: Applications Not Enable

These rules are ALWAYS ACTIVE for Android applications using React Native framework, specifically MainApplication or equivalent application entry point classes that handle native library initialization during application bootstrap.

### Rules

- **R-RNSOL-001** MUST_NOT: Applications MUST NOT enable native exopackage mode in SoLoader.init() unless explicitly required for development optimization

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
- Native exopackage mode is disabled in SoLoader.init() call (exopackage parameter is false or omitted)

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for SoLoader.init() presence, ReactApplication interface implementation, and exopackage configuration is mandatory before accepting any Android application bootstrap code.
</enforcement>