# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this app is

Traccar Manager is a **thin Flutter native shell around a WebView** that hosts the
Traccar server's own web UI (`flutter_inappwebview`). The app itself contains no
tracking/map/UI logic — that all lives in the remote web app served by the user's
Traccar server (default `https://demo.traccar.org`). The Flutter side exists only to
bridge native-only capabilities to the web app: secure token storage with biometric
auth, Firebase push, file download/share, OAuth deep links, and runtime permissions.

There are only four source files in `lib/`. Understand the bridge and you understand the app.

## Commands

Requires Dart SDK `^3.7.2` (per `pubspec.yaml`).

```bash
flutter pub get                 # install dependencies
flutter analyze                 # lint (CI gate — runs on every push/PR to main)
flutter run                     # run on attached device/emulator
flutter build apk --release     # Android release build
flutter build ios --release     # iOS release build
flutterfire configure           # regenerate lib/firebase_options.dart + native Firebase config
```

There are **no tests** (only the `flutter_test` dev dependency is declared; no `test/`
directory exists). The only CI quality gate is `flutter analyze`.

## Architecture: the JS ↔ native bridge

All native/web communication flows through one bidirectional channel set up in
`lib/main_screen.dart`.

**Web → native:** an injected user script exposes `window.appInterface.postMessage(msg)`
(queued until `flutter_inappwebview` is ready). Messages are **pipe-delimited strings**
(`"command|arg"`) dispatched in `_handleWebMessage`. Commands:
- `login|<token>` — persist the login token (then push the FCM token back to web)
- `authentication` — web requests the stored token; native reads it (biometric-gated) and calls back `handleLoginToken`
- `authenticated` — completes the `_authenticated` Completer (gates notification setup)
- `logout` — delete the stored token
- `download|<base64>` — save/share a base64 blob (also auto-intercepts Blob `URL.createObjectURL` for xlsx exports)
- `server|<url>` — switch Traccar server: clear token, persist new URL, reload

**Native → web:** `_controller.evaluateJavascript` calls global functions the web app
defines (all called optionally with `?.`): `handleLoginToken(token)`,
`updateNotificationToken(fcmToken)`, `handleNativeNotification(json)`.

### Key mechanics

- **Token storage** (`lib/token_store.dart`): `flutter_secure_storage` keyed by `token`.
  `read(authenticate: true)` gates retrieval behind a `local_auth` biometric/device-credential
  prompt; `read(false)` skips it (used for file downloads).
- **Server URL** persisted in `SharedPreferences` under key `url`; `_getUrl()` strips
  trailing slash and falls back to `demo.traccar.org`.
- **Navigation interception** (`shouldOverrideUrlLoading`): OAuth authorize requests
  (URLs carrying `response_type`/`client_id`/`redirect_uri`/`scope`) are launched in the
  external browser via `_launchAuthorizeRequest`; off-origin links open externally;
  downloadable extensions (`xlsx`/`kml`/`csv`/`gpx`) are fetched with the bearer token
  and shared instead of navigated.
- **Deep links / OAuth callback** (`app_links`): the `org.traccar.manager://` scheme
  carries the OAuth `code` back; `_initAppLinks` rewrites it onto the current server
  origin and loads it in the WebView.
- **Push** (`firebase_messaging`): cold-start notifications with an `eventId` deep-link
  to `/event/<id>`; foreground messages forward to `handleNativeNotification` and show a
  SnackBar. FCM token is pushed to the web app on login and on refresh.
- **Init ordering**: `_initialized` and `_authenticated` Completers sequence startup —
  settings + WebView controller must both be ready before app-links/notifications wire up.
- **Error handling** (`lib/error_screen.dart`): main-frame load errors swap the WebView
  for a screen letting the user re-enter the server URL.

## White-labeling / rebranding

`tool/brand.dart` is a one-shot script to fork this into a rebranded app: edit the
constants at the top (`appName`, `packageId`, `version`, `url`, `iconPath`, keystore
settings), then run:

```bash
dart tool/brand.dart        # rewrites title, IDs, icons, default URL; creates keystore
flutterfire configure       # regenerate Firebase config for the new bundle/app IDs
```

It rewrites the app title, bundle/application IDs, launcher icons, the default server URL
in `main_screen.dart`, and generates an Android signing keystore + `key.properties`.

## Release

- `org.traccar.manager` (Android) / `org.traccar.TraccarManager` (iOS); version in `pubspec.yaml`.
- `.github/workflows/release.yml` runs on `v*` tags (or manual dispatch): builds a signed
  Android APK → emulator smoke test → Play publish, and a signed iOS IPA → App Store
  Connect upload, then a GitHub release. All signing material comes from repo secrets.
- `pubspec.lock` and `ios/Podfile.lock` are intentionally untracked.

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

Rules:
- For codebase questions, first run `graphify query "<question>"` when graphify-out/graph.json exists. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).
