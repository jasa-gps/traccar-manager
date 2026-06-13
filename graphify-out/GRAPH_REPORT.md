# Graph Report - .  (2026-06-13)

## Corpus Check
- Corpus is ~12,790 words - fits in a single context window. You may not need a graph.

## Summary
- 179 nodes · 185 edges · 15 communities (12 shown, 3 thin omitted)
- Extraction: 96% EXTRACTED · 4% INFERRED · 0% AMBIGUOUS · INFERRED: 7 edges (avg confidence: 0.88)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_WebView Main Screen|WebView Main Screen]]
- [[_COMMUNITY_Brand & Build Tooling|Brand & Build Tooling]]
- [[_COMMUNITY_Error Screen & URL Entry|Error Screen & URL Entry]]
- [[_COMMUNITY_Project Config, Metadata & Licensing|Project Config, Metadata & Licensing]]
- [[_COMMUNITY_App Bootstrap & Firebase Init|App Bootstrap & Firebase Init]]
- [[_COMMUNITY_Secure Token Storage & Biometrics|Secure Token Storage & Biometrics]]
- [[_COMMUNITY_iOS App Delegate|iOS App Delegate]]
- [[_COMMUNITY_Firebase Platform Options|Firebase Platform Options]]
- [[_COMMUNITY_iOS Runner Tests|iOS Runner Tests]]
- [[_COMMUNITY_Android MainActivity|Android MainActivity]]
- [[_COMMUNITY_App Icon & GPS Branding|App Icon & GPS Branding]]

## God Nodes (most connected - your core abstractions)
1. `Release Workflow` - 7 edges
2. `AppDelegate` - 5 edges
3. `pubspec.yaml (traccar_manager v6.0.5+71)` - 5 edges
4. `RunnerTests` - 3 edges
5. `ErrorScreen` - 3 edges
6. `_ErrorScreenState` - 3 edges
7. `MainApp` - 3 edges
8. `_MainAppState` - 3 edges
9. `MainScreen` - 3 edges
10. `_MainScreenState` - 3 edges

## Surprising Connections (you probably didn't know these)
- `Dependabot Config (pub, daily)` --references--> `pubspec.yaml (traccar_manager v6.0.5+71)`  [INFERRED]
  /Users/rizal/develop/jasa-gps/_references/traccar-manager/.github/dependabot.yml → /Users/rizal/develop/jasa-gps/_references/traccar-manager/pubspec.yaml
- `Flutter Analyze Workflow` --references--> `pubspec.yaml (traccar_manager v6.0.5+71)`  [INFERRED]
  /Users/rizal/develop/jasa-gps/_references/traccar-manager/.github/workflows/analyze.yml → /Users/rizal/develop/jasa-gps/_references/traccar-manager/pubspec.yaml
- `pubspec.yaml (traccar_manager v6.0.5+71)` --references--> `flutter_lints Ruleset`  [INFERRED]
  /Users/rizal/develop/jasa-gps/_references/traccar-manager/pubspec.yaml → /Users/rizal/develop/jasa-gps/_references/traccar-manager/analysis_options.yaml
- `GitHub Funding Config (traccar)` --conceptually_related_to--> `Traccar Platform (open-source GPS tracking)`  [INFERRED]
  /Users/rizal/develop/jasa-gps/_references/traccar-manager/.github/FUNDING.yml → /Users/rizal/develop/jasa-gps/_references/traccar-manager/README.md
- `Release Workflow` --references--> `pubspec.yaml (traccar_manager v6.0.5+71)`  [INFERRED]
  /Users/rizal/develop/jasa-gps/_references/traccar-manager/.github/workflows/release.yml → /Users/rizal/develop/jasa-gps/_references/traccar-manager/pubspec.yaml

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **End-to-end Release Pipeline** — workflows_release_build_android, workflows_release_smoke_test_android, workflows_release_publish_android, workflows_release_build_ios, workflows_release_publish_ios, workflows_release_release_github [EXTRACTED 0.95]

## Communities (15 total, 3 thin omitted)

### Community 0 - "WebView Main Screen"
Cohesion: 0.04
Nodes (49): dart:async, dart:collection, dart:convert, InAppWebViewController?, _appLinks, _appLinksSubscription, _authenticated, build (+41 more)

### Community 1 - "Brand & Build Tooling"
Cohesion: 0.06
Nodes (32): dart:io, return, addStream, appName, args, code, content, _createKeystore (+24 more)

### Community 2 - "Error Screen & URL Entry"
Cohesion: 0.11
Nodes (19): build, _controller, createState, dispose, error, ErrorScreen, _ErrorScreenState, initState (+11 more)

### Community 3 - "Project Config, Metadata & Licensing"
Cohesion: 0.14
Nodes (18): Dart Analysis Options, flutter_lints Ruleset, Dependabot Config (pub, daily), GitHub Funding Config (traccar), Apache License 2.0, pubspec.yaml (traccar_manager v6.0.5+71), Traccar Manager README, Anton Tananaev (+10 more)

### Community 4 - "App Bootstrap & Firebase Init"
Cohesion: 0.14
Nodes (14): GlobalKey, build, createState, initializeApp, initState, main, MainApp, _MainAppState (+6 more)

### Community 5 - "Secure Token Storage & Biometrics"
Cohesion: 0.15
Nodes (12): dart:developer, _auth, delete, read, save, _storage, _tokenKey, TokenStore (+4 more)

### Community 6 - "iOS App Delegate"
Cohesion: 0.20
Nodes (7): Any, Bool, FlutterAppDelegate, FlutterImplicitEngineBridge, FlutterImplicitEngineDelegate, AppDelegate, UIApplication

### Community 7 - "Firebase Platform Options"
Cohesion: 0.29
Nodes (6): android, DefaultFirebaseOptions, ios, package:firebase_core/firebase_core.dart, package:flutter/foundation.dart, static const FirebaseOptions

## Knowledge Gaps
- **97 isolated node(s):** `UIApplication`, `Any`, `Bool`, `FlutterImplicitEngineBridge`, `error` (+92 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `MainScreen` connect `Error Screen & URL Entry` to `WebView Main Screen`?**
  _High betweenness centrality (0.010) - this node is a cross-community bridge._
- **Why does `_MainScreenState` connect `Error Screen & URL Entry` to `WebView Main Screen`?**
  _High betweenness centrality (0.010) - this node is a cross-community bridge._
- **Are the 5 inferred relationships involving `pubspec.yaml (traccar_manager v6.0.5+71)` (e.g. with `Dependabot Config (pub, daily)` and `flutter_lints Ruleset`) actually correct?**
  _`pubspec.yaml (traccar_manager v6.0.5+71)` has 5 INFERRED edges - model-reasoned connections that need verification._
- **What connects `UIApplication`, `Any`, `Bool` to the rest of the system?**
  _97 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `WebView Main Screen` be split into smaller, more focused modules?**
  _Cohesion score 0.04 - nodes in this community are weakly interconnected._
- **Should `Brand & Build Tooling` be split into smaller, more focused modules?**
  _Cohesion score 0.06060606060606061 - nodes in this community are weakly interconnected._
- **Should `Error Screen & URL Entry` be split into smaller, more focused modules?**
  _Cohesion score 0.11052631578947368 - nodes in this community are weakly interconnected._