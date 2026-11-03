# Aquaman

A personal gym tracker for Android. Log sessions, exercises, sets and bodyweight, and follow
progress over time. Native Kotlin + Jetpack Compose, with Firebase Authentication for accounts
and Cloud Firestore as the data store.

Course project for **ID00ES00** (OAMK). Built as a real daily-use app, not a demo.

---

## What it does

- Sign up / sign in with email and password (Firebase Authentication)
- Log a training session: exercises, sets, reps, weight, RPE and a session rating
- Log bodyweight entries against a target weight
- Review past sessions and progress charts
- Works offline — Firestore caches locally, so logging at the gym does not need a connection

## Status

Under construction. This README describes the app and is updated as the work progresses.

| Part | State |
|---|---|
| Kotlin + Compose project skeleton | done |
| Firebase project created, `google-services.json` in `app/` | done |
| Firebase SDK wired into Gradle | in progress |
| Email/password auth (sign up, sign in, sign out) | not started |
| Firestore data layer (sessions, weigh-ins) | not started |
| Session and weigh-in screens | not started |
| Progress charts | not started |

## Tech stack

| Layer | Choice |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose + Material 3 |
| Auth | Firebase Authentication (email/password) |
| Data | Cloud Firestore (offline persistence enabled) |
| Build | Gradle (Kotlin DSL) with a version catalog in `gradle/libs.versions.toml` |
| Min SDK | API 26 (Android 8.0) |

## Data model (Cloud Firestore)

Data is stored per user, so one document tree never exposes another user's data:

```
users/{uid}/sessions/{sessionId}
    date, exercises[], rpe, rating, notes
users/{uid}/weighins/{weighInId}
    date, weightKg
```

## Setup

1. **Firebase config** — `app/google-services.json` is committed (it is client configuration,
   not a secret). It belongs to the Firebase project `aquaman-bdaf7` and the package
   `fi.eemeli.aquaman`. If the package name changes, the app must be re-registered in Firebase.
2. **Authentication** — in the Firebase console, enable `Email/Password` under
   Authentication → Sign-in method.
3. **Firestore** — create the database (production mode, region `europe-north1`) and publish the
   security rules from `firestore.rules`. The rules allow a user to read and write only their own
   `users/{uid}` tree.
4. **Build and run**

```bash
./gradlew :app:assembleDebug          # build
./gradlew :app:installDebug           # install on a connected device
```

Note: Firebase requires Google Play services on the device or emulator. An emulator image without
the Play Store will fail authentication.

## Repository layout

```
app/
├── google-services.json               # Firebase client configuration
├── build.gradle.kts                   # app module: SDK levels, Firebase dependencies
└── src/main/java/fi/eemeli/aquaman/
    ├── MainActivity.kt                # entry point
    └── ui/theme/                      # Compose theme (colours, typography)
gradle/libs.versions.toml              # version catalog: all dependency versions
```

## Development notes

- Branch per feature, then a pull request into `main` — no direct commits to `main`.
- Commit messages describe what changed and why, not just "update".
