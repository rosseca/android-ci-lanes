# android-ci-lanes

Shared Android Fastlane lanes that mirror the tattoo-android Codemagic workflows. Lanes assume Codemagic-style environment variables but work with any CI once the same secrets are exported.

## Lanes

| Lane | What it does |
| --- | --- |
| `ci_seed_android_files` | Re-creates `local.properties`, flavor-specific `app.properties`, `google-services.json`, `meta.properties`, and `keystore.properties` from Base64 env vars. |
| `ci_pr_checks` | Runs the same setup as Codemagic PR workflow followed by `spotlessCheck` (if available) and `testDevDebugUnitTest`. |
| `ci_dev_distribution` | Prepares files, runs checks, assembles `DevRelease`, and optionally uploads the latest APK/AAB to Firebase App Distribution. |
| `ci_prod_play_store` | Prepares files, runs Prod unit tests, bundles `ProdRelease`, and can upload the generated AAB to the Play Store. |
| `ci_unit_tests` | Convenience wrapper around a configurable Gradle unit test task. |
| `ci_static_analysis` | Runs Spotless (skips gracefully if not configured). |

All helper logic lives in the same `Fastfile`, so importing this repo is enough.

## Expected environment variables

| Variable | Purpose |
| --- | --- |
| `ANDROID_SDK_ROOT` | Used to materialize `local.properties`. |
| `APP_PROPERTIES_DEV`, `APP_PROPERTIES_STAGE`, `APP_PROPERTIES_PROD` | Base64-encoded flavor config files written under `app/src/<Flavor>/app.properties`. |
| `GOOGLE_SERVICES_DEV`, `GOOGLE_SERVICES_STAGE`, `GOOGLE_SERVICES_PROD` | Base64-encoded `google-services.json` files per flavor. |
| `META_PROPERTIES` | Base64 payload for `app/src/Prod/meta.properties`. |
| `KEYSTORE_FILE` | Base64 keystore binary written to `app/upload_keystore.jks` (override via `KEYSTORE_RELATIVE_PATH`). |
| `KEYSTORE_PASSWORD`, `KEYSTORE_ALIAS`, `KEY_PASSWORD` | Used for `keystore.properties`. |
| `PROJECT_BUILD_NUMBER` | Passed as Gradle `-PversionCode` when present. |
| `FIREBASE_APP_ID`, `FIREBASE_TEST_GROUP`, `FIREBASE_SERVICE_ACCOUNT`, `FIREBASE_TOKEN` | Drive Firebase App Distribution uploads. Supply Base64 JSON for the service account. |
| `GOOGLE_PLAY_SERVICE_ACCOUNT_CREDENTIALS`, `PLAY_STORE_TRACK`, `PLAY_STORE_RELEASE_STATUS` | Drive Play Store uploads via `upload_to_play_store`. |
| `CI_UPLOAD_FIREBASE`, `CI_UPLOAD_PLAY_STORE` | Set to `1` to opt into uploads inside CI lanes. |

## Using from another repo

Add the repo as a git dependency inside your Android project Fastfile:

```ruby
import_from_git(
  url: 'https://github.com/andreimarinov-leadtech/android-ci-lanes',
  path: 'fastlane/Fastfile'
)
```

You can then call any lane defined above, e.g. `bundle exec fastlane android ci_pr_checks`.

