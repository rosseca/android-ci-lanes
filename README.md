# android-ci-lanes

Shared Fastlane lanes for Android CI/CD that replicate the Codemagic workflows used by [`rosseca/tattoo-android`](https://github.com/rosseca/tattoo-android).

## Features

- Deterministic recreation of all Codemagic secrets (`local.properties`, flavor configs, `google-services.json`, meta files, keystores).
- Drop-in lanes for PR checks, Dev Firebase distributions, and Prod Play Store releases.
- Optional Firebase App Distribution and Play Store uploads controlled purely via environment variables.
- Works with Codemagic, GitHub Actions, Bitrise, or local development via the same Fastlane API.

## Getting started

1. Install dependencies:
   ```bash
   cd android-ci-lanes
   bundle install
   ```
2. In your Android repository Fastfile, import the shared lanes:
   ```ruby
   import_from_git(
     url: 'https://github.com/andreimarinov-leadtech/android-ci-lanes',
     path: 'fastlane/Fastfile'
   )
   ```
3. Export the expected environment variables (see [`fastlane/README.md`](fastlane/README.md)).
4. Invoke a lane, for example:
   ```bash
   bundle exec fastlane android ci_dev_distribution upload_to_firebase:true
   ```

## Repo structure

```
android-ci-lanes/
├── Gemfile              # pin Fastlane
├── fastlane/
│   ├── Fastfile         # shared lanes and helper logic
│   ├── Pluginfile       # firebase_app_distribution plugin declaration
│   └── README.md        # lane docs and env var reference
└── .gitignore
```

Pull requests / issues welcome.
