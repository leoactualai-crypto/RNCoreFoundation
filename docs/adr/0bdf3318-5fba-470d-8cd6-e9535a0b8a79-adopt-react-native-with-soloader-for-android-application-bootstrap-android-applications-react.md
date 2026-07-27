# Adopt React Native with SoLoader for Android Application Bootstrap: Android Applications React

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The Android application uses React Native framework requiring native library initialization through SoLoader at application startup
- The MainApplication class extends ReactApplication and implements standard React Native lifecycle hooks for developer support and instance management
- The application uses Facebook's PackageList and ReactInstanceManager for coordinating JavaScript and native module integration
- Native library loading through SoLoader.init() occurs during the onCreate lifecycle method with native exopackage disabled
- The architecture separates React Native host configuration from the main application context using getReactNativeHost pattern

## Problem Statement

Android applications using React Native must initialize native libraries securely and reliably during application bootstrap while maintaining proper separation between JavaScript runtime and native Android context, ensuring that native dependencies are loaded before any React Native components attempt to access them.

## Decision

1. MUST: Android applications using React Native MUST initialize SoLoader in the Application.onCreate() method before any React Native components are instantiated

## Policy Block

- MUST Android applications using React Native MUST initialize SoLoader in the Application.onCreate() method before any React Native components are instantiated

In scope:
- Android applications using React Native framework
- MainApplication or equivalent application entry point classes
- Native library initialization during application bootstrap
- React Native host and instance manager configuration

Out of scope:
- Pure native Android applications without React Native
- React Native iOS implementations
- Third-party library initialization unrelated to React Native
- Runtime native module loading after application startup

## Rationale

- SoLoader provides secure and reliable native library loading for React Native's C++ bridge components, preventing runtime crashes from missing native dependencies
- The ReactApplication interface contract ensures consistent lifecycle management across React Native Android applications and enables proper integration with the framework
- Initializing SoLoader in onCreate() guarantees native libraries are available before any JavaScript code executes, preventing race conditions and undefined behavior
- The pattern separates concerns between Android application context and React Native runtime, enabling independent testing and configuration of each layer

## Consequences

Positive:
- Native libraries are loaded securely and deterministically during application startup, preventing runtime failures
- Standard React Native lifecycle integration enables developer tools, hot reloading, and debugging capabilities
- PackageList automation reduces manual package registration errors and maintenance burden
- Clear separation between native Android and JavaScript runtime contexts improves testability and modularity

Negative:
- SoLoader initialization adds startup latency to application launch time, particularly on first run
- React Native framework dependency increases application size and complexity compared to pure native Android
- Native exopackage disabled mode may slow development iteration cycles compared to optimized configurations
- Framework coupling makes migration to alternative cross-platform solutions more difficult

## Alternatives

- Pure native Android implementation without React Native framework (rejected)
  Rejected because: Evidence shows React Native is already integrated with MainApplication extending ReactApplication, indicating cross-platform JavaScript development is a project requirement
  When valid: Valid for greenfield Android-only applications without cross-platform requirements
- Lazy initialization of SoLoader on first React Native component access (rejected)
  Rejected because: Deferred initialization introduces race conditions and unpredictable failure modes when multiple components attempt concurrent native access during startup
  When valid: Valid only for applications with guaranteed single-threaded React Native initialization paths
- Enable native exopackage mode for faster development iteration (deferred)
  Rejected because: Evidence shows exopackage explicitly disabled, suggesting stability or compatibility concerns outweigh development speed benefits
  When valid: Valid for development builds when team has verified exopackage compatibility with all native modules

## Risks

- SoLoader initialization failure on devices with restricted native library loading policies or incompatible architectures
  Mitigation: Implement error handling around SoLoader.init() with fallback messaging and crash reporting to identify affected device configurations
  Owner: Android platform team
- React Native version upgrades may introduce breaking changes to SoLoader initialization or ReactApplication interface contracts
  Mitigation: Pin React Native versions in dependency management and test upgrades in isolated environments before production deployment
  Owner: Mobile engineering team
- Native library loading increases application startup time, potentially violating performance budgets on low-end devices
  Mitigation: Monitor cold start metrics across device tiers and consider lazy loading non-critical native modules after initial render
  Owner: Performance engineering team

## Implementation Notes

- Ensure SoLoader.init() is called before super.onCreate() completes and before any ReactInstanceManager operations
- Configure BuildConfig.DEBUG or equivalent flag for getUseDeveloperSupport() to enable development tools in debug builds only
- Use PackageList(this).getPackages() for automatic package discovery unless custom native modules require manual registration
- Implement proper error handling and logging around SoLoader initialization to diagnose failures in production environments

## Continuation Context


Verify commands:
- grep -r 'SoLoader\.init' template/android/app/src/main/java/ | grep -q 'onCreate'
- grep -r 'extends.*Application.*implements.*ReactApplication' template/android/app/src/main/java/
- grep -r 'getReactNativeHost\|getUseDeveloperSupport' template/android/app/src/main/java/

Accept when:
- SoLoader.init() call is present in Application.onCreate() method before React Native component initialization
- MainApplication class extends android.app.Application and implements ReactApplication interface
- getReactNativeHost() and getUseDeveloperSupport() methods are implemented and return valid configurations

## Enforcement

- Verified by: Static analysis scanning for SoLoader.init() presence in Application.onCreate() lifecycle method
- Verified by: Code review verification of ReactApplication interface implementation and required method overrides
- Verified by: Automated testing of application startup sequence on multiple Android API levels and device configurations
- Violation handling: CI pipeline fails if SoLoader initialization is missing or occurs after React Native component instantiation
- Violation handling: Runtime crash reporting alerts trigger when SoLoader initialization fails in production
- Violation handling: Code review blocks merge requests that modify Application.onCreate() without maintaining SoLoader initialization order
- Exception process: Document technical justification for alternative native library loading approach with security and stability analysis
- Exception process: Obtain approval from Android platform team lead and mobile architecture review board
- Exception process: Implement equivalent native library initialization guarantees with comprehensive test coverage demonstrating reliability