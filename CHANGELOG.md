# Changelog

## 1.5.0 — Release hardening
- App version label now reads generated BuildConfig metadata.
- Added a confirmation dialog before clearing all local preferences.
- Added an application information section with version/API compatibility and capability boundaries.
- Added optional minified Release APK build in GitHub Actions, gated by manual workflow input and repository signing secrets.
- Signing credentials are read only from CI environment variables; no keystore or passwords are committed.
- Expanded JVM test declarations from 8 to 10.

## 1.4.0 — Import stability
- Validated JSON imports before committing preferences.
- Added duplicate-ID, finite-value, range-clamping and unknown-ID handling.
