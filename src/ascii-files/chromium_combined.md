

== ./chromium/chromium_0.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/android_webview/browser/metrics/aw_metrics_service_client.h#L48-L111

```c
// AwMetricsServiceClient is a singleton which manages WebView metrics
// collection.
//
// Metrics should be enabled iff all these conditions are met:
//  - The user has not opted out (controlled by GMS).
//  - The app has not opted out (controlled by manifest tag).
//  - This client is in the 2% sample (controlled by client ID hash).
// The first two are recorded in |user_consent_| and |app_consent_|, which are
// set by SetHaveMetricsConsent(). The last is recorded in |is_in_sample_|.
//
// Metrics are pseudonymously identified by a randomly-generated "client ID".
// WebView stores this in prefs, written to the app's data directory. There's a
// different such directory for each user, for each app, on each device. So the
// ID should be unique per (device, app, user) tuple.
//
// To avoid the appearance that we're doing anything sneaky, the client ID
// should only be created and retained when neither the user nor the app have
// opted out. Otherwise, the presence of the ID could give the impression that
// metrics were being collected.
//
< ASCII >
// WebView metrics set up happens like so:
//
//   startup
//      │
//      ├────────────┐
//      │            ▼
//      │         query GMS for consent
//      ▼            │
//   Initialize()    │
//      │            ▼
//      │         SetHaveMetricsConsent()
//      │            │
//      │ ┌──────────┘
//      ▼ ▼
//   MaybeStartMetrics()
//      │
//      ▼
//   MetricsService::Start()
< ASCII >
//
// All the named functions in this diagram happen on the UI thread. Querying GMS
// happens in the background, and the result is posted back to the UI thread, to
// SetHaveMetricsConsent(). Querying GMS is slow, so SetHaveMetricsConsent()
// typically happens after Initialize(), but it may happen before.
//
// Each path sets a flag, |init_finished_| or |set_consent_finished_|, to show
// that path has finished, and then calls MaybeStartMetrics(). When
// MaybeStartMetrics() is called the first time, it sees only one flag is true,
// and does nothing. When MaybeStartMetrics() is called the second time, it
// decides whether to start metrics.
//
// If consent was granted, MaybeStartMetrics() determines sampling by hashing
// the client ID (generating a new ID if there was none). If this client is in
// the sample, it then calls MetricsService::Start(). If consent was not
// granted, MaybeStartMetrics() instead clears the client ID, if any.
//
// Similarly, when
// `android_webview::features::kWebViewAppsPackageNamesAllowlist` is enabled,
// WebView will try to lookup the embedding app's package name in a list of apps
// whose package names are allowed to be recorded. This operation takes place on
// a background thread. The result of the lookup is then posted back on the UI
// thread and SetAppPackageNameLoggingRule() will be called. Unlike user's
// consent, the metrics service doesn't currently block on the allowlist lookup
// result. If the result isn't present at the moment of creating a metrics log,
// it assumes that the app package name isn't allowed to be logged.
```
## Visual type:
- #flowchart


== ./chromium/chromium_1.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/android_webview/browser/safe_browsing/aw_safe_browsing_allowlist_manager.cc#L24-L51

```c
// This is a simple trie structure designed for handling host/domain matches
// for Safebrowsing allowlisting. For the match rules, see the class header.
//
< ASCII >
// It is easy to visualize the trie edges as hostname components of a url in
// reverse order. For example an allowlist of google.com will have a tree
// tree structure as below.
//                       root
//                         | com
//                       Node1
//                google/    \ example
//                   Node2   Node3
//
// Normally, a search in the tree should end in a leaf node for a positive
// match. For example in the tree above com.google and com.example are matches.
// However, the allowlisting also allows matching subdomains if there is a
// leading dot,  for example, see ."google.com" and a.google.com below:
//                       root
//                         | com
//                       Node1
//                         | google
//                       Node2
//                         | a
//                       Node3
// Here, both Node2 and Node3 are valid terminal nodes to terminate a match.
// The boolean is_terminal indicates whether a node can successfully terminate
// a search (aka. whether this rule was entered to the allowlist) and
// match_prefix indicate if this should match exactly, or just do a prefix
// match.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_10.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/components/arc/mojom/payment_app.mojom#L123-L137

```c
< ASCII >
// The service that runs in ARC and allows the browser to invoke the TWA payment
// app that is installed in ARC, if it implements payment intents as described
// in https://web.dev/android-payment-apps-overview/. At first, only
// "https://play.google.com/billing" payment method is supported.
//
// --------------------      --------------------------------------------------
// |     Browser      |      |                      ARC                       |
// |                  |      |                                                |
// | ---------------  |      |  --------------    -------    ---------------- |
// | | Web Payment |<-|------|->| PaymentApp |<-->| TWA |<-->| Play Billing | |
// | ---------------  |      |  --------------    -------    ---------------- |
// |                  |      |                                                |
// --------------------      --------------------------------------------------
//
// Next method ID: 3
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_100.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/try_chrome_dialog_win/try_chrome_dialog.h#L28-L41

```c
// This class displays a modal dialog using the views system. The dialog asks
// the user to give Chrome another try. This class only handles the UI so the
// resulting actions are up to the caller.
//
< ASCII >
// The layout is as follows:
//
//   +-----------------------------------------------+
//   | |icon| Header text.                       [x] |
//   |                                               |
//   |        Body text.                             |
//   |        [ Open Chrome ] [No Thanks]            |
//   +-----------------------------------------------+
//
// Some variants do not have body text, or only have one button.
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_101.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/webauthn/authenticator_request_sheet_view.h#L23-L59

```c
// Defines the basic structure of sheets shown in the authenticator request
// dialog. Each sheet corresponds to a given step of the authentication flow,
// and encapsulates the controls above the Ok/Cancel buttons, namely:
//  -- an optional progress-bar-style activity indicator (at the top),
//  -- an optional `back icon`,
//  -- a pretty illustration in the top half of the dialog,
//  -- the title of the current step,
//  -- the description of the current step,
//  -- an optional view with step-specific content, added by subclasses, filling
//     the rest of the space, and
//  -- an optional contextual error.
< ASCII >
//
// +-------------------------------------------------+
// |*************************************************|
// |. (<-). . . . . . . . . . . . . . . . . . . . . .|
// |. . . . I L L U S T R A T I O N   H E R E . . . .|
// |. . . . . . . . . . . . . . . . . . . . . . . . .|
// |                                                 |
// | Title of the current step                       |
// |                                                 |
// | Description text explaining to the user what    |
// | this step is all about.                         |
// |                                                 |
// | +---------------------------------------------+ |
// | |                                             | |
// | |          Step-specific content view         | |
// | |                                             | |
// | |                                             | |
// | +---------------------------------------------+ |
// |  optional contextual error                      |
// +-------------------------------------------------+
// |                                   OK   CANCEL   | <- Not part of this view.
// +-------------------------------------------------+
//
< ASCII >
// TODO(https://crbug.com/852352): The Web Authentication and Web Payment APIs
// both use the concept of showing multiple "sheets" in a single dialog. To
// avoid code duplication, consider factoring out common parts.
```
## Visual type:
- #gui


== ./chromium/chromium_102.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/webauthn/hover_list_view.h#L26-L39

```c
< ASCII >
// View that shows a list of items. Each item is rendered as a HoverButton with
// an icon, name, optional description, and chevron, like so:
//
//  +----------------------------------+
//  | ICON1 | Item 1 name          | > |
//  |       | Item 1 description   | > |
//  +----------------------------------+
//  | ICON2 | Item 2 name          | > |
//  |       | Item 2 description   | > |
//  +----------------------------------+
//  | ICON3 | Item 3 name          | > |
//  |       | Item 3 description   | > |
//  +----------------------------------+
//
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_103.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/vr/speech_recognizer.h#L26-L41

```c
< ASCII >
// Note that speech recognition is activated on VR UI thread. This means it
// usually involves 3 threads. In the simplest case, the thread communication
// looks like the following:
//     VR UI thread        Browser thread         IO thread
//          |                    |                    |
//          |----ActivateVS----->|                    |
//          |                    |------Start------>  |
//          |                    |                    |
//          |                    |<-NotifyStateChange-|
//          |<--OnSRStateChanged-|                    |
//          |                    |                    |
//          |                    |<--OnSpeechResult---|
//          |<--OnSRStateChanged-|                    |
//          |                 navigate                |
//          |                    |                    |
// VS = voice search, SR = speech recognition
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_104.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/vr/renderers/textured_quad_renderer.cc#L14-L61

```c
// clang-format off
//
< ASCII >
// A rounded rect is subdivided into a number of triangles.
// _______________
// | /    _,-' \ |
// |/_,,-'______\|
// |            /|
// |           / |
// |          /  |
// |         /   |
// |        /    |
// |       /     |
// |      /      |
// |     /       |
// |    /        |
// |   /         |
// |  /          |
// | /           |
// |/____________|
// |\     _,-'' /|
// |_\ ,-'____ /_|
//
< ASCII >
// Most of these do not contain an arc. To simplify the rendering of those
// that do, we include a "corner position" attribute. The corner position is
// the distance from the center of the nearest "corner circle". Only those
// triangles containing arcs have a non-zero corner position set. The result
// is that for interior triangles, their corner position is uniformly (0, 0).
// I.e., they are always deemed "inside".
//
// A further complication is that different corner radii will require these
// various triangles to be sized differently relative to one another. We
// would prefer not no continually recreate our vertex buffer, so we include
// another attribute, the "offset scalars". These scalars are only ever 1.0,
// 0.0, or -1.0 and control the addition or subtraction of the horizontal
// and vertical corner offset. This lets the corners of the triangles be
// computed in the vertex shader dynamically. It also happens that the
// texture coordinates can also be easily computed in the vertex shader.
//
// So if the the corner offsets are vr and hr where
//     vr = corner_radius / height;
//     hr = corner_radius / width;
//
// Then the full position is then given by
//   p = (x + osx * hr, y + osy * vr, 0.0, 1.0)
//
// And the full texture coordinate is given by
//   (0.5 + p[0], 0.5 - p[1])
//
```
## Visual type:
- #custom


== ./chromium/chromium_105.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/common/pref_names.cc#L158-L183

```c
// Non-syncable item.
// It is used in two distinct ways.
// (1) Used for two-step initialization of locale in ChromeOS
//     because synchronization of kApplicationLocale is not instant.
// (2) Used to detect locale change.  Locale change is detected by
//     LocaleChangeGuard in case values of kApplicationLocaleBackup and
//     kApplicationLocale are both non-empty and differ.
< ASCII >
// Following is a table showing how state of those prefs may change upon
// common real-life use cases:
//                                  AppLocale Backup Accepted
// Initial login                       -        A       -
// Sync                                B        A       -
// Accept (B)                          B        B       B
// -----------------------------------------------------------
// Initial login                       -        A       -
// No sync and second login            A        A       -
// Change options                      B        B       -
// -----------------------------------------------------------
// Initial login                       -        A       -
// Sync                                A        A       -
// Locale changed on login screen      A        C       -
// Accept (A)                          A        A       A
// -----------------------------------------------------------
// Initial login                       -        A       -
// Sync                                B        A       -
// Revert                              A        A       -
< ASCII >
```
## Visual type:
- #table 


== ./chromium/chromium_106.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/renderer/safe_browsing/phishing_dom_feature_extractor_browsertest.cc#L360-L363

```c
< ASCII >
// A page with nested iframes.
//         html
// iframe2 /  \ iframe1
//              \ iframe3
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_107.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/test/data/extensions/no_best_effort_tasks_test_extension/background.js#L12-L1106

```c
/*
What follows is garbage to make the total size of this file more than 64
kilobytes. This is to test the case where the NetworkService has to schedule
multiple tasks to feed this file through a mojo data pipe (when the extension
loads this background.js file resource).

See http://crbug.com/924416 for further details.
< ASCII >


|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
| _       _                                                                  |
|( )  _  ( )                                         _                       |
|| | ( ) | |   __      _ _    _ __   _     ___ ___  (_)  ___    __           |
|| | | | | | /'__`\   ( '_`\ ( '__)/'_`\ /' _ ` _ `\| |/',__) /'__`\         |
|| (_/ \_) |(  ___/   | (_) )| |  ( (_) )| ( ) ( ) || |\__, \(  ___/         |
|`\___x___/'`\____)   | ,__/'(_)  `\___/'(_) (_) (_)(_)(____/`\____)         |
|                     | |                                                    |
|                     (_)                                                    |
| _                    _                                                     |
|( )_                 ( )_                                                   |
|| ,_)   _        ___ | ,_)   _    _ _                                       |
|| |   /'_`\    /',__)| |   /'_`\ ( '_`\                                     |
|| |_ ( (_) )   \__, \| |_ ( (_) )| (_) )                                    |
|`\__)`\___/'   (____/`\__)`\___/'| ,__/'                                    |
|                                 | |                                        |
|                                 (_)                                        |
|                     _                        ___      _        _           |
|                  _ ( )_  _                  (  _`\   ( )      ( )          |
| _   _   _  _ __ (_)| ,_)(_)  ___     __     | ( (_)__| |__  __| |__        |
|( ) ( ) ( )( '__)| || |  | |/' _ `\ /'_ `\   | |  _(__   __)(__   __)       |
|| \_/ \_/ || |   | || |_ | || ( ) |( (_) |   | (_( )  | |      | |          |
|`\___x___/'(_)   (_)`\__)(_)(_) (_)`\__  |   (____/'  (_)      (_)          |
|                                   ( )_) |                                  |
|                                    \___/'                                  |
| _       _                _____                                             |
|(_ )  _ ( )              (___  )                                            |
| | | (_)| |/')    __         | |   _ _  _   _    _ _                        |
| | | | || , <   /'__`\    _  | | /'_` )( ) ( ) /'_` )                       |
| | | | || |\`\ (  ___/   ( )_| |( (_| || \_/ |( (_| | _                     |
|(___)(_)(_) (_)`\____)   `\___/'`\__,_)`\___/'`\__,_)(_)                    |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
|                                                                            |
*/
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_108.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/updater/policy/mac/managed_preference_policy_manager_impl.mm#L56-L71

```c
// For historical reasons, "update" policy has different enum values in Manage
// Preferences from the Device Management. This function converts the former
// to latter.
< ASCII >
// +----------------+---------------------+--------------------+
// | Update policy  | Managed Preferences |  Device Management |
// +----------------+---------------------+--------------------+
// | Enabled        |          0          |         1          |
// +----------------+---------------------+--------------------+
// | Automatic only |          1          |         3          |
// +----------------+---------------------+--------------------+
// | Manual only    |          2          |         2          |
// +----------------+---------------------+--------------------+
// | Disabled       |          3          |         0          |
// +----------------+---------------------+--------------------+
// | Machine only   |          4          |         4          |
// +----------------+---------------------+--------------------+
< ASCII >
```
## Visual type:
- #table 


== ./chromium/chromium_109.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromecast/media/audio/cast_audio_output_stream.h#L30-L89

```c
// Chromecast implementation of AudioOutputStream.
// This class forwards to MixerService if valid
// |mixer_service_connection_factory| is passed in on the construction call,
// for a lower latency audio playback (using MixerServiceWrapper). Otherwise,
// when a nullptr is passed in as |mixer_service_connection_factory| it forwards
// to CMA backend (using CmaWrapper).
//
// In either case, involved components live on two threads:
// 1. Audio thread
//    |CastAudioOutputStream|
//    Where the object gets construction from AudioManager.
//    How the object gets controlled from AudioManager.
// 2. Media thread or an IO thread opened within |AudioOutputStream|.
//    |CastAudioOutputStream::CmaWrapper| or |MixerServiceWrapper| lives on
//    this thread.
//
// The interface between AudioManager and AudioOutputStream is synchronous, so
// in order to allow asynchronous thread hops, we:
// * Maintain the current state independently in each thread.
// * Cache function calls like Start() and SetVolume() as bound callbacks.
//
// The individual thread states should nearly always be the same. The only time
// they are expected to be different is when the audio thread has executed a
// task and posted to the media thread/IO thread, but the media thread has not
// executed yet.
< ASCII >
//
// The below illustrates the case when CMA backend is used for playback.
//
//  Audio Thread |CAOS|                         Media Thread |CmaWrapper|
//  ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯                         ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯
//  *[ Closed ]                                 *[ Closed ]
//      |                                           |
//      |                                           |
//      v                                           v
//   [   Opened    ]    --post open-->           [   Opened    ]
//      |     |  ^                                  |     |  ^
//      |     |  |                                  |     |  |
//      |     | Stop()  --post stop-->              |     | Stop()
//      |     v  |                                  |     v  |
//      |  [ Started ]  --post start-->             |  [ Started ]
//      |     |                                     |     |
//      |     |                                     |     |
//      v     v                                     v     v
// **[ Pending Close ]  --post close-->        **[ Pending Close ]
//      |                                           |
//      |                                           |
//   ( waits for closure )         <--post closure--'
//      |
//      v
//   ( released)
// *  Initial states.
// ** Final states.
< ASCII >
//
// When MixerService is used in place of CMA backend, the state transition is
// similar but a little simpler.
// MixerServiceWrapper creates a new mixer service connection at Start() and
// destroys the connection at Stop(). When the volume is adjusted between a
// Stop() and the next Start(), the volume is recorded and then applied to the
// mixer service connection after the connection is established on the Start()
// call.
```
## Visual type:
- #state-machine


== ./chromium/chromium_11.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/display/touch_calibrator_view.cc#L254-L269

```c
< ASCII >
//   Circular      _________________________________
//   Throbber     |                                 |
//     View       |                                 |
//  ___________   |                                 |
// |           |  |                                 |
// |           |  |                                 |
// |     .     |  |            Hint Box             |
// |           |  |                                 |
// |___________|  |                                 |
//                |                                 |
//                |                                 |
//                |_________________________________|
//
// This view is set next to the throbber circle view such that their centers
// align. The hint box has a label text and a sublabel text to assist the
// user by informing them about the next step in the calibration process.
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_110.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromecast/ui/display_settings/screen_power_controller_aura.h#L13-L29

```c
< ASCII >
// This class implements the ScreenPowerController with AURA enabled. The class
// doesn't depend on AURA but it will only be available if |use_aura| build flag
// is true. The class wraps the logic of the following transition graph:
//
//          SetScreenOff() && !allow_screen_off
//     On <==================================> Brightness Off
//     /\             SetScreenOn()                 /\
//     ||                                           ||
//     ||                                           ||
//     ||                          allow_screen_off || !allow_screen_off
//     ||                                           ||
//     ||                                           ||
//     ||   SetScreenOff() && allow_screen_off      \/
//     ++=====================================> Power Off
//                     SetScreenOn()
//
//                        Screen Stage Transitions
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_111.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler.h#L29-L43

```c
< ASCII >
// This class watches for BETTER_TOGETHER_HOST to be disabled on a device and
// reacts by also disabling EASY_UNLOCK_HOST on that device. See
// https://crbug.com/881612.
//
// The flow of the class is as follows:
//
// Constructor --------------+
//                           |
//                           V                              (no host to disable)
// OnHostChangedOnBackend -->+---> DisableEasyUnlockHostIfNecessary ------> Done
//                           ^                |                               ^
//                           |                |                               |
//             (retry timer) |                |                               |
//                           |                V                    (success)  |
//                           +--- OnDisableEasyUnlockResult ------------------+
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_112.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler_unittest.cc#L218-L229

```c
< ASCII >
// Situation #1:
//   BTH = BETTER_TOGETHER_HOST, 0 = disabled, A = devices[0]
//   EUH = EASY_UNLOCK_HOST,     1 = enabled,  B = devices[1]
//
//    | A | B |           | A | B |
// ---+---+---+        ---+---+---+
// BTH| 1 | 0 |        BTH| 0 | 0 |
// ---+---+---+  --->  ---+---+---+
// EUH| 1 | 0 |        EUH| 1 | 0 |
//
// Grandfathering prevents EUH from being disabled automatically. This class
// disables EUH manually.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_113.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler_unittest.cc#L249-L261

```c
< ASCII >
// Situation #2:
//   BTH = BETTER_TOGETHER_HOST, 0 = disabled, A = devices[0]
//   EUH = EASY_UNLOCK_HOST,     1 = enabled,  B = devices[1]
//
//    | A | B |           | A | B |
// ---+---+---+        ---+---+---+
// BTH| 0 | 0 |        BTH| 0 | 1 |
// ---+---+---+  --->  ---+---+---+
// EUH| 1 | 0 |        EUH| 0 | 1 |
//
// The CryptAuth backend (via GmsCore) disables EUH on device A when BTH is
// enabled (exclusively) on another device, B. No action necessary from this
// class.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_114.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler_unittest.cc#L275-L287

```c
< ASCII >
// Situation #3:
//   BTH = BETTER_TOGETHER_HOST, 0 = disabled, A = devices[0]
//   EUH = EASY_UNLOCK_HOST,     1 = enabled,  B = devices[1]
//
//    | A | B |           | A | B |
// ---+---+---+        ---+---+---+
// BTH| 1 | 0 |        BTH| 0 | 1 |
// ---+---+---+  --->  ---+---+---+
// EUH| 1 | 0 |        EUH| 0 | 1 |
//
// The CryptAuth backend (via GmsCore) disables EUH on device A when BTH is
// enabled (exclusively) on another device, B. We still attempt to disable
// EUH in this case to be safe.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_115.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler_unittest.cc#L319-L325

```c
< ASCII >
// Situation #1 where device A is removed from list of synced devices:
//
//    | A | B |           | A | B |
// ---+---+---+        ---+---+---+
// BTH| 1 | 0 |        BTH| 0 | 0 |
// ---+---+---+  --->  ---+---+---+
// EUH| 1 | 0 |        EUH| 1 | 0 |
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_116.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/multidevice_setup/grandfathered_easy_unlock_host_disabler_unittest.cc#L342-L348

```c
< ASCII >
// Situation #1 with failure:
//
//    | A | B |           | A | B |
// ---+---+---+        ---+---+---+
// BTH| 1 | 0 |        BTH| 0 | 0 |
// ---+---+---+  --->  ---+---+---+
// EUH| 1 | 0 |        EUH| 1 | 0 |
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_117.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/ash/services/secure_channel/ble_weave_client_connection.h#L47-L58

```c
< ASCII >
// Creates GATT connection on top of the BLE connection and act as a Client.
// uWeave communication follows the flow:
//
// Client                           | Server
// ---------------------------------|--------------------------------
// send connection request          |
//                                  | receive connection request
//                                  | send connection response
// receive connection response      |
// opt: send data                   | opt: send data
// receive data                     | receive data
// opt: close connection            | opt: close connection
< ASCII >
```
## Visual type:
- #table 


== ./chromium/chromium_118.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chromeos/printing/printer_configuration.h#L32-L49

```c
< ASCII >
// This function checks if the given URI is a valid printer URI. |uri| is
// considered to be a valid printer URI if it has one of the scheme listed in
// the table below and meets the criteria defined there.
//
//  scheme  | userinfo |   host   |   port   |   path   |  query   | fragment
// ---------+----------+----------+----------+----------+----------+----------
//   http   |    NO    | required | optional | optional | optional |    NO
//   https  |    NO    | required | optional | optional | optional |    NO
//   ipp    |    NO    | required | optional | optional | optional |    NO
//   ipps   |    NO    | required | optional | optional | optional |    NO
//   lpd    | optional | required | optional | optional | optional |    NO
//   socket |    NO    | required | optional |    NO    | optional |    NO
//   ippusb |    NO    | required |    NO    | required | optional |    NO
//   usb    |    NO    | required |    NO    | required | optional |    NO
//
// If the given |uri| does not meet the criteria the function returns false and
// set an error message in |error_message| (if it is not nullptr). The message
// has the prefix "Malformed printer URI: ".
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_119.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/content/browser/content_autofill_router.h#L28-L137

```c
// ContentAutofillRouter routes events between ContentAutofillDriver objects in
// order to handle frame-transcending forms.
//
// A *frame-transcending* form is a form whose fields live in different frames.
// For example, credit card forms often have the credit card number field in an
// iframe hosted by a payment service provider.
//
// A frame-transcending form therefore consists of multiple *renderer forms*.
// ContentAutofillRouter *flattens* these forms into a single *browser form*,
// and maps all events concerning the renderer forms to that browser form, and
// vice versa.
//
// That way, the collection of renderer forms appears as one ordinary form to
// the browser.
//
// For example, consider the following pseudo HTML code:
//   <html>
//   <form id="Form-1">
//     <input id="Field-1">
//     <iframe id="Frame-1">
//       <input id="Field-2">
//     </iframe>
//     <iframe id="Frame-2">
//       <iframe id="Frame-3">
//         <form id="Form-2">
//           <input id="Field-3">
//         </form>
//         <form id="Form-3">
//           <input id="Field-4">
//         </form>
//       </iframe>
//     </iframe>
//     <input id="Field-5">
//   </form>
//
// Forms can be actual <form> elements or synthetic forms: <input>, <select>,
// and <iframe> elements that are not in the scope of any <form> belong to the
// enclosing frame's synthetic form.
//
// The five renderer forms are therefore, in pseudo C++ code:
//   FormData{
//     .host_frame = "Frame-0",  // The main frame.
//     .name = "Form-1",
//     .fields = { "Field-1", "Field-5" },
//     .child_frames = { "Frame-1", "Frame-2" }
//   }
//   FormData{
//     .host_frame = "Frame-1",
//     .name = "synthetic",
//     .fields = { "Field-2" },
//     .child_frames = { }
//   }
//   FormData{
//     .host_frame = "Frame-2",
//     .name = "synthetic",
//     .fields = { },
//     .child_frames = { "Frame-3" }
//   }
//   FormData{
//     .host_frame = "Frame-3",
//     .name = "Form-2",
//     .fields = { "Field-3" },
//     .child_frames = { }
//   }
//   FormData{
//     .host_frame = "Frame-3",
//     .name = "Form-3",
//     .fields = { "Field-4" },
//     .child_frames = { }
//   }
//
// The browser form of these renderer forms is obtained by flattening the fields
// into the root form:
//   FormData{
//     .name = "Form-1",
//     .fields = { "Field-1", "Field-2", "Field-3", "Field-4", "Field-5" }
//   }
//
< ASCII >
// Let AutofillAgent-N, ContentAutofillRouter-N, and AutofillManager-N
// correspond to the Frame-N. ContentAutofillRouter would route an event
// concerning any of the forms in Frame-3 from ContentAutofillDriver-3 to
// ContentAutofillDriver-0:
//
//   +---Tab---+            +---Tab----+            +----Tab----+
//   | Agent-0 |      +---> | Driver-0 | ---------> | Manager-0 |
//   |         |      |     |          |            |           |
//   | Agent-1 |      |     | Driver-1 |            | Manager-1 |
//   |         |      |     |          |            |           |
//   | Agent-2 |      |     | Driver-2 |            | Manager-2 |
//   |         |      |     |          |            |           |
//   | Agent-3 | -----|---> | Driver-3 | -----+     | Manager-3 |
//   +---------+      |     +----------+      |     +-----------+
//                    |                       |
//                    |      +--Tab---+       |
//                    +----- | Router | <-----+
//                           +--------+
//
// If the event name is `f`, the control flow is as follows:
//   Driver-3's ContentAutofillDriver::f(args...) calls
//   Router's   ContentAutofillRouter::f(this, args..., callback) calls
//   Driver-0's ContentAutofillDriver::callback(args...).
< ASCII >
//
// Every function in ContentAutofillRouter takes a |source| parameter, which
// points to the ContentAutofillDriver that triggered the event. In events
// triggered by the renderer, the source driver is the driver the associated
// renderer form originates from.
//
// See ContentAutofillDriver for details on the naming pattern and an example.
//
// See FormForest for details on (un)flattening.
```
## Visual type:
- #custom


== ./chromium/chromium_12.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/fast_ink/laser/laser_pointer_view.cc#L45-L63

```c
// The laser segment calcuates the path needed to draw a laser segment. A laser
// segment is used instead of just a regular line segments to avoid overlapping.
< ASCII >
// A laser segment looks as follows:
//    _______         _________       _________        _________
//   /       \        \       /      /         /      /         \       |
//   |   A   |       2|.  B  .|1    2|.   C   .|1    2|.   D     \.1    |
//   |       |        |       |      |         |      |          /      |
//    \_____/         /_______\      \_________\      \_________/       |
//
//
// Given a start and end point (represented by the periods in the above
// diagrams), we create each segment by projecting each point along the normal
// to the line segment formed by the start(1) and end(2) points. We then
// create a path using arcs and lines. There are three types of laser segments:
// head(B), regular(C) and tail(D). A typical laser is created by rendering one
// tail(D), zero or more regular segments(C), one head(B) and a circle at the
// end(A). They are meant to fit perfectly with the previous and next segments,
// so that no whitespace/overlap is shown.
// A more detailed version of this is located at https://goo.gl/qixdux.
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_120.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/content/browser/form_forest.h#L22-L146

```c
// FormForest converts renderer forms into a browser form and vice versa.
//
// A *frame-transcending* form is a form whose logical fields live in different
// frames. The *renderer forms* are the FormData objects as we receive them from
// the renderers of these frames. The *browser form* of a frame-transcending
// form is its root FormData, with all fields of its descendant FormDatas moved
// into the root.
// See ContentAutofillRouter for further details on the terminology and
// motivation.
< ASCII >
//
// Consider the following main frame with two frame-transcending forms:
//
//                    +--------------- Frame ---------------+
//                    |                                     |
//             +--- Form-A --+                       +--- Form-C --+
//             |      |      |                       |             |
//          Field-1 Frame Field-4                  Frame         Frame
//                    |                              |             |
//             +--- Form-B --+                     Form-D        Form-E
//             |             |                       |             |
//          Field-2       Field-3                 Field-5        Frame
//                                                                 |
//                                                               Form-F
//                                                                 |
//                                                              Field-6
//
< ASCII >
// The renderer forms are form A, B, C, D, E, F.
//
// The browser form of forms A and B has the fields fields 1, 2, 3, 4.
// Converting this browser form back to renderer forms yields Form-A and Form-B.
//
// Analogously, the browser form of forms C, D, E, and F has the fields 5 and 6.
// Converting this browser form back to renderer forms yields forms C, D, E, F.
//
// The three key functions of FormForest are:
// - UpdateTreeOfRendererForm()
// - GetBrowserFormOfRendererForm()
// - GetRendererFormsOfBrowserForm()
//
// UpdateTreeOfRendererForm() incrementally builds up a graph of frames, forms,
// and fields.
//
// This graph is a forest in two (entirely independent) ways:
//
// Firstly, there may be multiple root frames. One reason is the website author
// can disconnect an entire frame subtree from the rest of the frame tree in the
// future using the fencedframes tag and/or disallowdocumentaccess attribute.
// Another reason is that the frame hierarchy emerges gradually and therefore
// some links may be unknown. For example, Form-A might point to a nonexistent
// frame of Form-B because, after Form-A was last parsed, a cross-origin
// navigation happened in Form-B's frame.
//
// Secondly, removing the root frames obtains a forest, where each tree
// corresponds to a frame-transcending form. We call the roots of this forest
// *root forms*. In the example, forms A and C are root forms. This is relevant
// because filling operations happen on the granularity of root forms.
//
< ASCII >
// As an invariant, UpdateTreeOfRendererForm() keeps each frame-transcending
// form in a flattened state: fields are stored as children of their root
// forms. The fields are ordered according to pre-order depth-first (DOM order)
// traversal of the original tree. In our example:
//
//                    +--------------- Frame ---------------+
//                    |                                     |
//    +-------+---- Form-A ---+-------+        +-------+- Form-C -+-------+
//    |       |       |       |       |        |       |          |       |
// Field-1 Field-2 Field-3 Field-4  Frame   Field-5 Field-6     Frame   Frame
//                                    |                           |       |
//                                  Form-B                      Form-D  Form-E
//                                                                        |
//                                                                      Frame
//                                                                        |
//                                                                      Form-F
//
// There is no meaningful order between the fields and frames in these flattened
// forms.
< ASCII >
//
// GetBrowserFormOfRendererForm(renderer_form) simply retrieves the form node
// of |renderer_form| and returns the root form, along with its field children
// For example, if |renderer_form| is form B, it returns form A with fields 1–4.
//
// GetRendererFormsOfBrowserForm(browser_form) returns the individual renderer
// forms that constitute |browser_form|, with their fields reinstated. For
// example, if |browser_form| has fields 1–4, it returns form A with fields 1
// and 4, and form B with fields 2 and 3.
//
// The node types in the forest always alternate as follows:
// - The root nodes are frames.
// - The children of frames are forms.
// - The children of forms are frames or fields.
// - Fields are leaves. Forms and frames may be leaves.
//
// Frames, forms, and fields are represented as FrameData, FormData, and
// FormFieldData objects respectively. The graph is stored as follows:
// - All FrameData nodes are stored directly in FormForest::frame_datas_.
// - The FrameData -> FormData edges are stored in the FrameData::child_forms
//   vector of FormData objects. That is, each FrameData directly holds its
//   children.
// - The FormData -> FrameData edges are stored in the FormData::child_frames
//   vector of FrameTokens. To retrieve the actual FrameData child, the token
//   is resolved to a LocalFrameToken if necessary (see Resolve()), and then
//   looked up in FormForest::frame_datas_.
// - The FormData -> FormFieldData edges are stored in the FormData::fields
//   vector of FormFieldData objects. As per the aforementioned invariant,
//   fields are only stored in root forms. Each field's original parent can be
//   identified by FormFieldData::host_frame and FormFieldData::host_form_id.
//
// Reasonable usage of FormForest follows this protocol:
// 1. Call any of the functions only for forms and fields which have the
//    following attributes set:
//    - FormData::host_frame
//    - FormData::unique_renderer_id
//    - FormFieldData::host_frame
//    - FormFieldData::unique_renderer_id
//    - FormFieldData::host_form_id
// 2. Call UpdateTreeOfRendererForm(renderer_form) whenever a renderer form is
//    seen to make FormForest aware of the (new or potentially changed) form.
// 3. Call GetBrowserFormOfRendererForm(renderer_form) only after a preceding
//    UpdateTreeOfRendererForm(some_renderer_form) call where |renderer_form|
//    typically is identical to |some_renderer_form|, but technically it
//    suffices if both forms have the same global_id().
// 4. Call GetRendererFormsOfBrowserForm(browser_form) only if |browser_form|
//    was previously returned by GetBrowserFormOfRendererForm(), perhaps with
//    different FormFieldData::value, FormFieldData::is_autofilled.
// Items 1 and 3 are mandatory for FormForest to be memory-safe.
```
## Visual type:
- #tree


== ./chromium/chromium_121.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/core/browser/form_structure_sectioning_util.h#L18-L114

```c
// Sectioning is the process of splitting a form into logical groups (e.g.
// shipping, billing, credit card section) which are filled separately.
//
// After this function has finished, the following conditions hold:
//  1. Every field with an autocomplete attribute section S is in section S.
//  2. All credit card fields are in one, distinct section, unless they have a
//     valid autocomplete attribute section.
//  3. All other fields that are focusable or <select> fields are partitioned
//     into intervals, each of which is a section.
//  4. All remaining fields are in one, distinct section.
//
// The basic idea of interval partitioning is to start a new section when the
// same field type appears repeatedly. See `ShouldStartNewSection()` for the
// details.
//
// The motivation behind the special handling of credit card fields is that
// credit card forms frequently contain multiple fields of the same type, some
// of which are invisible. In such cases, repeated field types must not start a
// new section.
< ASCII >
//
// Example:
//   ------------------------------------------------------+-------------------
//       HTML code                                         |      Section
//   ------------------------------------------------------+-------------------
//   Name:      <input id=1>                               | field 1 based
//   Country:   <input id=2>                               | field 1 based
//   Name:      <input id=3 autocomplete=”section-A name”> | A
//   Street:    <input id=4>                               | field 1 based
//   CC number: <input id=5>                               | field 5 based
//   CC number: <input id=6 style="display:none">          | field 5 based
//   Name:      <input id=7>                               | field 7 based
//   Country:   <input id=8>                               | field 7 based
//   CC number: <input id=9>                               | field 5 based
//   ------------------------------------------------------+-------------------
//
// The `kAutofillUseParameterizedSectioning` has boolean feature parameters
// which assign sections in the following way:
// a. `kAutofillSectioningModeIgnoreAutocomplete`: Disables condition 1, i.e.
//    ignores the autocomplete attribute section. Otherwise, conditions 2 to 4
//    apply.
//
//    Example:
//   ------------------------------------------------------+-------------------
//       HTML code                                         |      Section
//   ------------------------------------------------------+-------------------
//   Name:      <input id=1>                               | field 1 based
//   Country:   <input id=2>                               | field 1 based
//   Name:      <input id=3 autocomplete=”section-A name”> | field 3 based
//   Street:    <input id=4>                               | field 3 based
//   CC number: <input id=5>                               | field 5 based
//   CC number: <input id=6 style="display:none">          | field 5 based
//   Name:      <input id=7>                               | field 7 based
//   Country:   <input id=8>                               | field 7 based
//   CC number: <input id=9>                               | field 5 based
//   ------------------------------------------------------+-------------------
//
// b. `kAutofillSectioningModeCreateGaps`: The conditions from above apply.
//    Additionally, in condition 3, intervals are split by fields that were
//    sectioned based on conditions 1 and 2.
//
//    Example:
//   ------------------------------------------------------+-------------------
//       HTML code                                         |      Section
//   ------------------------------------------------------+-------------------
//   Name:      <input id=1>                               | field 1 based
//   Country:   <input id=2>                               | field 1 based
//   Name:      <input id=3 autocomplete=”section-A name”> | A
//   Street:    <input id=4>                               | field 4 based
//   CC number: <input id=5>                               | field 5 based
//   CC number: <input id=6 style="display:none">          | field 5 based
//   Name:      <input id=7>                               | field 7 based
//   Country:   <input id=8>                               | field 7 based
//   CC number: <input id=9>                               | field 5 based
//   ------------------------------------------------------+-------------------
//
// c. `kAutofillSectioningModeExpand`: The conditions from above apply.
//    Additionally, we look at gaps between fields with sections based on
//    conditions 1 and 2. If the gap is enclosed by the same section, this
//    section is expanded to all the fields inside the gap. If there are no such
//    gaps, this mode degenerates to the regular sectioning algorithm.
//    In the example below, the "Name" and "Country" fields between the last two
//    credit card fields are affected.
//
//    Example:
//   ------------------------------------------------------+-------------------
//       HTML code                                         |      Section
//   ------------------------------------------------------+-------------------
//   Name:      <input id=1>                               | field 1 based
//   Country:   <input id=2>                               | field 1 based
//   Name:      <input id=3 autocomplete=”section-A name”> | A
//   Street:    <input id=4>                               | field 1 based
//   CC number: <input id=5>                               | field 5 based
//   CC number: <input id=6 style="display:none">          | field 5 based
//   Name:      <input id=7>                               | field 5 based
//   Country:   <input id=8>                               | field 5 based
//   CC number: <input id=9>                               | field 5 based
//   ------------------------------------------------------+-------------------
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_122.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/core/browser/data_model/autofill_structured_address_name.h#L78-L93

```c
< ASCII >
// Compound that represent a last name. It contains a first and second last name
// and a conjunction as it is used in Hispanic/Latinx names. Note, that compound
// family names like Miller-Smith are not supposed to be split up into two
// components. If a name contains only a single component, the component is
// stored in the second part by default.
//
//               +-------+
//               | _LAST |
//               +--------
//               /    |    \
//             /      |      \
//           /        |        \
// +--------+ +-----------+ +---------+
// | _FIRST | | _CONJUNC. | | _SECOND |
// +--------+ +-----------+ +---------+
//
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_123.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/core/browser/data_model/autofill_structured_address_name.h#L111-L130

```c
< ASCII >
// Compound that represents a full name. It contains a honorific, a first
// name, a middle name and a last name. The last name is a compound itself.
//
//                     +------------+
//                     | NAME_FULL  |
//                     +------------+
//                    /       |      \
//                   /        |       \
//                  /         |        \
//    +------------+  +-------------+   +-----------+
//    | NAME_FIRST |  | NAME_MIDDLE |   | NAME_LAST |
//    +------------+  +-------------+   +-----------+
//                                     /      |      \
//                                    /       |       \
//                                   /        |        \
//                                  /         |         \
//                          +--------+ +--------------+ +---------+
//                          | _FIRST | | _CONJUNCTION | | _SECOND |
//                          +--------+ +--------------+ +---------+
//
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_124.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/autofill/core/browser/data_model/autofill_structured_address_name.h#L160-L185

```c
< ASCII >
// Compound that represent a full name and a honorific prefix.
//
//             +-----------------------+
//             | NAME_FULL_WITH_PREFIX |
//             +-----------------------+
//                   /            \
//                  /              \
//                 /                \
//                /                  \
//   +-------------------+      +------------+
//   | HONORIFIC_PREFIX  |      | NAME_FULL  |
//   +-------------------+      +------------+
//                             /       |      \
//                            /        |       \
//                           /         |        \
//             +------------+  +-------------+   +-----------+
//             | NAME_FIRST |  | NAME_MIDDLE |   | NAME_LAST |
//             +------------+  +-------------+   +-----------+
//                                              /      |      \
//                                             /       |       \
//                                            /        |        \
//                                           /         |         \
//                                   +--------+ +--------------+ +---------+
//                                   | _FIRST | | _CONJUNCTION | | _SECOND |
//                                   +--------+ +--------------+ +---------+
//
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_125.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/browser_ui/widget/android/java/src/org/chromium/components/browser_ui/widget/RadioButtonWithDescriptionLayout.java#L20-L52

```c
/**
< ASCII >
 * <p>
 * Manages a group of exclusive RadioButtonWithDescriptions. Has the option to set an accessory view
 * on any given RadioButtonWithDescription. Only one accessory view per layout is supported.
 * <pre>
 * -------------------------------------------------
 * | O | MESSAGE #1                                |
 *        description_1                            |
 *        [optional] accessory view                |
 * | O | MESSAGE #N                                |
 *        description_n                            |
 * -------------------------------------------------
 * </pre>
 * </p>
< ASCII >
 *
 * <p>
 * To declare in XML, define a RadioButtonWithDescriptionLayout that contains
 * RadioButtonWithDescription, RadioButtonWithDescriptionAndAuxButton and/or
 * RadioButtonWithEditText children. For example:
 * <pre>{@code
 *  <org.chromium.components.browser_ui.widget.RadioButtonWithDescriptionLayout
 *       android:layout_width="match_parent"
 *       android:layout_height="match_parent" >
 *       <org.chromium.components.browser_ui.widget.RadioButtonWithDescription
 *           ... />
 *       <org.chromium.components.browser_ui.widget.RadioButtonWithDescriptionAndAuxButton
 *           ... />
 *       <org.chromium.components.browser_ui.widget.RadioButtonWithEditText
 *           ... />
 *   </org.chromium.components.browser_ui.widget.RadioButtonWithDescriptionLayout>
 * }</pre>
 * </p>
 */
```
## Visual type:
- #gui


== ./chromium/chromium_126.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/cast/message_port/blink_message_port_adapter.cc#L14-L48

```c
// MessagePortAdapters are used to adapt between two different implementations
// of cast_api_bindings::MessagePort.
//
< ASCII >
// PostMessageWithTransferables flow including adaptation:
//+---+     +-------+    +---------+        +---------+   +-------+   +---+
//| A |     | PortA |    | AdptrA  |        | AdptrB  |   | PortB |   | B |
//+---+     +-------+    +---------+        +---------+   +-------+   +---+
//  | Post      |             |                  |            |         |
//  |---------->|             |                  |            |         |
//  |           | OnMsg       |                  |            |         |
//  |           |------------>|                  |            |         |
//  |           |             | Adapt Ports      |            |         |
//  |           |             |-----------|      |            |         |
//  |           |             |<----------|      |            |         |
//  |           |             | Post             |            |         |
//  |           |             |----------------->|            |         |
//  |           |             |                  | OnMsg      |         |
//  |           |             |                  |----------->|         |
//  |           |             |                  |            | OnMsg   |
//  |           |             |                  |            |-------->|
//
// Error flow including deletion, for example when OnMessage fails
//  |           |             |                  |            |   false |
//  |           |             |                  |            |<--------|
//  |           |             |                  |      OnErr |         |
//  |           |             |                  |<-----------|         |
//  |           |             |           delete |            |         |
//  |           |             |<-----------------|            |         |
//  |           |      delete |                  |            |         |
//  |           |<------------|                  |            |         |
//  |     OnErr |             |                  |            |         |
//  |<----------|             |                  |            |         |
//  |           |             |                  | delete     |         |
//  |           |             |                  |------|     |         |
//  |           |             |                  |<-----|     |         |
< ASCII >
```
## Visual type:
- #sequence-diagram 


== ./chromium/chromium_127.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/embedder_support/android/metrics/android_metrics_service_client.h#L38-L94

```c
// AndroidMetricsServiceClient is a singleton which manages metrics collection
// intended for use by WebView & WebLayer.
//
// Metrics should be enabled iff all these conditions are met:
//  - The user has not opted out.
//  - The app has not opted out.
//  - This client is in the 10% sample (controlled by client ID hash).
// The first two are recorded in |user_consent_| and |app_consent_|, which are
// set by SetHaveMetricsConsent(). The last is recorded in |is_in_sample_|.
//
// Metrics are pseudonymously identified by a randomly-generated "client ID".
// AndroidMetricsServiceClient stores this in prefs, written to the app's data
// directory. There's a different such directory for each user, for each app,
// on each device. So the ID should be unique per (device, app, user) tuple.
//
// In order to be transparent about not associating an ID with an opted out user
// or app, the client ID should only be created and retained when neither the
// user nor the app have opted out. Otherwise, the presence of the ID could give
// the impression that metrics were being collected.
< ASCII >
//
// AndroidMetricsServiceClient metrics set up happens like so:
//
//   startup
//      │
//      ├────────────┐
//      │            ▼
//      │         query for consent
//      ▼            │
//   Initialize()    │
//      │            ▼
//      │         SetHaveMetricsConsent()
//      │            │
//      │ ┌──────────┘
//      ▼ ▼
//   MaybeStartMetrics()
//      │
//      ▼
//   MetricsService::Start()
//
// All the named functions in this diagram happen on the UI thread. Querying GMS
// happens in the background, and the result is posted back to the UI thread, to
// SetHaveMetricsConsent(). Querying GMS is slow, so SetHaveMetricsConsent()
// typically happens after Initialize(), but it may happen before.
//
// Each path sets a flag, |init_finished_| or |set_consent_finished_|, to show
// that path has finished, and then calls MaybeStartMetrics(). When
// MaybeStartMetrics() is called the first time, it sees only one flag is true,
// and does nothing. When MaybeStartMetrics() is called the second time, it
// decides whether to start metrics.
//
// If consent was granted, MaybeStartMetrics() determines sampling by hashing
// the client ID (generating a new ID if there was none). If this client is in
// the sample, it then calls MetricsService::Start(). If consent was not
// granted, MaybeStartMetrics() instead clears the client ID, if any.
< ASCII >
//
// To match chrome on other platforms (including android), the MetricsService is
// always created.
```
## Visual type:
- #flowchart


== ./chromium/chromium_128.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/feedback/redaction_tool/ip_address.h#L274-L292

```c
< ASCII >
// According to RFC6052 Section 2.2 IPv4-Embedded IPv6 Address Format.
// https://www.rfc-editor.org/rfc/rfc6052#section-2.2
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |PL| 0-------------32--40--48--56--64--72--80--88--96--104---------|
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |32|     prefix    |v4(32)         | u | suffix                    |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |40|     prefix        |v4(24)     | u |(8)| suffix                |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |48|     prefix            |v4(16) | u | (16)  | suffix            |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |56|     prefix                |(8)| u |  v4(24)   | suffix        |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |64|     prefix                    | u |   v4(32)      | suffix    |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
// |96|     prefix                                    |    v4(32)     |
// +--+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
//
// The NAT64/DNS64 translation prefixes has one of the following lengths.
< ASCII >
```
## Visual type:
- #memory-layout  


== ./chromium/chromium_129.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/omnibox/browser/autocomplete_provider.h#L27-L151

```c
// The AutocompleteProviders each return different kinds of matches,
// such as history or search matches.  These matches are given
// "relevance" scores.  Higher scores are better matches than lower
// scores.  The relevance scores and classes providing the respective
// matches are as listed below.
// 
< ASCII >
// IMPORTANT CAVEAT: The tables below are NOT COMPLETE.  Developers
// often forget to keep these tables in sync with the code when they
// change scoring algorithms or add new providers.  For example,
// neither the HistoryQuickProvider (which is a provider that appears
// often) nor the ShortcutsProvider are listed here.  For the best
// idea of how scoring works and what providers are affecting which
// queries, play with chrome://omnibox/ for a while.  While the tables
// below may have some utility, nothing compares with first-hand
// investigation and experience.
//
// ZERO SUGGEST (empty input type) on NTP:
// --------------------------------------------------------------------|-----
// Query Tiles (Android only)                                          |  1599
// Clipboard (Mobile only)                                             |  1501
// Remote Zero Suggest (relevance expected to be overridden by server) |  100
// Local History Zero Suggest (signed-out users)                       |  1450--
// Local History Zero Suggest (signed-in users)                        |  500--
//
// ZERO SUGGEST (empty input type) on SERP:
// --------------------------------------------------------------------|-----
// Verbatim Match (Mobile only)                                        |  1600
// Clipboard (Mobile only)                                             |  1501
//
// ZERO SUGGEST (empty input type) on OTHER (e.g., contextual web):
// --------------------------------------------------------------------|-----
// Verbatim Match (Mobile only)                                        |  1600
// Clipboard (Mobile only)                                             |  1501
// Most Visited Carousel (Android only)                                |  1500
// Most Visited Sites (Mobile only)                                    |  600--
// Remote Zero Suggest (relevance expected to be overridden by server) |  100
//
// UNKNOWN input type:
// --------------------------------------------------------------------|-----
// Keyword (non-substituting or in keyword UI mode, exact match)       | 1500
// HistoryURL (good exact or inline autocomplete matches, some inexact)| 1410++
// HistoryURL (intranet url never visited match, some inexact matches) | 1400++
// Search Primary Provider (past query in history within 2 days)       | 1399**
// Search Primary Provider (what you typed)                            | 1300
// HistoryURL (what you typed, some inexact matches)                   | 1200++
// Keyword (substituting, exact match)                                 | 1100
// Search Primary Provider (past query in history older than 2 days)   | 1050*
// HistoryURL (some inexact matches)                                   |  900++
// BookmarkProvider (prefix match in bookmark title or URL)            |  900+-
// Built-in                                                            |  860++
// Search Primary Provider (navigational suggestion)                   |  800++
// Search Primary Provider (suggestion)                                |  600++
// Keyword (inexact match)                                             |  450
// Search Secondary Provider (what you typed)                          |  250
// Search Secondary Provider (past query in history)                   |  200*
// Search Secondary Provider (navigational suggestion)                 |  150++
// Search Secondary Provider (suggestion)                              |  100++
// Non Personalized On Device Head Suggest Provider                    |    *
//                  (default value 99--, can be changed by Finch)
// Document Suggestions (*experimental): value controlled by Finch     |    *
//
// URL input type:
// --------------------------------------------------------------------|-----
// Keyword (non-substituting or in keyword UI mode, exact match)       | 1500
// HistoryURL (good exact or inline autocomplete matches, some inexact)| 1410++
// HistoryURL (intranet url never visited match, some inexact matches) | 1400++
// HistoryURL (what you typed, some inexact matches)                   | 1200++
// Keyword (substituting, exact match)                                 | 1100
// HistoryURL (some inexact matches)                                   |  900++
// Built-in                                                            |  860++
// Search Primary Provider (what you typed)                            |  850
// Search Primary Provider (navigational suggestion)                   |  800++
// Search Primary Provider (past query in history)                     |  750*
// Keyword (inexact match)                                             |  700
// Search Primary Provider (suggestion)                                |  300++
// Search Secondary Provider (what you typed)                          |  250
// Search Secondary Provider (past query in history)                   |  200*
// Search Secondary Provider (navigational suggestion)                 |  150++
// Search Secondary Provider (suggestion)                              |  100++
// Non Personalized On Device Head Suggest Provider                    |   99--
//
// QUERY input type:
// --------------------------------------------------------------------|-----
// Search Primary or Secondary (past query in history within 2 days)   | 1599**
// Keyword (non-substituting or in keyword UI mode, exact match)       | 1500
// Keyword (substituting, exact match)                                 | 1450
// Search Primary Provider (past query in history within 2 days)       | 1399**
// Search Primary Provider (what you typed)                            | 1300
// Search Primary Provider (past query in history older than 2 days)   | 1050*
// HistoryURL (inexact match)                                          |  900++
// BookmarkProvider (prefix match in bookmark title or URL)            |  900+-
// Search Primary Provider (navigational suggestion)                   |  800++
// Search Primary Provider (suggestion)                                |  600++
// Keyword (inexact match)                                             |  450
// Search Secondary Provider (what you typed)                          |  250
// Search Secondary Provider (past query in history)                   |  200*
// Search Secondary Provider (navigational suggestion)                 |  150++
// Search Secondary Provider (suggestion)                              |  100++
// Non Personalized On Device Head Suggest Provider                    |    *
//                  (default value 99--, can be changed by Finch)
//
// (A search keyword is a keyword with a replacement string; a bookmark keyword
// is a keyword with no replacement string, that is, a shortcut for a URL.)
< ASCII >
//
// There are two possible providers for search suggestions. If the user has
// typed a keyword, then the primary provider is the keyword provider and the
// secondary provider is the default provider. If the user has not typed a
// keyword, then the primary provider corresponds to the default provider.
//
// Search providers may supply relevance values along with their results to be
// used in place of client-side calculated values.
//
// The value column gives the ranking returned from the various providers.
// ++: a series of matches with relevance from n up to (n + max_matches).
// --: a series of matches with relevance from n down to (n - max_matches).
// *:  relevance score falls off over time (discounted 50 points @ 15 minutes,
//     450 points @ two weeks)
// **: relevance score falls off over two days (discounted 99 points after two
//     days).
// +-: A base score that the provider will adjust upward or downward based on
//     provider-specific metrics.
//
// A single result provider for the autocomplete system.  Given user input, the
// provider decides what (if any) matches to return, their relevance, and their
// classifications.
```
## Visual type:
- #table


== ./chromium/chromium_13.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/login/ui/login_pin_view.h#L24-L50

```c
// Implements a PIN keyboard. The class emits high-level events that can be used
// by the embedder. The PIN keyboard, while displaying letters, only emits
// numbers.
//
// The view is always rendered via layers.
//
< ASCII >
// The UI looks a little like this:
//    _______    _______    _______
//   |   1   |  |   2   |  |   3   |
//   |       |  | A B C |  | D E F |
//    -------    -------    -------
//    _______    _______    _______
//   |   4   |  |   5   |  |   6   |
//   | G H I |  | J K L |  | M N O |
//    -------    -------    -------
//    _______    _______    _______
//   |   7   |  |   8   |  |   9   |
//   |P Q R S|  | T U V |  |W X Y Z|
//    -------    -------    -------
//    _______    _______    _______
//   |  <-   |  |   0   |  |  ->   |
//   |       |  |   +   |  |       |
//    -------    -------    -------
//
// The <- represents the delete button while -> represents the submit button.
// The submit button is optional.
< ASCII >
//
```
## Visual type:
- #gui


== ./chromium/chromium_130.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/omnibox/browser/history_url_provider.h#L38-L93

```c
// How history autocomplete works
// ==============================
//
< ASCII >
// Read down this diagram for temporal ordering.
//
//   Main thread                History thread
//   -----------                --------------
//   AutocompleteController::Start
//     -> HistoryURLProvider::Start
//       -> VerbatimMatchForInput
//       [params_ allocated]
//       -> DoAutocomplete (for inline autocomplete)
//         -> URLDatabase::AutocompleteForPrefix (on in-memory DB)
//       -> HistoryService::ScheduleAutocomplete
//       (return to controller) ----
//                                 /
//                            HistoryBackend::ScheduleAutocomplete
//                              -> HistoryURLProvider::ExecuteWithDB
//                                -> DoAutocomplete
//                                  -> URLDatabase::AutocompleteForPrefix
//                              /
//   HistoryService::QueryComplete
//     [params_ destroyed]
//     -> AutocompleteProviderListener::OnProviderUpdate
< ASCII >
//
// The autocomplete controller calls us, and must be called back, on the main
// thread.  When called, we run two autocomplete passes.  The first pass runs
// synchronously on the main thread and queries the in-memory URL database.
// This pass promotes matches for inline autocomplete if applicable.  We do
// this synchronously so that users get consistent behavior when they type
// quickly and hit enter, no matter how loaded the main history database is.
// Doing this synchronously also prevents inline autocomplete from being
// "flickery" in the AutocompleteEdit.  Because the in-memory DB does not have
// redirect data, results other than the top match might change between the
// two passes, so we can't just decide to use this pass' matches as the final
// results.
//
// The second autocomplete pass uses the full history database, which must be
// queried on the history thread.  Start() asks the history service schedule to
// callback on the history thread with a pointer to the main database.  When we
// are done doing queries, we schedule a task on the main thread that notifies
// the AutocompleteController that we're done.
//
// The communication between these threads is done using a
// HistoryURLProviderParams object.  This is allocated in the main thread, and
// normally deleted in QueryComplete().  So that both autocomplete passes can
// use the same code, we also use this to hold results during the first
// autocomplete pass.
//
// While the second pass is running, the AutocompleteController may cancel the
// request.  This can happen frequently when the user is typing quickly.  In
// this case, the main thread sets params_->cancel, which the background thread
// checks periodically.  If it finds the flag set, it stops what it's doing
// immediately and calls back to the main thread.  (We don't delete the params
// on the history thread, because we should only do that when we can safely
// NULL out params_, and that must be done on the main thread.)
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_131.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/omnibox/browser/on_device_head_model_unittest.cc#L20-L50

```c
< ASCII >
// The test head model used for unittests contains 14 queries and their scores
// shown below; the test model uses 3-bytes address and 2-bytes score so the
// highest score is 32767:
// ----------------------
// Query            Score
// ----------------------
// g                32767
// gmail            32766
// google maps      32765
// google           32764
// get out          32763
// googler          32762
// gamestop         32761
// maps             32761
// mail             32760
// map              32759
// 谷歌              32759
// ガツガツしてる人    32759
// 비데 두꺼비         32759
// переводчик       32759
// ----------------------
// The tree structure for queries above is similar as this:
//  [ g | ma | 谷歌 | ガツガツしてる人| 비데 두꺼비 | переводчик ]
//    |   |
//    | [ p | il ]
//    |   |
//    | [ # | s ]
//    |
//  [ # | oogle | mail | et out | amestop ]
//          |
//        [ # | _maps | er ]
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_132.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/page_load_metrics/browser/metrics_web_contents_observer_unittest.cc#L1139-L1145

```c
< ASCII >
// -----------------------------------------------------------------------------
//     |                          |                          |
//     1s                         2s                         3s
//     Subframe1                  Main Frame                 Subframe2
//     LID (15ms)                 LID (100ms)                LID (200ms)
//
// Delivery order: Main Frame -> Subframe1 -> Subframe2.
< ASCII >
```
## Visual type:
- #interval


== ./chromium/chromium_133.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L542-L576

```c
< ASCII >
// The following tests verify both typical and edge case behavior of Prefetch()
// requests: they should prevent the FacetManager from being discarded, and keep
// the data fresh by initial fetches and refetches (scheduled as described in
// facet_manager.cc).
//
// Legend:
//   [---): Interval representing a finite Prefetch request (open from right).
//          The data should be kept fresh, the FacetManager not discarded.
//   [--->: Interval representing a indefinite Prefetch request.
//          The data should be kept fresh, the FacetManager not discarded.
//   F:     Fetch (initial or refetch) should take place here.
//   Fn:    The time of the n-th fetch (starting from 1).
//   D:     Time interval equal to GetShortTestPeriod().
//   N:     Fetch is signaled to be needed here.
//   X:     A corresponding CancelPrefetch call is placed here.
//   S:     |kCacheSoftExpiryInHours| hours
//   H:     |kCacheHardExpiryInHours| hours
//
// Note: It is guaranteed that S < H and that H < 2*S.
//
// Prefetches with the cache is initially stale/empty:
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [)                         :       :               :       :
//     [F--)                      :       :               :       :
//     [F------------------------):       :               :       :
//     [F--------------------------------):               :       :
//     [F-------------------------F----------)            :       :
//     [F-------------------------F----------------------):       :
//     [F-------------------------F------------------------------):
//     [F-------------------------F-----------------------F------------------>
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_134.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L635-L683

```c
// Prefetches with cached affiliation data that is fresh to some extent:
//
< ASCII >
// Suppose an unrelated fetch at t=0 has resulted in affiliation information
// being stored into the cache (freshness interval marked with '='). See legend
// above.
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o-----> t
//     [F================================):               :       :
//     :                          :       :               :       :
//     [)                         :       :               :       :
//     [--)                       :       :               :       :
//     [-------------------------):       :               :       :
//     [---------------------------------):               :       :
//     [--------------------------F-----------)           :       :
//     [--------------------------F----------------------):       :
//     [--------------------------F------------------------------):
//     [--------------------------F-----------------------F------------->
//                                :       :               :       :
//         [)                     :       :               :       :
//         [--)                   :       :               :       :
//         [---------------------):       :               :       :
//         [-----------------------------):               :       :
//         [----------------------F-----------)           :       :
//         [----------------------F----------------------):       :
//         [----------------------F------------------------------):
//         [----------------------F-----------------------F------------->
//                                :       :               :       :
//                                [)      :               :       :
//                                [----)  :               :       :
//                                [------):               :       :
//                                [F----------)           :       :
//                                [F---------------------):       :
//                                [F-----------------------------):
//                                [F----------------------F------------->
//
//      t=0                      S   S+D   H            F2+S         F2+H
//      /                        \   /     /               \         /
//  ---o--------------------------o-o-----o-----------------o-------o-----> t
//     [F================================):                 :       :
//                                :       :                 :       :
//                                : [)    :                 :       :
//                                : [----):                 :       :
//                                : [F------)               :       :
//                                : [F---------------------):       :
//                                : [F-----------------------------):
//                                : [F----------------------F------------->
< ASCII >
//
```
## Visual type:
- #custom 


== ./chromium/chromium_135.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L860-L882

```c
< ASCII >
// Nested prefetches. See legend above.
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [F=========================F==============================):
//     [F=========================F=======================F===========>
//     [--)                       :       :               :       :
//     :  [--)                    :       :               :       :
//     :  [----------------------):       :               :       :
//     :  [------------------------------):               :       :
//     :  [----------------------------------------------):       :
//     :  [------------------------------------------------------):
//     :                          [------):               :       :
//     :                          [----------------------):       :
//     :                          [------------------------------):
//     :                          : [----):               :       :
//     :                          : [--------------------):       :
//     :                          : [----------------------------):
//     :                          :                       [------):
//     :                          :                       : [----):
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_136.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L956-L967

```c
< ASCII >
// Overlapping prefetches. See legend above.
//
//      t=0                        S       H               F2+S    F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [F================================):               :       :
//     :     [--------------------F------------------------------):
//     :     [--------------------F-----------------------F----------->
//     :                          [F-----------------------------):
//     :                          [F----------------------F----------->
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_137.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L1019-L1039

```c
< ASCII >
// Prefetches with network fetches taking non-zero time. See legend above.
//
//      t=0                      S   S+D   H                   S+H     S+H+2*D
//      /                        \   /     /                     \     /
//  ---o--------------------------o-o-----o-----------------------o-o-o-----> t
//     :                          : :     :                       : : :
//     [NNF------------------------------):                       : : :
//     [F-------------------------NNF----------------------------): : :
//     [NNNNNNNNNNNNNNNNNNNNNNNNNNF------------------------------): : :
//     :                          : :     :                       : : :
//     [NNF----------------------------------)                    : : :
//     [F-------------------------NNF------------------------------): :
//     [NNNNNNNNNNNNNNNNNNNNNNNNNNNNF------------------------------): :
//     [NNF-------------------------NNF------------------------------):
//     :                          : :     :                       :
//     [NNN)                      : :     :                       :
//     [NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN):                       :
//     [F-------------------------NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN):
//     [NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN):
//     [NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN...
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_138.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L1151-L1166

```c
< ASCII >
// Canceling prefetches. See legend above.
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [F--X- - - - - - - - - - - - - - -):               :       :
//     [F-------------------------X - - -):               :       :
//     [F----------------------------X- -):               :       :
//     [F--X- - - - - - - - - - - - - - - - - - - - - - - - - - - - - ->
//     [F-------------------------X - - - - - - - - - - - - - - - - - ->
//     [F-------------------------F--X- - - - - - - - - - - - - - - - ->
//     [F-------------------------F----------X- - - - - - - - - - - - ->
//     [F-------------------------F-----------------------X - - - - - ->
//     [F-------------------------F-----------------------F--X- - - - ->
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_139.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L1220-L1229

```c
< ASCII >
// Canceling in case of multiple nested prefetches. See legend above.
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [F-------------------------F---------------------------X- ):
//        [---------------------------------X- - - - - - - - - - - -)
//           [--X- - - - - - - - - - - - - - - - - - - - - - - - - - -)
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_14.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/public/cpp/window_properties.h#L120-L128

```c
< ASCII >
// A property key to store the PIP snap fraction for this window.
// The fraction is defined in a clockwise fashion against the PIP movement area.
//
//            0   1
//          4 +---+ 1
//            |   |
//          3 +---+ 2
//            3   2
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_140.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L1263-L1273

```c
< ASCII >
// Canceling in case of duplicate prefetches with the same |until| value. See
// legend above.
//
//      t=0                        S       H               F2+S   F2+H
//      /                          /       /               /      /
//  ---o--------------------------o-------o---------------o-------o---------> t
//     :                          :       :               :       :
//     [F-------------------------F-----------------------F--X- - - - ->
//     [--------------------------X - - - - - - - - - - - - - - - - - ->
//     [--X - - - - - - - - - - - - - - - - - - - - - - - - - - - - - ->
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_141.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/password_manager/core/browser/affiliation/facet_manager_unittest.cc#L1483-L1492

```c
< ASCII >
// The cached data can be discarded (indicated by 'd') if and only if it is no
// longer needed to be kept fresh, or if it already stale.
//
//      t=0                        S       H                  F2+S   F2+H
//      /                          /       /                  /      /
//  ---o--------------------------o-------o------------------o-------o------> t
//     :                          :       :
//     [F-------------------------NNNNNNNNNNNF---)
//                                        ddd    ddd...
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_142.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/payments/content/android/payment_handler_host.h#L18-L43

```c
< ASCII >
// The native bridge for Java to interact with the payment handler host.
// Object relationship diagram:
//
// ChromePaymentRequestService.java --- implements --->
// PaymentRequestUpdateEventListener
//       |        ^
//      owns      |________________________
//       |                                |
//       v                                |
// PaymentHandlerHost.java                |
//       |                                |
//      owns                              |
//       |                             listener
//       v                                |
// android/payment_handler_host.h         |
//       |        |                       |
//      owns      |                       |
//       |       owns                     |
//       |        |                       |
//       |        v                       |
//       |    android/payment_request_update_event_listener.h
//       |        ^        \ ---- implements ---> PaymentHandlerHost::Delegate
//       |        |
//       |     delegate
//       v        |
// payment_handler_host.h
< ASCII >
```
## Visual type:
- #class-diagram


== ./chromium/chromium_143.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/payments/content/android/payment_request_spec.h#L17-L30

```c
< ASCII >
// A bridge for Android to own a C++ PaymentRequestSpec object.
//
// Object ownership diagram:
//
// ChromePaymentRequestService.java
//       |
//       v
// PaymentRequestSpec.java
//       |
//       v
// android/payment_request_spec.h
//       |
//       v
// payment_request_spec.h
< ASCII >
```
## Visual type:
- #class-diagram


== ./chromium/chromium_144.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/performance_manager/test_support/mock_graphs.h#L119-L132

```c
< ASCII >
// The following graph topology is created to emulate a scenario where a page
// contains a single frame that creates a single dedicated worker.
//
// Pg  Pr_
//  \ /   |
//   F    |
//    \   |
//     W__|
//
// Where:
// Pg: page
// F: frame(frame_tree_id:0)
// W: worker
// Pr: process(pid:1)
< ASCII >
```
## Visual type:
- #topology 


== ./chromium/chromium_145.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/performance_manager/test_support/mock_graphs.h#L143-L166

```c
< ASCII >
// The following graph topology is created to emulate a scenario where multiple
// pages making use of workers are hosted in multiple processes (e.g.
// out-of-process iFrames and multiple pages in a process):
//
//    Pg    OPg
//    |     |
//    F     OF
//   /\    /  \
//  W  \  /   CF
//   \ | /    | \
//     Pr     | OW
//            | /
//            OPr
//
// Where:
// Pg: page
// OPg: other_page
// F: frame(frame_tree_id:0)
// OF: other_frame(frame_tree_id:1)
// CF: child_frame(frame_tree_id:3)
// W: worker
// OW: other_worker
// Pr: process(pid:1)
// OPr: other_process(pid:2)
< ASCII >
```
## Visual type:
- #topology 


== ./chromium/chromium_146.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/prefs/segregated_pref_store.h#L24-L36

```c
// Provides a unified PersistentPrefStore implementation that splits its storage
// and retrieval between two underlying PersistentPrefStore instances: a set of
// preference names is used to partition the preferences.
//
< ASCII >
// Combines properties of the two stores as follows:
//   * The unified read error will be:
//                           Selected Store Error
//    Default Store Error | NO_ERROR      | NO_FILE       | other selected |
//               NO_ERROR | NO_ERROR      | NO_ERROR      | other selected |
//               NO_FILE  | NO_FILE       | NO_FILE       | NO_FILE        |
//          other default | other default | other default | other default  |
//   * The unified initialization success, initialization completion, and
//     read-only state are the boolean OR of the underlying stores' properties.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_147.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/safe_browsing/core/browser/password_protection/password_protection_request.h#L38-L56

```c
// A request for checking if an unfamiliar login form or a password reuse event
// is safe. PasswordProtectionRequest objects are owned by
// PasswordProtectionServiceBase indicated by |password_protection_service_|.
// PasswordProtectionServiceBase is RefCountedThreadSafe such that it can post
// task safely between IO and UI threads. It can only be destroyed on UI thread.
//
< ASCII >
// PasswordProtectionRequest flow:
// Step| Thread |                    Task
// (1) |   UI   | If incognito or !SBER, quit request.
// (2) |   UI   | Add task to IO thread for allowlist checking.
// (3) |   IO   | Check allowlist and return the result back to UI thread.
// (4) |   UI   | If allowlisted, check verdict cache; else quit request.
// (5) |   UI   | If verdict cached, quit request; else prepare request proto.
// (6) |   UI   | Collect features related to the DOM of the page.
// (7) |   UI   | If appropriate, compute visual features of the page.
// (7) |   UI   | Start a timeout task, and send network request.
// (8) |   UI   | On receiving response, handle response and finish.
//     |        | On request timeout, cancel request.
//     |        | On deletion of |password_protection_service_|, cancel request.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_148.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/services/storage/dom_storage/session_storage_metadata.cc#L19-L36

```c
< ASCII >
// Example layout of the database:
// | key                                    | value              |
// |----------------------------------------|--------------------|
// | map-1-a                                | b (a = b in map 1) |
// | ...                                    |                    |
// | namespace-<36 char guid 1>-StorageKey1 | 1 (mapid)          |
// | namespace-<36 char guid 1>-StorageKey2 | 2                  |
// | namespace-<36 char guid 2>-StorageKey1 | 1 (shallow copy)   |
// | namespace-<36 char guid 2>-StorageKey2 | 2 (shallow copy)   |
// | namespace-<36 char guid 3>-StorageKey1 | 3 (deep copy)      |
// | namespace-<36 char guid 3>-StorageKey2 | 2 (shallow copy)   |
// | next-map-id                            | 4                  |
// | version                                | 1                  |
// Example area key:
//   namespace-dabc53e1_8291_4de5_824f_dab8aa69c846-https://example.com/
//
// All number values (map numbers and the version) are string conversions of
// numbers. Map keys are converted to UTF-8 and the values stay as UTF-16.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_149.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/services/storage/dom_storage/testing_legacy_session_storage_database.cc#L55-L71

```c
< ASCII >
// Layout of the database:
// | key                            | value                              |
// -----------------------------------------------------------------------
// | map-1-                         | 2 (refcount, start of map-1-* keys)|
// | map-1-a                        | b (a = b in map 1)                 |
// | ...                            |                                    |
// | namespace-                     | dummy (start of namespace-* keys)  |
// | namespace-1- (1 = namespace id)| dummy (start of namespace-1-* keys)|
// | namespace-1-origin1            | 1 (mapid)                          |
// | namespace-1-origin2            | 2                                  |
// | namespace-2-                   | dummy                              |
// | namespace-2-origin1            | 1 (shallow copy)                   |
// | namespace-2-origin2            | 2 (shallow copy)                   |
// | namespace-3-                   | dummy                              |
// | namespace-3-origin1            | 3 (deep copy)                      |
// | namespace-3-origin2            | 2 (shallow copy)                   |
// | next-map-id                    | 4                                  |
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_15.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/shelf/shelf_application_menu_model.h#L22-L30

```c
< ASCII >
// A menu model listing open applications associated with a shelf item. Layout:
// +---------------------------+
// |                           |
// |         Shelf Item Title  |
// |                           |
// |  [Icon] Menu Item Label   |
// |  [Icon] Menu Item Label   |
// |                           |
// +---------------------------+
< ASCII >
```
## Visual type:



== ./chromium/chromium_150.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/subresource_filter/content/browser/ruleset_service.h#L5-L28

```c
< ASCII >
// This file contains the top-level class for the RulesetService.  There are
// associated classes that tie this into the dealer as well as the filter
// agents.  The distribution pipeline looks like this:
//
//                      RulesetService
//                           |
//                           v                  Browser
//                 RulesetPublisher(Impl)
//                     |              |
//        - - - - - - -|- - - - - - - |- - - - - - - - - -
//                     |       |      |
//                     v              v
//          *RulesetDealer     |  *RulesetDealer
//                 |                |       |
//                 |           |    |       v
//                 v                |      SubresourceFilterAgent
//    SubresourceFilterAgent   |    v
//                                SubresourceFilterAgent
//                             |
//
//         Renderer #1         |          Renderer #n
//
// Note: UnverifiedRulesetDealer is shortened to *RulesetDealer above. There is
// also a VerifiedRulesetDealer which is used similarly on the browser side.
< ASCII >
```
## Visual type:
- #flowchart


== ./chromium/chromium_151.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/subresource_filter/content/browser/ruleset_service.h#L62-L82

```c
// Contains all utility functions that govern how files pertaining to indexed
// ruleset version should be organized on disk.
//
< ASCII >
// The various indexed ruleset versions are kept in a two-level directory
// hierarchy based on their format and content version numbers, like so:
//
//   |base_dir|
//    |
//    +--10 (format_version)
//    |  |
//    |  +--1 (content_version)
//    |  |   \...
//    |  |
//    |  +--2 (content_version)
//    |      \...
//    |
//    +--11 (format_version)
//       |
//       +--2 (content_version)
//           \...
//
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_152.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/update_client/background_downloader_win.cc#L39-L103

```c
// The class BackgroundDownloader in this module is an adapter between
// the CrxDownloader interface and the BITS service interfaces.
// The interface exposed on the CrxDownloader code runs on the main sequence,
// while the BITS specific code runs in a separate sequence bound to a
// COM apartment. For every url to download, a BITS job is created, unless
// there is already an existing job for that url, in which case, the downloader
// connects to it. Once a job is associated with the url, the code looks for
// changes in the BITS job state. The checks are triggered by a timer.
// The BITS job contains just one file to download. There could only be one
// download in progress at a time. If Chrome closes down before the download is
// complete, the BITS job remains active and finishes in the background, without
// any intervention. The job can be completed next time the code runs, if the
// file is still needed, otherwise it will be cleaned up on a periodic basis.
//
// To list the BITS jobs for a user, use the |bitsadmin| tool. The command line
// to do that is: "bitsadmin /list /verbose". Another useful command is
// "bitsadmin /info" and provide the job id returned by the previous /list
// command.
//
< ASCII >
// Ignoring the suspend/resume issues since this code is not using them, the
// job state machine implemented by BITS is something like this:
//
//  Suspended--->Queued--->Connecting---->Transferring--->Transferred
//       |          ^         |                 |               |
//       |          |         V                 V               | (complete)
//       +----------|---------+-----------------+-----+         V
//                  |         |                 |     |    Acknowledged
//                  |         V                 V     |
//                  |  Transient Error------->Error   |
//                  |         |                 |     |(cancel)
//                  |         +-------+---------+--->-+
//                  |                 V               |
//                  |   (resume)      |               |
//                  +------<----------+               +---->Cancelled
//
< ASCII >
// The job is created in the "suspended" state. Once |Resume| is called,
// BITS queues up the job, then tries to connect, begins transferring the
// job bytes, and moves the job to the "transferred state, after the job files
// have been transferred. When calling |Complete| for a job, the job files are
// made available to the caller, and the job is moved to the "acknowledged"
// state.
// At any point, the job can be cancelled, in which case, the job is moved
// to the "cancelled state" and the job object is removed from the BITS queue.
// Along the way, the job can encounter recoverable and non-recoverable errors.
// BITS moves the job to "transient error" or "error", depending on which kind
// of error has occured.
// If  the job has reached the "transient error" state, BITS retries the
// job after a certain programmable delay. If the job can't be completed in a
// certain time interval, BITS stops retrying and errors the job out. This time
// interval is also programmable.
// If the job is in either of the error states, the job parameters can be
// adjusted to handle the error, after which the job can be resumed, and the
// whole cycle starts again.
// Jobs that are not touched in 90 days (or a value set by group policy) are
// automatically disposed off by BITS. This concludes the brief description of
// a job lifetime, according to BITS.
//
// In addition to how BITS is managing the life time of the job, there are a
// couple of special cases defined by the BackgroundDownloader.
// First, if the job encounters any of the 5xx HTTP responses, the job is
// not retried, in order to avoid DDOS-ing the servers.
// Second, there is a simple mechanism to detect stuck jobs, and allow the rest
// of the code to move on to trying other urls or trying other components.
// Last, after completing a job, irrespective of the outcome, the jobs older
// than a week are proactively cleaned up.
```
## Visual type:
- #state-machine


== ./chromium/chromium_153.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/update_client/component.cc#L45-L75

```c
< ASCII >
// The state machine representing how a CRX component changes during an update.
//
//     +------------------------- kNew
//     |                            |
//     |                            V
//     |                        kChecking
//     |                            |
//     V                error       V     no           no
//  kUpdateError <------------- [update?] -> [action?] -> kUpToDate  kUpdated
//     ^                            |           |            ^        ^
//     |                        yes |           | yes        |        |
//     |     update disabled        V           |            |        |
//     +-<--------------------- kCanUpdate      +--------> kRun       |
//     |                            |                                 |
//     |                no          V                                 |
//     |               +-<- [differential update?]                    |
//     |               |               |                              |
//     |               |           yes |                              |
//     |               | error         V                              |
//     |               +-<----- kDownloadingDiff            kRun---->-+
//     |               |               |                     ^        |
//     |               |               |                 yes |        |
//     |               | error         V                     |        |
//     |               +-<----- kUpdatingDiff ---------> [action?] ->-+
//     |               |                                     ^     no
//     |    error      V                                     |
//     +-<-------- kDownloading                              |
//     |               |                                     |
//     |               |                                     |
//     |    error      V                                     |
//     +-<-------- kUpdating --------------------------------+
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_154.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/update_client/component_unpacker.h#L33-L69

```c
// In charge of unpacking the component CRX package and verifying that it is
// well formed and the cryptographic signature is correct.
//
// This class should be used only by the component updater. It is inspired by
// and overlaps with code in the extension's SandboxedUnpacker.
// The main differences are:
// - The public key hash is full SHA256.
// - Does not use a sandboxed unpacker. A valid component is fully trusted.
// - The manifest can have different attributes and resources are not
//   transcoded.
< ASCII >
//
// If the CRX is a delta CRX, the flow is:
//   [ComponentUpdater]      [ComponentPatcher]
//   Unpack
//     \_ Verify
//     \_ Unzip
//     \_ BeginPatching ---> DifferentialUpdatePatch
//                             ...
//   EndPatching <------------ ...
//     \_ EndUnpacking
//
// For a full CRX, the flow is:
//   [ComponentUpdater]
//   Unpack
//     \_ Verify
//     \_ Unzip
//     \_ BeginPatching
//          |
//          V
//   EndPatching
//     \_ EndUnpacking
//
< ASCII >
// During unzip step we also check for verified_contents.json in the header
// of crx file and unpack it to metadata_ folder if it doesn't already contain
// verified_contents file.
// In both cases, if there is an error at any point, the remaining steps will
// be skipped and EndUnpacking will be called.
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_155.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/variations/proto/layer.proto#L16-L30

```c
// Each layer consists of N slots, and each Chrome client instance will randomly
// select a specific slot to be "active" within that layer based on the layer's
// salt and the client's entropy source value. That slot will be part of a range
// within a specific LayerMember. Each study that participates in a layer points
// to a particular LayerMember within that layer. Only the studies pointing to
// the active LayerMember will be active for a given client.
< ASCII >
// Example:
//    LayerFoo:
//        num_slots=10: |0, 1, 2, 3, 4, 5, 6, 7, 8, 9|
//        LayerMembers: <0   -   3>, <4     -       9>
//                        ^Member1^    ^Member2^.
//
//    In this example, the Member1 LayerMember has four slots and
//    Member2 has six out of ten total. This means studies pointing to
//    Member1 will receive ~40% of the population and Member2 ~60%.
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_156.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L37-L44

```c
< ASCII >
// One surface.
//
//  +e---------+
//  |          |
//  |          |
//  |          |
//  +----------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_157.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L83-L91

```c
< ASCII >
// One embedder with two children.
//
//  +e------------+     Point   maps to
//  | +c1-+ +c2---|     -----   -------
//  |1|   | |     |      1        e
//  | | 2 | | 3   | 4    2        c1
//  | +---+ |     |      3        c2
//  +-------------+      4        none
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_158.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L190-L200

```c
< ASCII >
// One embedder with a clipped child with a tab and transparent background.
//
//  +e-------------+
//  |   +c---------|     Point   maps to
//  | 1 |+a--+     |     -----   -------
//  |   || 2 |  3  |       1        e
//  |   |+b--------|       2        a
//  |   ||         |       3        e ( transparent area in c )
//  |   ||   4     |       4        b
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_159.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L266-L278

```c
// One embedder with a clipped child with a tab and transparent background, and
// a child d under that.
//
< ASCII >
//  +e-------------+
//  |      +d------|
//  |   +c-|-------|     Point   maps to
//  | 1 |+a|-+     |     -----   -------
//  |   || 2 |  3  |       1        e
//  |   |+b|-------|       2        a
//  |   || |       |       3        d
//  |   || | 4     |       4        b
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_16.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/style/style_viewer/system_ui_components_grid_view.cc#L29-L50

```c
//------------------------------------------------------------------------------
// SystemUIComponentsGridView::GridLayout:
// GridLayout splits the contents into `row_num` x `col_num` grids. Grids in the
// same row have same height and grids in the same column have same width. The
// rows and columns can be divided into equal sized groups. `row_group_size` and
// `col_group_size` indicate the number of rows and columns in each row and
// column group. If the number of rows and columns cannot be divided by their
// group size, the last group will have the remainders. There is a spacing
// between the row and column groups. There is also a border around the grids.
< ASCII >
// An example is shown below:
// +---------------------------------------------------------------------------+
// |                                border                                     |
// |        +--------+-------+                   +---------+----------+        |
// |        |        |       | col group spacing |         |          |        |
// |        +--------+-------+                   +---------+----------+        |
// |         row group spacing                     row group spacing           |
// |        +--------+-------+                   +---------+----------+        |
// | border |        |       |                   |         |          | border |
// |        |        |       | col group spacing |         |          |        |
// |        +--------+-------+                   +---------+----------+        |
// |                                border                                     |
// +---------------------------------------------------------------------------+
< ASCII >
```
## Visual type:



== ./chromium/chromium_160.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L415-L432

```c
< ASCII >
// One embedder with two clipped children with a tab and transparent background.
//
//  +e-------------+
//  |   +c1--------|     Point   maps to
//  | 1 |+a--+     |     -----   -------
//  |   || 2 |  3  |       1        e
//  |   |+b--------|       2        a
//  |   ||         |       3        e ( transparent area in c1 )
//  |   ||   4     |       4        b
//  |   +----------|       5        g
//  |   +c2--------|       6        e ( transparent area in c2 )
//  |   |+g--+     |       7        h
//  |   || 5 |  6  |
//  |   |+h--------|
//  |   ||         |
//  |   ||   7     |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_161.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L634-L648

```c
< ASCII >
// Children that are multiple layers deep.
//
//  +e--------------------+
//  |          +c2--------|     Point   maps to
//  | +c1------|----+     |     -----   -------
//  |1| +a-----|---+|     |       1        e
//  | | |+b----|--+||     |       2        g
//  | | ||+g--+|  |||     |       3        b
//  | | ||| 2 || 3|||  4  |       4        c2
//  | | ||+---+|  |||     |
//  | | |+-----|--+||     |
//  | | +------| --+|     |
//  | +--------|----+     |
//  +---------------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_162.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L728-L742

```c
< ASCII >
// Multiple layers deep of transparent children.
//
//  +e--------------------+
//  |          +c2--------|     Point   maps to
//  | +c1------|----+     |     -----   -------
//  |1| +a-----|---+|     |       1        e
//  | | |+b----|--+||     |       2        e
//  | | ||+g--+|  |||     |       3        c2
//  | | ||| 2 || 3|||  4  |       4        c2
//  | | ||+---+|  |||     |
//  | | |+-----|--+||     |
//  | | +------| --+|     |
//  | +--------|----+     |
//  +---------------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_163.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L1002-L1010

```c
< ASCII >
// One embedder with two children.
//
//  +e------------+     Point   maps to
//  | +c1-+ +c2---|     -----   -------
//  |1|   | |     |      1        e
//  | | 2 | | 3   |      2        c1
//  | +---+ |     |      3        c2
//  +-------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_164.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L1065-L1076

```c
< ASCII >
// One embedder with nested OOPIFs. When the mid-level iframe has kHitTestAsk
// flag we should do async hit test and skip checking its descendants.
//
//  +e-------------+
//  |   +c---------|     Point   maps to
//  | 1 |    2     |     -----   -------
//  |   |          |       1        e
//  |   |+b--------|       2        c
//  |   ||         |       3        c
//  |   ||   3     |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_165.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L1209-L1221

```c
< ASCII >
// Region c1 is transparent and on top of e, c2 is a child of e, d1 is a
// child of c1. Any events outside of d1 should go to either c2 or e instead
// of being taken by the transparent c1 region.
//
//  +e/c1----------+
//  |              |     Point   maps to
//  |   +c2-+      |     -----   -------
//  |   | 1 |      |       1        c2
//  |   +d1--------|       2        d1
//  |   | 2        |
//  |   |          |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_166.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/host/hit_test/hit_test_query_unittest.cc#L1320-L1331

```c
< ASCII >
// One embedder with nested OOPIFs. When the root view is overlapped we should
// continue checking its descendants rather than doing async hit test.
//
//  +e-------------+
//  |   +c---------|     Point   maps to
//  | 1 |    2     |     -----   -------
//  |   |          |       1        e
//  |   |          |       2        c
//  |   |          |
//  |   |          |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_167.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/display/display_unittest.cc#L1975-L1979

```c
< ASCII >
// Check if draw occlusion works well with rotation transform.
//
//  +-----+                                  +----+
//  |     |   rotation (by 45 on y-axis) ->  |    |     same height
//  +-----+                                  +----+     reduced weight
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_168.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/frame_sinks/video_capture/video_capture_overlay_unittest.cc#L600-L611

```c
// Tests that the overlay will be partially rendered (clipped) when any part of
// it extends outside the video frame's content region.
//
< ASCII >
// For this test, the content region is a rectangle, centered within the frame
// (e.g., the content is being letterboxed), and the test attempts to locate the
// overlay such that part of it should be clipped. The test succeeds if the
// overlay is clipped to the content region in the center. For example:
//
//    +-------------------------------+
//    |                               |
//    |     ......                    |
//    |     ..****////////////        |  **** the drawn part of the overlay
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_169.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/frame_sinks/video_capture/video_capture_overlay_unittest.cc#L613-L618

```c
< ASCII >
//    |     ..****CONTENT/////        |
//    |       /////REGION/////        |  .... the clipped part of the overlay
//    |       ////////////////        |       (i.e., not drawn)
//    |                               |
//    |                               |
//    +-------------------------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_17.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/style/style_viewer/system_ui_components_style_viewer_view.h#L22-L35

```c
< ASCII >
// SystemUIComponentsStyleViewerView is the client view of the system components
// style viewer. It has two parts: (1) the menu scroll view contains a list of
// component buttons which can toggle the component instances, (2) the component
// instances scroll view lists the instances of current selected component. The
// layout is shown below:
// +----------------------+---------------------------------------+
// |  Menu Scroll View    |    Component Instances Scroll View    |
// | +------------------+ |                                       |
// | | Component Button | |                                       |
// | +------------------+ |                                       |
// | | Component Button | |                                       |
// | +------------------+ |                                       |
// |                      |                                       |
// +----------------------+---------------------------------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_170.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L245-L252

```c
< ASCII >
// One surface.
//
//  +----------+
//  |          |
//  |          |
//  |          |
//  +----------+
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_171.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L281-L289

```c
< ASCII >
// One opaque embedder with two regions.
//
//  +e-------------+
//  | +r1-+ +r2--+ |
//  | |   | |    | |
//  | |   | |    | |
//  | +---+ +----+ |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_172.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L341-L349

```c
< ASCII >
// One embedder with two children.
//
//  +e-------------+
//  | +c1-+ +c2--+ |
//  | |   | |    | |
//  | |   | |    | |
//  | +---+ +----+ |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_173.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L426-L435

```c
< ASCII >
// Occluded child frame (OOPIF).
//
//  +e-----------+
//  | +c--+      |
//  | | +div-+   |
//  | | |    |   |
//  | | +----+   |
//  | +---+      |
//  +------------+
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_174.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L505-L515

```c
< ASCII >
// Foreground child frame (OOPIF).
// Same as the previous test except the child is foreground.
//
//  +e-----------+
//  | +c--+      |
//  | |   |div-+ |
//  | |   |    | |
//  | |   |----+ |
//  | +---+      |
//  +------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_175.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L586-L596

```c
< ASCII >
// One embedder with a clipped child with a tab and transparent background.
//
//  +e-------------+
//  |   +c---------|     Point   maps to
//  | 1 |+a--+     |     -----   -------
//  |   || 2 |  3  |       1        e
//  |   |+b--------|       2        a
//  |   ||         |       3        e (transparent area in c)
//  |   ||   4     |       4        b
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_176.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L712-L723

```c
< ASCII >
// Three children deep.
//
//  +e------------+
//  | +c1-------+ |
//  | | +c2---+ | |
//  | | | +c3-| | |
//  | | | |   | | |
//  | | | +---| | |
//  | | +-----+ | |
//  | +---------+ |
//  +-------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_177.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L836-L845

```c
< ASCII >
// Missing / late child.
//
//  +e-----------+
//  | +c--+      |
//  | |   |div-+ |
//  | |   |    | |
//  | |   |----+ |
//  | +---+      |
//  +------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_178.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/components/viz/service/hit_test/hit_test_aggregator_unittest.cc#L1025-L1036

```c
< ASCII >
// Region c1 is transparent and on top of e, c2 is a child of e, d1 is a
// child of c1.
//
//  +e/c1----------+
//  |              |     Point   maps to
//  |   +c2-+      |     -----   -------
//  |   | 1 |      |       1        c2
//  |   +d1--------|
//  |   |          |
//  |   |          |
//  +--------------+
//
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_179.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/back_forward_cache_internal_browsertest.cc#L835-L858

```c
< ASCII >
// Test the race condition where a document is evicted from the BackForwardCache
// while it is in the middle of being restored and before URL loader starts a
// response.
//
// ┌───────┐                 ┌────────┐
// │Browser│                 │Renderer│
// └───┬───┘                 └───┬────┘
// (Freeze & store the cache)    │
//     │────────────────────────>│
//     │                         │
// (Navigate to cached document) │
//     │──┐                      │
//     │  │                      │
//     │EvictFromBackForwardCache│
//     │<────────────────────────│
//     │  │                      │
//     │  x Navigation cancelled │
//     │    and reissued         │
// ┌───┴───┐                 ┌───┴────┐
// │Browser│                 │Renderer│
// └───────┘                 └────────┘
//
// When the eviction occurs, the in flight NavigationRequest to the cached
// document should be reissued (cancelled and replaced by a normal navigation).
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_18.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/system/input_device_settings/keyboard_modifier_metrics_recorder.cc#L48-L58

```c
< ASCII >
// | Index in kKeyboardModifierPrefs | Bit Range |
// | 0                               | [0, 2]    |
// | 1                               | [3, 5]    |
// | 2                               | [6, 8]    |
// | 3                               | [9, 11]   |
// | 4                               | [12, 14]  |
// | 5                               | [15, 17]  |
// | 6                               | [18, 20]  |
// | 7                               | [21, 23]  |
// | 8                               | [24, 26]  |
// | 9                               | [27, 29]  |
< ASCII >
```
## Visual type:



== ./chromium/chromium_180.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/back_forward_cache_internal_browsertest.cc#L982-L1001

```c
// The test is simulating a race condition. The scheduler tracked features are
// updated during the "freeze" event in a way that would have prevented the
// document from entering the BackForwardCache in the first place.
//
// TODO(https://crbug.com/996267): The document should be evicted.
< ASCII >
//
// ┌───────┐                     ┌────────┐
// │browser│                     │renderer│
// └───┬───┘                     └────┬───┘
//  (enter cache)                     │
//     │           Freeze()           │
//     │─────────────────────────────>│
//     │                          (onfreeze)
//     │OnSchedulerTrackedFeaturesUsed│
//     │<─────────────────────────────│
//     │                           (frozen)
//     │                              │
// ┌───┴───┐                     ┌────┴───┐
// │browser│                     │renderer│
// └───────┘                     └────────┘
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_181.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L1239-L1256

```c
// This test checks that killing a renderer process of a remote frame
// and then navigating some other frame to the same SiteInstance of the killed
// process works properly.
< ASCII >
// This can be illustrated as follows,
// where 1/2/3 are FrameTreeNode-s and A/B are processes and B* is the killed
// B process:
//
//     1        A                  A                           A
//    / \  ->  / \  -> Kill B ->  / \  -> Navigate 3 to B ->  / \  .
//   2   3    B   A              B*  A                       B*  B
//
// Initially, node1.proxy_hosts_ = {B}
// After we kill B, we make sure B stays in node1.proxy_hosts_, then we navigate
// 3 to B and we expect that to complete normally.
< ASCII >
// See http://crbug.com/432107.
//
// Note that due to http://crbug.com/450681, node2 cannot be re-navigated to
// site B and stays in not rendered state.
```
## Visual type:
- #tree


== ./chromium/chromium_182.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L1323-L1338

```c
< ASCII >
// This test is similar to
// SitePerProcessBrowserTest.NavigateRemoteFrameToKilledProcess with
// addition that node2 also has a cross-origin frame to site C.
//
//     1          A                  A                       A
//    / \        / \                / \                     / \  .
//   2   3 ->   B   A -> Kill B -> B*   A -> Navigate 3 -> B*  B
//  /          /
// 4          C
//
// Initially, node1.proxy_hosts_ = {B, C}
// After we kill B, we make sure B stays in node1.proxy_hosts_, but
// C gets cleared from node1.proxy_hosts_.
< ASCII >
//
// Note that due to http://crbug.com/450681, node2 cannot be re-navigated to
// site B and stays in not rendered state.
```
## Visual type:
- #tree


== ./chromium/chromium_183.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L1575-L1584

```c
< ASCII >
// Verify that killing a cross-site frame's process B and then navigating a
// frame to B correctly recreates all proxies in B.
//
//      1           A                    A          A
//    / | \       / | \                / | \      / | \  .
//   2  3  4 ->  B  A  A -> Kill B -> B* A  A -> B* B  A
//
// After the last step, the test sends a postMessage from node 3 to node 4,
// verifying that a proxy for node 4 has been recreated in process B.  This
// verifies the fix for https://crbug.com/478892.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_184.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L1636-L1647

```c
< ASCII >
// Verify that proxy creation doesn't recreate a crashed process if no frame
// will be created in it.
//
//      1           A                    A          A
//    / | \       / | \                / | \      / | \    .
//   2  3  4 ->  B  A  A -> Kill B -> B* A  A -> B* A  A
//                                                      \  .
//                                                       A
//
// The test kills process B (node 2), creates a child frame of node 4 in
// process A, and then checks that process B isn't resurrected to create a
// proxy for the new child frame.  See https://crbug.com/476846.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_185.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L1803-L1814

```c
< ASCII >
// In A-embed-B-embed-C scenario, verify that killing process B clears proxies
// of C from the tree.
//
//     1          A                  A
//    / \        / \                / \    .
//   2   3 ->   B   A -> Kill B -> B*  A
//  /          /
// 4          C
//
// node1 is the root.
// Initially, both node1.proxy_hosts_ and node3.proxy_hosts_ contain C.
// After we kill B, make sure proxies for C are cleared.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_186.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L2706-L2718

```c
< ASCII >
// Verify that when a new child frame is added, the proxies created for it in
// other SiteInstances have correct sandbox flags and origin.
//
//     A         A           A
//    /         / \         / \    .
//   B    ->   B   A   ->  B   A
//                              \  .
//                               B
//
// The test checks sandbox flags and origin for the proxy added in step 2, by
// checking whether the grandchild frame added in step 3 sees proper sandbox
// flags and origin for its (remote) parent.  This wasn't addressed when
// https://crbug.com/423587 was fixed.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_187.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L3610-L3621

```c
< ASCII >
// Check that frame opener updates work with subframes.  Set up a window with a
// popup and update openers for the popup's main frame and subframe to
// subframes on first window, as follows:
//
//    foo      +---- bar
//    / \      |     / \      .
// bar   foo <-+  bar   foo
//  ^                    |
//  +--------------------+
//
< ASCII >
// The sites are carefully set up so that both opener updates are cross-process
// but still allowed by Blink's navigation checks.
```
## Visual type:
- #topology


== ./chromium/chromium_188.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_browsertest.cc#L10039-L10049

```c
< ASCII >
// Verify that when the sandbox iframe attribute changes on a page which also
// has CSP-set sandbox flags, that the correct combination of flags is set in
// the sandboxed page after navigation.
//
//   A        A         A                                  A
//    \        \         \                                  \     .
//     B  ->    B*   ->   B*   -> (change sandbox attr) ->   B*
//             /  \      /  \                               /  \  .
//            B    B    A    B                             A'   B
//
// (B* has CSP-set sandbox flags)
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_189.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_unload_browsertest.cc#L395-L406

```c
< ASCII >
// Test that unload handlers in iframes are run, even when the removed subtree
// is complicated with nested iframes in different processes.
//     A1                         A1
//    / \                        / \
//   B1  D  --- Navigate --->   E   D
//  / \
// C1  C2
// |   |
// B2  A2
//     |
//     C3
< ASCII >
// TODO(crbug.com/1012185): Flaky timeouts on Linux and Mac.
```
## Visual type:
- #tree


== ./chromium/chromium_19.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/system/night_light/night_light_controller_unittest.cc#L1745-L1754

```c
< ASCII >
// Now is at 11 PM.
//
//      20:00               23:00                      5:00
// <----- + ----------------- + ----------------------- + ------->
//        |                   |                         |
//      sunset               now                     sunrise
//
// Tests that when the user logs in for the first time between sunset and
// sunrise with Auto Night Light enabled, and the notification has never been
// dismissed before, the notification should be shown.
< ASCII >
```
## Visual type:
- #interval


== ./chromium/chromium_190.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_unload_browsertest.cc#L570-L579

```c
< ASCII >
// Start with A(B(C)), navigate C to D and then B to E. By emulating a slow
// unload handler in B,C and D, the end result is C is in pending deletion in B
// and B is in pending deletion in A.
//   (1)     (2)     (3)
//|       |       |       |
//|   A   |  A    |   A   |
//|   |   |  |    |    \  |
//|   B   |  B    |  B  E |
//|   |   |   \   |   \   |
//|   C   | C  D  | C  D  |
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_191.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/site_per_process_unload_browsertest.cc#L749-L770

```c
// Test RenderFrameHostImpl::PendingDeletionCheckCompletedOnSubtree.
//
// After a navigation commit, some children with no unload handler may be
// eligible for immediate deletion. Several configurations are tested:
//
< ASCII >
// Before navigation commit
//
//              0               |  N  : No unload handler
//   ‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑‑      | [N] : Unload handler
//  |  |  |  |  |   |     |     |
// [1] 2 [3] 5  7   9     12    |
//        |  |  |  / \   / \    |
//        4 [6] 8 10 11 13 [14] |
//
// After navigation commit (expected)
//
//              0               |  N  : No unload handler
//   ---------------------      | [N] : Unload handler
//  |     |  |            |     |
// [1]   [3] 5            12    |
//           |             \    |
//          [6]            [14] |
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_192.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/android/synchronous_compositor_sync_call_bridge.h#L23-L71

```c
// For the synchronous compositor feature of webview it is necessary
// that the UI thread to block until the renderer process has processed
// certain messages entirely. (beginframe and resulting compositor frames).
// This object is used to manage the waiting and signaling behavior on the UI
// thread. The UI thread will wait on a WaitableEvent (via FrameFuture class)
// or condition variable which is then signal by handlers in this class.
// This object is a cross thread object accessed both on the UI and IO threads.
//
< ASCII >
// Examples of call graphs are:
//    Browser UI Thread         Browser IO Thread       Renderer
//
//  ->VSync Java
//      ----------------------------------------------->BeginFrame
//      CV Wait
//                                BeginFrameRes<----------
//                                CVSignal
//      WakeUp
//
//
//  ->DrawHwAsync
//      RegisterFrameFuture
//      ----------------------------------------------->DrawHwAsync
//      Do some stuff
//      FrameFuture::GetFrame()
//        WaitableEvent::Wait()
//                             ReceiveFrame<---------------
//                             WaitableEvent::Signal()
//      WakeUp
//
// This may seem simple but it gets a little more complicated when
// multiple views are involved. Each view will have it's own SyncCallBridge.
//
//   Once example is:
//
//    Browser UI Thread         Browser IO Thread       Renderer1    Renderer2
//
//  ->VSync Java
//      ----------------------------------------------->BeginFrame
//                                BeginFrameRes<----------
//                                CVSignal
//      ------------------------------------------------------------>BeginFrame
//      CV Wait
//                                BeginFrameRes<----------------------------
//                                CVSignal
//      WakeUp
//
// Notice that it is possible that before we wait on a CV variable a renderer
// may have already responded to the BeginFrame request.
< ASCII >
//
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_193.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/download/save_file_manager.h#L1-L50

```c
// Copyright 2012 The Chromium Authors
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.
//
// Objects that handle file operations for saving files, on the file thread.
//
// The SaveFileManager owns a set of SaveFile objects, each of which connects
// with a SaveItem object which belongs to one SavePackage and runs on the file
// thread for saving data in order to avoid disk activity on either network IO
// thread or the UI thread. It coordinates the notifications from the network
// and UI.
//
// The SaveFileManager itself is a singleton object created and owned by the
// BrowserMainLoop.
//
// The data sent to SaveFileManager have 2 sources:
// - SimpleURLLoaders which are used to download sub-resources and
//   save-only-HTML pages
// - render processese, for HTML pages which are serialized from the DOM in
//   their original encoding. The data is received on the UI thread and
//   dispatched directly to the SaveFileManager on the file thread.
< ASCII >
//
// A typical saving job operation involves multiple threads and sequences:
//
// Updating an in progress save file:
//      |----> data from    ---->|  |
//      |      render process    |  |
//      |      SimpleURLLoaders  |  |
// ui_thread                     |  |
//                   download_task_runner (writes to disk)
//                               |----> stats ---->|
//                                              ui_thread (feedback for user)
//
//
// Cancel operations perform the inverse order when triggered by a user action:
// ui_thread (user click)
//    |----> cancel command ---->|
//    |           |      download_task_runner (close file)
//    |           |---------------------> cancel command ---->|
//    |                                               io_thread (stops net IO
// ui_thread (user close contents)                               for saving)
//    |----> cancel command ---->|
//                            Render process(stop serializing DOM and sending
//                                           data)
//
< ASCII >
//
// The SaveFileManager tracks saving requests, mapping from a save item id to
// the SavePackage for the contents where the saving job was initiated. In the
// event of a contents closure during saving, the SavePackage will notify the
// SaveFileManage to cancel all SaveFile jobs.
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_194.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/renderer_host/display_feature.h#L17-L37

```c
// Information about a physical attribute of the screen which that creates a
// Logical separator or divider (e.g. a fold or mask).
< ASCII >
// This is a visual example of a vertically oriented display feature that masks
// content underneath
//
//    Orientation: vertical
//
//                 offset
//                   |
//         +---------|===|---------+
//         |         |   |         |
//         |         |   |         |
//         |         |   |         |
//         |         |   |         |
//         |         |   |         |
//         +---------|===|---------+
//                      \
//                      mask_length
//
// Note that the implicit height of the display feature is the entire height of
// the screen on which it exists.
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_195.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/renderer_host/render_frame_host_impl_browsertest.cc#L6199-L6211

```c
< ASCII >
// This test verifies that RFHImpl::ForEachImmediateLocalRoot works as expected.
// The frame tree used in the test is:
//                                A0
//                            /    |    \
//                          A1     B1    A2
//                         /  \    |    /  \
//                        B2   A3  B3  A4   C2
//                       /    /   / \    \
//                      D1   D2  C3  C4  C5
//
// As an example, the expected set of immediate local roots for the root node A0
// should be {B1, B2, C2, D2, C5}. Note that the order is compatible with that
// of a BFS traversal from root node A0.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_196.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/renderer_host/render_frame_host_manager_unittest.cc#L2449-L2462

```c
< ASCII >
// Build the following frame opener graph and see that it can be properly
// traversed when creating opener proxies:
//
//     +-> root4 <--+   root3 <---- root2    +--- root1
//     |     /      |     ^         /  \     |    /  \     .
//     |    42      +-----|------- 22  23 <--+   12  13
//     |     +------------+            |             | ^
//     +-------------------------------+             +-+
//
// The test starts traversing openers from root1 and expects to discover all
// four FrameTrees.  Nodes 13 (with cycle to itself) and 42 (with back link to
// root3) should be put on the list of nodes that will need their frame openers
// set separately in a second pass, since their opener routing IDs won't be
// available during the first pass of CreateOpenerProxies.
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_197.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/renderer_host/media/render_frame_audio_output_stream_factory.h#L24-L36

```c
< ASCII >
// This class is related to ForwardingAudioStreamFactory as follows:
//
//     WebContentsImpl       <--        RenderFrameHostImpl
//           ^                                  ^
//           |                                  |
//  ForwardingAudioStreamFactory   RenderFrameAudioOutputStreamFactory
//           ^                                  ^
//           |                                  |
//      FASF::Core           <--          RFAOSF::Core
//
// Both FASF::Core and RFAOSF::Core live on (and are destructed on) the IO
// thread. A weak pointer to ForwardingAudioStreamFactory is used since
// WebContentsImpl is sometimes destructed shortly before RenderFrameHostImpl.
< ASCII >
```
## Visual type:
- #class-diagram


== ./chromium/chromium_198.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/browser/webid/idp_network_request_manager.h#L36-L63

```c
< ASCII >
// Manages network requests and maintains relevant state for interaction with
// the Identity Provider across a FedCM transaction. Owned by
// FederatedAuthRequestImpl and has a lifetime limited to a single identity
// transaction between an RP and an IDP.
//
// Diagram of the permission-based data flows between the browser and the IDP:
//  .-------.                           .---.
//  |Browser|                           |IDP|
//  '-------'                           '---'
//      |                                 |
//      |     GET /fedcm.json             |
//      |-------------------------------->|
//      |                                 |
//      |        JSON{idp_url}            |
//      |<--------------------------------|
//      |                                 |
//      | POST /idp_url with OIDC request |
//      |-------------------------------->|
//      |                                 |
//      |       token or signin_url       |
//      |<--------------------------------|
//  .-------.                           .---.
//  |Browser|                           |IDP|
//  '-------'                           '---'
//
// If the IDP returns an token, the sequence finishes. If it returns a
// signin_url, that URL is loaded as a rendered Document into a new window for
// the user to interact with the IDP.
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_199.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/content/test/content_browser_test_utils_internal.h#L109-L128

```c
// Creates compact textual representations of the state of the frame tree that
// is appropriate for use in assertions.
//
< ASCII >
// The diagrams show frame tree structure, the SiteInstance of current frames,
// presence of pending frames, and the SiteInstances of any and all proxies.
// They look like this:
//
//        Site A (D pending) -- proxies for B C
//          |--Site B --------- proxies for A C
//          +--Site C --------- proxies for B A
//               |--Site A ---- proxies for B
//               +--Site A ---- proxies for B
//                    +--Site A -- proxies for B
//       Where A = http://127.0.0.1/
//             B = http://foo.com/ (no process)
//             C = http://bar.com/
//             D = http://next.com/
//
// SiteInstances are assigned single-letter names (A, B, C) which are remembered
// across invocations of the pretty-printer.
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_2.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/android_webview/tools/system_webview_shell/apk/src/org/chromium/webview_shell/StartupTimeActivity.java#L22-L48

```c
/**
 * An activity to measure the startup time of WebView in various scenarios.
 *
 * Example run:
 *   # Compile dex file for optimized run.
 *   adb shell cmd package compile -m speed -f <webview_package_name>
 *   adb shell killall org.chromium.webview_shell
 *   # Run scenario: CREATE (1). There are other scenarios you can try.
 *   adb shell am start -n org.chromium.webview_shell/.StartupTimeActivity \
 *     -a android.intent.action.VIEW --ei "target" 1
 *   adb logcat | grep WebViewShell
 *
< ASCII >
 * Then you will see a tuple of durations in ms that the scenario blocked UI thread for each loop in
 * the logcat. Here are empirical sample results for WebView 80 on an internal Go device:
 * +-------------+-------+-----------------+
 * | target name | index | results (ms)    |
 * +-------------+-------+-----------------+
 * | DO_NOTHING  | 0     | (empty)         |
 * | CREATE      | 1     | (620, 50)       |
 * | ADD_VIEW    | 2     | (620, 150)      |
 * | LOAD        | 3     | (620, 150, 140) |
 * | WORKAROUND  | 4     | (220, 155)      |
 * +-------------+-------+-----------------+
 *
 * Note that you need to run this multiple times and ignore the first few runs
 * to get an accurate measurement.
< ASCII >
 */
```
## Visual type:
- #table


== ./chromium/chromium_20.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/system/time/calendar_up_next_view_unittest.cc#L319-L326

```c
< ASCII >
// If we have a partially visible event view and the scroll left button is
// pressed, we should scroll to put the whole event into view, aligned to the
// start of the viewport.
//          [---------------] <-- ScrollView viewport
// [-E1-] [---E2---]          <-- Event 2 partially shown in the viewport.
// Press scroll left button.
//          [---------------] <-- ScrollView viewport
//   [-E1-] [---E2---]        <-- Event 2 now fully shown in viewport.
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_200.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/device/fido/mac/credential_metadata.cc#L261-L268

```c
< ASCII >
// UnsealLegacyCredentialId attempts to decrypt a credential ID that has been
// encrypted under the scheme for version 0 or 1, which is:
//    | version  |    nonce   | AEAD(pt=CBOR(metadata),    |
//    | (1 byte) | (12 bytes) |      nonce=nonce,          |
//    |          |            |      ad=(version, rpID))   |
// In these versions, the `version` field is not part of the AEAD pt. Version 0
// also lacks the `is_resident` boolean inside the metadata (i.e. all V0
// credentials are non-resident).
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_201.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/device/vr/windows/geometry_shader.h#L10-L31

```c
< ASCII >
// Generated by Microsoft (R) HLSL Shader Compiler 6.3.9600.16384
//
//
//
// Input signature:
//
// Name                 Index   Mask Register SysValue  Format   Used
// -------------------- ----- ------ -------- -------- ------- ------
// SV_POSITION              0   xyzw        0      POS   float   xyzw
// TEXCOORD                 0   xy          1     NONE   float   xy
// TEXCOORD                 1   x           2     NONE    uint   x
//
//
// Output signature:
//
// Name                 Index   Mask Register SysValue  Format   Used
// -------------------- ----- ------ -------- -------- ------- ------
// SV_POSITION              0   xyzw        0      POS   float   xyzw
// TEXCOORD                 0   xy          1     NONE   float   xy
// SV_RenderTargetArrayIndex     0   x           2  RTINDEX    uint   x
//
< ASCII >
```
## Visual type:
- #table 


== ./chromium/chromium_202.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/device/vr/windows/vertex_shader.h#L10-L41

```c
//
// Generated by Microsoft (R) HLSL Shader Compiler 6.3.9600.16384
//
//
//
< ASCII >
// Input signature:
//
// Name                 Index   Mask Register SysValue  Format   Used
// -------------------- ----- ------ -------- -------- ------- ------
// POSITION                 0   xy          0     NONE   float   xy
// TEXCOORD                 0   xy          1     NONE   float   xy
// TEXCOORD                 1   x           2     NONE    uint   x
//
//
// Output signature:
//
// Name                 Index   Mask Register SysValue  Format   Used
// -------------------- ----- ------ -------- -------- ------- ------
// SV_POSITION              0   xyzw        0      POS   float   xyzw
// TEXCOORD                 0   xy          1     NONE   float   xy
// TEXCOORD                 1   x           2     NONE    uint   x
//
//
// Runtime generated constant mappings:
//
// Target Reg                               Constant Description
// ---------- --------------------------------------------------
// c0                              Vertex Shader position offset
//
//
// Level9 shader bytecode:
//
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_203.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/docs/ui/learn/bestpractices/layout.md#L508-L513

```c
< ASCII >
// Used to create the following layout
// +-----------+---------------------+----------------+
// | icon_view | title               | secondary_view |
// +-----------+---------------------+----------------+
// |           | subtitle            |                |
// +-----------+---------------------+----------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_204.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/docs/ui/learn/bestpractices/layout.md#L586-L591

```c
< ASCII >
// Used to create the following layout
// +-----------+---------------------+----------------+
// |           | title               |                |
// | icon_view |---------------------| secondary_view |
// |           | subtitle            |                |
// +-----------+---------------------+----------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_205.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/extensions/browser/content_hash_tree.h#L13-L33

```c
// This takes a list of sha256 hashes, considers them to be leaf nodes of a
// hash tree (aka Merkle tree), and computes the root node of the tree using
// the given branching factor to hash lower level nodes together. Tree hash
// implementations differ in how they handle the case where the number of
// leaves isn't an integral power of the branch factor. This implementation
// just hashes together however many are left at a given level, even if that is
// less than the branching factor (instead of, for instance, directly promoting
// elements). E.g., imagine we use a branch factor of 3 for a vector of 4 leaf
// nodes [A,B,C,D]. This implemention will compute the root hash G as follows:
< ASCII >
//
// |      G      |
// |     / \     |
// |    E   F    |
// |   /|\   \   |
// |  A B C   D  |
//
// where E = Hash(A||B||C), F = Hash(D), and G = Hash(E||F)
< ASCII >
//
// The one exception to this rule is when there is only one node left. This
// means that the root hash of any vector with just one leaf is the same as
// that leaf. Ie RootHash([A]) == A, not Hash(A).
```
## Visual type:
- #tree


== ./chromium/chromium_206.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/extensions/browser/extension_pref_value_map.h#L24-L60

```c
// Non-persistent data container that is shared by ExtensionPrefStores. All
// extension pref values (incognito and regular) are stored herein and
// provided to ExtensionPrefStores.
//
// The semantics of the ExtensionPrefValueMap are:
// - A regular setting applies to regular browsing sessions as well as incognito
//   browsing sessions.
// - An incognito setting applies only to incognito browsing sessions, not to
//   regular ones. It takes precedence over a regular setting set by the same
//   extension.
// - A regular-only setting applies only to regular browsing sessions, not to
//   incognito ones. It takes precedence over a regular setting set by the same
//   extension.
// - If two different extensions set a value for the same preference (and both
//   values apply to the regular/incognito browsing session), the extension that
//   was installed later takes precedence, regardless of whether the settings
//   are regular, incognito or regular-only.
//
// 
< ASCII >The following table illustrates the behavior:
//   A.reg | A.reg_only | A.inc | B.reg | B.reg_only | B.inc | E.reg | E.inc
//     1   |      -     |   -   |   -   |      -     |   -   |   1   |   1
//     1   |      2     |   -   |   -   |      -     |   -   |   2   |   1
//     1   |      -     |   3   |   -   |      -     |   -   |   1   |   3
//     1   |      2     |   3   |   -   |      -     |   -   |   2   |   3
//     1   |      -     |   -   |   4   |      -     |   -   |   4   |   4
//     1   |      2     |   3   |   4   |      -     |   -   |   4   |   4
//     1   |      -     |   -   |   -   |      5     |   -   |   5   |   1
//     1   |      -     |   3   |   4   |      5     |   -   |   5   |   4
//     1   |      -     |   -   |   -   |      -     |   6   |   1   |   6
//     1   |      2     |   -   |   4   |      -     |   6   |   4   |   6
//     1   |      2     |   3   |   -   |      5     |   6   |   5   |   6
< ASCII >
//
// A = extension A, B = extension B, E = effective value
// .reg = regular value
// .reg_only = regular-only value
// .inc = incognito value
// Extension B has higher precedence than A.
```
## Visual type:
- #table


== ./chromium/chromium_207.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/gin/v8_isolate_memory_dump_provider.cc#L92-L107

```c
// Dump the number of native and detached contexts.
< ASCII >
// The result looks as follows in the Chrome trace viewer:
// ========================================
// Component                   object_count
// - v8
//   - main
//     - contexts
//       - detached_context  10
//       - native_context    20
//   - workers
//     - contexts
//       - detached_context
//         - isolate_0x1234  10
//       - native_context
//         - isolate_0x1234  20
// ========================================
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_208.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ios/chrome/browser/ui/authentication/views/identity_view.h#L12-L22

```c
// View with the avatar on the leading side, a title and a subtitle. Only the
// title is required. The title contains the user name if it exists, or the
// email address. The subtitle contains the email address if the name exists,
// otherwise it is hidden.
// The avatar is displayed as a round image.
< ASCII >
// +--------------------------------+
// |  +------+                      |
// |  |      |  Title (name)        |
// |  |Avatar|  Subtitle (email)    |
// |  +------+                      |
// +--------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_209.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ios/chrome/browser/ui/settings/cells/settings_check_cell.h#L12-L20

```c
< ASCII >
// Cell representation for SettingsCheckItem.
//  +---------------------------------------------------------+
//  | +--------+                                +---------+   |
//  | |        |  One line title                |trailing |   |
//  | | leading|                                |image    |   |
//  | | image  |  Multiline detail text         |spinner  |   |
//  | |        |  Multiline detail text         |or button|   |
//  | +--------+                                +---------+   |
//  +---------------------------------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_21.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/system/time/calendar_up_next_view_unittest.cc#L377-L385

```c
// If we have a partially visible event and the scroll right button is pressed,
// we should scroll to put the whole event into view, aligned to the start of
// the viewport.
< ASCII >
// If we scroll right for a partially visible event view.
//           [---------------]      <-- ScrollView viewport
//           [--E1--]    [--E2--]   <-- Event 2 partially shown in the viewport.
// Press scroll right button.
//           [---------------]      <-- ScrollView viewport
// [--E1--]  [--E2--]               <-- Event 2 now fully shown in the viewport.
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_210.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ios/chrome/browser/ui/settings/cells/settings_image_detail_text_cell.h#L12-L18

```c
< ASCII >
// Cell representation for SettingsImageDetailTextItem.
//  +--------------------------------------------------+
//  |  +-------+                                       |
//  |  | image |   Multiline title                     |
//  |  |       |   Optional multiline detail text      |
//  |  +-------+                                       |
//  +--------------------------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_211.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ios/chrome/browser/ui/tab_switcher/tab_grid/tab_grid_bottom_toolbar.h#L14-L27

```c
< ASCII >
// Bottom toolbar for TabGrid. The appearance of the toolbar is decided by
// screen size, current TabGrid page and mode:
//
// Horizontal-compact and vertical-regular screen size:
//   Small newTabButton, translucent background.
//   Incognito & Regular page: [CloseAllButton  newTabButton  DoneButton]
//   Remote page:              [                              DoneButton]
//   Selection mode:           [CloseTabButton  shareButton  AddToButton]
//
// Other screen size:
//   Large newTabButton, floating layout without UIToolbar.
//   Normal mode:    [                                      newTabButton]
//   Remote page:    [                                                  ]
//   Selection mode: [CloseTabButton       shareButton       AddToButton]
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_212.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ios/chrome/browser/ui/tab_switcher/tab_grid/tab_grid_top_toolbar.h#L14-L24

```c
< ASCII >
// Top toolbar for TabGrid. The appearance of the toolbar is decided by screen
// size, current TabGrid page and mode:
//
// Horizontal-compact and vertical-regular screen size:
//   Normal mode:    [               PageControl      Select]
//   Remote page:    [               PageControl            ]
//   Selection mode: [SelectAll    SelectedTabsCount    Done]
// Other screen size:
//   Normal mode:    [CloseAll           PageControl      Select Done]
//   Remote page:    [                   PageControl             Done]
//   Selection mode: [SelectAll        SelectedTabsCount         Done]
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_213.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/audio/audio_debug_recording_manager.h#L26-L53

```c
// A manager for audio debug recording that handles registration of data
// sources and hands them a recorder (AudioDebugRecordingHelper) to feed data
// to. The recorder will unregister with the manager automatically when deleted.
// When debug recording is enabled, it is enabled on all recorders and
// constructs a unique file name for each recorder by using a running ID.
// A somewhat simplified diagram of the the debug recording infrastructure,
// interfaces omitted:
< ASCII >
//
//                                AudioDebugFileWriter
//                                        ^
//                                        | owns
//                        owns            |                     owns
//   OnMoreDataConverter  ---->  AudioDebugRecordingHelper <---------
//            ^                           ^                          |
//            | owns several              | raw pointer to several   |
//            |                   AudioDebugRecordingManager         |
//   AudioOutputResampler                 ^                          |
//            ^                           |      AudioInputStreamDataInterceptor
//            |                           |                          ^
//            | owns several              | owns        owns several |
//             ------------------  AudioManagerBase  ----------------
//
< ASCII >
// AudioDebugRecordingManager is created when
// AudioManager::InitializeDebugRecording() is called. That is done in
// AudioManager::Create() in WebRTC enabled builds, but not in non WebRTC
// enabled builds. If AudioDebugRecordingManager is not created, neither is
// AudioDebugRecordingHelper or AudioDebugFileWriter. In this case the pointers
// to AudioDebugRecordingManager and AudioDebugRecordingHelper are null.
```
## Visual type:
- #class-diagram


== ./chromium/chromium_214.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/audio/cras/cras_unified.cc#L64-L101

```c
// Overview of operation:
// 1) An object of CrasUnifiedStream is created by the AudioManager
// factory: audio_man->MakeAudioStream().
// 2) Next some thread will call Open(), at that point a client is created and
// configured for the correct format and sample rate.
// 3) Then Start(source) is called and a stream is added to the CRAS client
// which will create its own thread that periodically calls the source for more
// data as buffers are being consumed.
// 4) When finished Stop() is called, which is handled by stopping the stream.
// 5) Finally Close() is called. It cleans up and notifies the audio manager,
// which likely will destroy this object.
//
< ASCII >
// Simplified data flow for output only streams:
//
//   +-------------+                  +------------------+
//   | CRAS Server |                  | Chrome Client    |
//   +------+------+    Add Stream    +---------+--------+
//          |<----------------------------------|
//          |                                   |
//          | Near out of samples, request more |
//          |---------------------------------->|
//          |                                   |  UnifiedCallback()
//          |                                   |  WriteAudio()
//          |                                   |
//          |  buffer_frames written to shm     |
//          |<----------------------------------|
//          |                                   |
//         ...  Repeats for each block.        ...
//          |                                   |
//          |                                   |
//          |  Remove stream                    |
//          |<----------------------------------|
//          |                                   |
< ASCII >
//
// For Unified streams the Chrome client is notified whenever buffer_frames have
// been captured.  For Output streams the client is notified a few milliseconds
// before the hardware buffer underruns and fills the buffer with another block
// of audio.
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_215.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/base/pipeline_impl.h#L39-L82

```c
// Pipeline runs the media pipeline.  Filters are created and called on the
// task runner injected into this object. Pipeline works like a state
// machine to perform asynchronous initialization, pausing, seeking and playing.
//
< ASCII >
// Here's a state diagram that describes the lifetime of this object.
//
//   [ *Created ]                       [ Any State ]
//         | Start()                         | Stop()
//         V                                 V
//   [ Starting ]                       [ Stopping ]
//         |                                 |
//         V                                 V
//   [ Playing ] <---------.            [ Stopped ]
//     |  |  | Seek()      |
//     |  |  V             |
//     |  | [ Seeking ] ---'
//     |  |                ^
//     |  | *TrackChange() |
//     |  V                |
//     | [ Switching ] ----'
//     |                   ^
//     | Suspend()         |
//     V                   |
//   [ Suspending ]        |
//     |                   |
//     V                   |
//   [ Suspended ]         |
//     | Resume()          |
//     V                   |
//   [ Resuming ] ---------'
//
< ASCII >
// Initialization is a series of state transitions from "Created" through each
// filter initialization state.  When all filter initialization states have
// completed, we simulate a Seek() to the beginning of the media to give filters
// a chance to preroll. From then on the normal Seek() transitions are carried
// out and we start playing the media.
//
// If Stop() is ever called, this object will transition to "Stopped" state.
// Pipeline::Stop() is never called from withing PipelineImpl. It's |client_|'s
// responsibility to call stop when appropriate.
//
// TODO(sandersd): It should be possible to pass through Suspended when going
// from InitDemuxer to InitRenderer, thereby eliminating the Resuming state.
// Some annoying differences between the two paths need to be removed first.
```
## Visual type:
- #state-machine 


== ./chromium/chromium_216.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/base/android/media_codec_loop.h#L35-L105

```c
// Implementation notes.
//
// The MediaCodec works by exchanging buffers between the client and the codec
// itself.  On the input side, an "empty" buffer has to be dequeued from the
// codec, filled with data, and queued back.  On the output side, a "full"
// buffer with data should be dequeued, the data used, and the buffer returned
// back to the codec.
//
// Neither input nor output dequeue operations are guaranteed to succeed: the
// codec might not have available input buffers yet, or not every encoded buffer
// has arrived to complete an output frame. In such case the client should try
// to dequeue a buffer again at a later time.
//
// There is also a special situation related to an encrypted stream, where the
// enqueuing of a filled input buffer might fail due to lack of the relevant key
// in the CDM module.  While MediaCodecLoop does not handle the CDM directly,
// it does understand the codec state.
//
// Much of that logic is common to all users of MediaCodec.  The MediaCodecLoop
// provides the main driver loop to talk to MediaCodec.  Via the Client
// interface, MediaCodecLoop can request input data, send output buffers, etc.
// MediaCodecLoop does not construct a MediaCodec, but instead takes ownership
// of one that it provided during construction.
//
// Although one can specify a delay in the MediaCodec's dequeue operations,
// this implementation uses polling.  We can't tell in advance if a call into
// MediaCodec will block indefinitely, and we don't want to block the thread
// waiting for it.
//
// This implementation detects that the MediaCodec is idle (no input or output
// buffer processing) and after being idle for a predefined time the timer
// stops.  The client is responsible for signalling us to wake up via a call
// to DoPendingWork.  It is okay for the client to call this even when the timer
// is already running.
//
// The current implementation is single threaded. Every method is supposed to
// run on the same thread.
//
< ASCII >
// State diagram.
//
//  -[Any state]-
//     |
// (MediaCodec error)
//     |
//   [Error]
//
//  [Ready]
//     |
// (EOS enqueued)
//     |
//  [Draining]
//     |
// (EOS dequeued on output)
//     |
//   [Drained]
//
//      [Ready]
//         |
// (MEDIA_CODEC_NO_KEY)
//         |
//   [WaitingForKey]
//         |
//    (OnKeyAdded)
//         |
//      [Ready]
//
//     -[Any state]-
//    |             |
// (Flush ok) (Flush fails)
//    |             |
// [Ready]       [Error]
< ASCII >
```
## Visual type:
- #state-machine 


== ./chromium/chromium_217.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/base/android/media_service_throttler.cc#L51-L66

```c
< ASCII >
// The throttling progression based on number of crashes looks as follows:
//
// | # crashes | period  | clients/sec | clients/mins | # burst clients
// | 0         | 200  ms | 5.0         | 300          | 10
// | 1         | 320  ms | 3.1         | 188          | 6
// | 2         | 560  ms | 1.8         | 107          | 4
// | 3         | 1040 ms | 1.0         | 58           | 2
// | 4         | 2000 ms | 0.5         | 30           | 1
// | 5         | 3000 ms | 0.3         | 20           | 1
// | 6         | 3000 ms | 0.3         | 20           | 1
//
< ASCII >
// NOTE: Since we use the floor function and a decay rate of 1 crash/minute when
// calculating the effective # of crashes, a single crash per minute will result
// in 0 effective crashes (since floor(1.0 - 'tiny decay') is 0). If we
// experience slightly more than 1 crash per 60 seconds, the effective number of
// crashes will go up as expected.
```
## Visual type:
- #table


== ./chromium/chromium_218.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/capture/mojom/video_capture.mojom#L11-L50

```c
// This file decribes the communication between a given Renderer Host interface
// implementation (VideoCaptureHost) and a remote VideoCaptureObserver.
// VideoCaptureHost offers a stateless part (GetDeviceSupportedFormats() and
// GetDeviceFormatsInUse()) that can be invoked at any time, and a stateful part
// sandwiched between Start() and Stop(). A Client's OnStateChanged() can be
// notified any time during the stateful part. The stateful part is composed of
// a preamble where a Renderer client sends a command to Start() the capture,
// registering itself as the associated remote VideoCaptureObserver. The Host
// will then create and pre- share a number of buffers:
< ASCII >
//
//   Observer                                        VideoCaptureHost
//      | ---> StartCapture                                     |
//      |                        OnStateChanged(STARTED) <---   |
//      |                                 OnNewBuffer(1) <---   |
//      |                                 OnNewBuffer(2) <---   |
//      =                                                       =
// and capture will then refer to those preallocated buffers:
//      |                               OnBufferReady(1) <---   |
//      |                               OnBufferReady(2) <---   |
//      | ---> ReleaseBuffer(1)                                 |
//      |                               OnBufferReady(1) <---   |
//      | ---> ReleaseBuffer(2)                                 |
//      |                               OnBufferReady(2) <---   |
//      | ---> ReleaseBuffer(1)                                 |
//      |                         ...                           |
//      =                                                       =
// Buffers can be reallocated with a larger size, if e.g. resolution changes.
//      |                 (resolution change)                   |
//      |                           OnBufferDestroyed(1) <---   |
//      |                                 OnNewBuffer(3) <---   |
//      |                               OnBufferReady(3) <---   |
//      | ---> ReleaseBuffer(2)                                 |
//      |                           OnBufferDestroyed(2) <---   |
//      |                                 OnNewBuffer(5) <---   |
//      |                               OnBufferReady(5) <---   |
//      =                                                       =
// In the communication epilogue, the client Stop()s capture, receiving a last
// status update:
//      | ---> StopCapture                                      |
//      |                        OnStateChanged(STOPPED) <---   |
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_219.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/cast/net/rtcp/rtcp_builder.cc#L292-L308

```c
< ASCII >
/*
   0                   1                   2                   3
   0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
  +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |V=2|P|reserved |   PT=XR=207   |             length            |
  +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |                              SSRC                             |
  +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |     BT=5      |   reserved    |         block length          |
  +=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+
  |                 SSRC1 (SSRC of first receiver)               | sub-
  +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+ block
  |                         last RR (LRR)                         |   1
  +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  |                   delay since last RR (DLRR)                  |
  +=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+
*/
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_22.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/utility/cropping_util.h#L17-L71

```c
// Crops an image such that its aspect ratio matches that of a target size, but
// does not perform any "scaling". The cropping is calculated with the image
// and the target rect "center-aligned". The image dimension with the smaller
// (target_size / original_size) ratio gets cropped.
//
// A visual example with a portrait image whose dimensions exceeds a landscape
// target size:
//
< ASCII >
// Before:
//
//         Portrait Image
// +---------------------------+
// |                           |
// |                           |
// |                           |
// |                           |
// |      Landscape Target     |
// |    +-----------------+    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    +-----------------+    |
// |                           |
// |                           |
// |                           |
// |                           |
// |                           |
// +---------------------------+
//
// After (ok, maybe it's not the exact same aspect ratio, but you get the idea):
//
//         Cropped Image
// +---------------------------+
// |                           |
// |      Landscape Target     |
// |    +-----------------+    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    |                 |    |
// |    +-----------------+    |
// |                           |
// |                           |
// +---------------------------+
< ASCII >
//
// The ultimate result is always a cropped image whose aspect ratio matches that
// of the target size. Therefore, the cropped image can subsequently be scaled
// up or down to match the dimensions of the target size.
//
// There are no requirements for the image and target dimensions other than that
// they're non-empty. This function cannot fail; the returned SkBitmap is always
// non-null and points to ref-counted pixel memory shared with |image|.
```
## Visual type:
- #custom


== ./chromium/chromium_220.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/cast/sender/performance_metrics_overlay.h#L11-L49

```c
< ASCII >
// This module provides a display of frame-level performance metrics, rendered
// in the lower-right corner of a VideoFrame.  It looks like this:
//
// +---------------------------------------------------------------------+
// |                         @@@@@@@@@@@@@@@@@@@@@@@                     |
// |                 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                     |
// |              @@@@@@@@@@@@@@@@@@@@@@@ @@@@@@@@@@@@                   |
// |             @@@@@@@@@@@@@                    @@@@                   |
// |            @@@@@@@@@@                        @@@@                   |
// |           @@@@@  @@@               @@@       @@@@                   |
// |           @@@     @    @@@         @@@@      @@@@                   |
// |          @@@@          @@@@                  @@@@                   |
// |          @@@@                  @@@           @@@                    |
// |            @@@@                 @@           @@@                    |
// |             @@@@@      @@@            @@@   @@@                     |
// |              @@@@@     @@@@@        @@@@   @@@@                     |
// |               @@@@@      @@@@@@@@@@@@@    @@@@                      |
// |                @@@@@@                    @@@@           1  45%  75% |
// |                    @@@@@@@@         @@@@@@            22  400. 4000 |
// |                         @@@@@@@@@@@@@@@@      16.7 1280x720 0:15.12 |
// +---------------------------------------------------------------------+
< ASCII >
//
// Line 1: Reads as, "1 frame ago, the encoder utilization for the frame was 45%
// and the lossy utilization was 75%."  For CPU-bound encoders, encoder
// utilization is usually measured as the amount of real-world time it took to
// encode the frame, divided by the maximum amount of time allowed.  Lossy
// utilization is the amount of "complexity" in the frame's content versus the
// target encoded byte size, where a value over 100% means the frame's content
// is too complex to encode within the target number of bytes.
//
// Line 2: Reads as, "Capture of this frame took 22 ms.  The current target
// playout delay is 400 ms and low-latency adjustment mode is not active.  The
// target bitrate for this frame is 4000 kbps."  If there were an exclamation
// mark (!) after the playout delay number instead of a period (.), it would
// indicate low-latency adjustment mode is active.  See VideoSender for more
// details.
//
// Line 3: Contains the frame's duration (16.7 milliseconds), resolution, and
// media timestamp in minutes:seconds.hundredths format.
```
## Visual type:
- #custom


== ./chromium/chromium_221.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/cast/test/utility/barcode.cc#L5-L22

```c
// Routines for encoding and decoding a small number of bits into an image
// in a way that is decodable even after scaling/encoding/cropping.
//
< ASCII >
// The encoding is very simple:
//
//   ####    ####    ########    ####        ####    ####
//   ####    ####    ########    ####        ####    ####
//   ####    ####    ########    ####        ####    ####
//   ####    ####    ########    ####        ####    ####
//   1   2   3   4   5   6   7   8   9   10  11  12  13  14
//   <-----start----><--one-bit-><-zero bit-><----stop---->
< ASCII >
//
// We use a basic unit, depicted here as four characters wide.
// We start with 1u black 1u white 1u black 1u white. (1-4 above)
// From there on, a "one" bit is encoded as 2u black and 1u white,
// and a zero bit is encoded as 1u black and 2u white. After
// all the bits we end the pattern with the same pattern as the
// start of the pattern.
```
## Visual type:
- #custom


== ./chromium/chromium_222.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L844-L849

```c
< ASCII >
// Using position based test API:
// DTS  :  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
// PTS  :  0  4  1  2  3  5  9  6  7  8 10 14 11 12 13 15 19 16 17 18 20
// old  :                                A  a  a  a  a  A  a  a  a  a
// new  :                 B  b  b  b  b  B  b  b
// after:                 B  b  b  b  b  B  b  b        A  a  a  a  a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_223.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L876-L881

```c
< ASCII >
// Test an end overlap edge case where a single buffer overlaps the
// beginning of a range.
// old  : 0K   30   60   90   120K  150
// new  : 0K
// after: 0K                  120K  150
// track:
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_224.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L896-L901

```c
< ASCII >
// Using position based test API:
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :            A a                 A a                 A a
// new  :  B b b b b B b b b b B b b b b B b b b b B b b b b B b b b b
// after == new
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_225.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L930-L934

```c
< ASCII >
// Using position based test API:
// DTS:0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6
// PTS:0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9
// old:          A a                 A a                 A a                 A a
// new:B b b b b B b b b b B b b b b B b b b b B b b b b B b b b b B b b b b
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_226.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1163-L1169

```c
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer is a keyframe that's being overlapped by new
// buffers.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :           *A*a a a a A a a a a
// new  :  B b b b b B b b b b
// after:  B b b b b*B*b b b b A a a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_227.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1192-L1198

```c
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is after the newly appended buffers.
//
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :           |A a a a a A a a*a*a|
// new  :  B b b b b B b b b b
// after: |B b b b b B b b b b A a a*a*a|
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_228.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1222-L1230

```c
// Using position based test API:
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is after the newly appended buffers.
//
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :            A a a a a A a a*a*a
// new  :  B b b b b B b b
// after:  B b b b b B b b     A a a*a*a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_229.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1264-L1273

```c
// Using position based test API:
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is after the newly appended buffers.
//
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :            A a a*a*a A a a a a
// new  :  B b b b b B b b
// after:  B b b b b B b b     A a a a a
// track:                  a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_23.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/utility/rounded_window_targeter_unittest.cc#L47-L57

```c
< ASCII >
/*
Window shape (global coordinates)
(0,0)_____________
    |.   * | *    | <- mouse move (1,1)
    | *    |    * |
    |*_____|     *|
    |*     (r,r) *|
    | *        *  |
    |____*___*____|
                  (2r, 2r)
This mouse event hits the square but not the circular window targeter.*/
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_230.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1310-L1317

```c
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is overlapped by the new buffers.
//
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :           |A a a*a*a A a a a a|
// new  :  B b b b b B b b b b
// after: |B b b b b B b b b b A a a a a|
// track:                 |a a|
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_231.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1343-L1352

```c
// Using position based test API:
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is overlapped by the new buffers.
//
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :            A*a*a a a A a a a a
// new  :  B b b b b B b
// after:  B b b b b B b       A a a a a
// track:              a a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_232.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1389-L1400

```c
// Using position based test API:
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next buffer in the range is overlapped by the new buffers.
// In this particular case, the next keyframe after the track buffer is in the
// range with the new buffers.
//
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :            A*a*a a a A a a a a A a a a a
// new  :  B b b b b B b b b b B
// after:  B b b b b B b b b b B         A a a a a
// track:              a a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_233.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1437-L1443

```c
// This test covers the case where new buffers end-overlap an existing, selected
// range, and there is no keyframe after the end of the new buffers.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :           |A*a*a a a|
// new  :  B b b b b B
// after: |B b b b b B|
// track:             |a a a a|
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_234.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1471-L1484

```c
// Using position based test API:
// This test covers the case where new buffers end-overlap an existing, selected
// range, and there is no keyframe after the end of the new buffers, then more
// buffers end-overlap the beginning.
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// PTS :   0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0
// old  :                      A a a a a A*a*
// new  :            B b b b b B b b b b B
// after:            B b b b b B b b b b B
// new  :  A a a a a A
// after:  A a a a a A         B b b b b B
// track:                                  a
// new  :                                B b b b b B
// after:  A a a a a A         B b b b b B b b b b B
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_235.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1547-L1553

```c
// This test covers the case where new buffers end-overlap an existing, selected
// range, and the next keyframe in a separate range.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :           |A*a*a a a|         |A a a a a|
// new  :  B b b b b B
// after: |B b b b b B|                 |A a a a a|
// track:             |a a a a|
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_236.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1593-L1599

```c
// This test covers the case when new buffers overlap the middle of a selected
// range. This tests the case when there is precise overlap of an existing GOP,
// and the next buffer is a keyframe.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :  A a a a a*A*a a a a A a a a a
// new  :            B b b b b
// after:  A a a a a*B*b b b b A a a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_237.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1624-L1630

```c
// This test covers the case when new buffers overlap the middle of a selected
// range. This tests the case when there is precise overlap of an existing GOP,
// and the next buffer is a non-keyframe in a GOP after the new buffers.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :  A a a a a A a a a a A*a*a a a
// new  :            B b b b b
// after:  A a a a a B b b b b A*a*a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_238.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1655-L1662

```c
// This test covers the case when new buffers overlap the middle of a selected
// range. This tests the case when only a partial GOP is appended, that append
// is merged into the overlapped range, and the next buffer is before the new
// buffers.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :  A a*a*a a A a a a a A a a a a
// new  :            B
// after:  A a*a*a a B         A a a a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_239.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1693-L1701

```c
// This test covers the case when new buffers overlap the middle of a selected
// range. This tests the case when only a partial GOP is appended, and the next
// buffer is after the new buffers, and comes from the track buffer until the
// next GOP in the original buffers.
< ASCII >
// index:  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
// old  :  A a a a a A a a*a*a A a a a a
// new  :            B
// after:  A a a a a B         A a a a a
// track:                  a a
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_24.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/utility/rounded_window_targeter_unittest.cc#L84-L94

```c
/*
< ASCII >
Window shape (global coordinates)
(0,0)_____________
    |    * | *    |
    | *    |    * |
    |*_____|     *| <- mouse move (r,r)
    |*     (r,r) *|
    | *        *  |
    |____*___*____|
                  (2r, 2r)
This mouse event hits both the square and the circular window targeter.*/
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_240.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1786-L1788

```c
< ASCII >
// old  :   10K  40  *70*  100K  125  130K
// new  : 0K   30   60   90   120K
// after: 0K   30   60   90  *120K*   130K
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_241.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1813-L1820

```c
< ASCII >
// Overlap the next keyframe after the end of the track buffer with a new
// keyframe.
// old  :   10K  40  *70*  100K  125  130K
// new  : 0K   30   60   90   120K
// after: 0K   30   60   90  *120K*   130K
// track:             70
// new  :                     110K    130
// after: 0K   30   60   90  *110K*   130
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_242.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1845-L1853

```c
< ASCII >
// Overlap the next keyframe after the end of the track buffer without a
// new keyframe.
// old  :   10K  40  *70*  100K  125  130K
// new  : 0K   30   60   90   120K
// after: 0K   30   60   90  *120K*   130K
// track:             70
// new  :        50K   80   110          140
// after: 0K   30   50K   80   110   140 * (waiting for keyframe)
// track:             70
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_243.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1883-L1891

```c
< ASCII >
// Overlap the next keyframe after the end of the track buffer with a keyframe
// that comes before the end of the track buffer.
// old  :   10K  40  *70*  100K  125  130K
// new  : 0K   30   60   90   120K
// after: 0K   30   60   90  *120K*   130K
// track:             70
// new  :              80K  110          140
// after: 0K   30   60   *80K*  110   140
// track:               70
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_244.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1913-L1922

```c
< ASCII >
// Overlap the next keyframe after the end of the track buffer with a keyframe
// that comes before the end of the track buffer, when the selected stream was
// waiting for the next keyframe.
// old  :   10K  40  *70*  100K
// new  : 0K   30   60   90   120
// after: 0K   30   60   90   120 * (waiting for keyframe)
// track:             70
// new  :              80K  110          140
// after: 0K   30   60   *80K*  110   140
// track:               70
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_245.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1943-L1952

```c
< ASCII >
// Test that appending to a different range while there is data in
// the track buffer doesn't affect the selected range or track buffer state.
// old  :   10K  40  *70*  100K  125  130K ... 200K 230
// new  : 0K   30   60   90   120K
// after: 0K   30   60   90  *120K*   130K ... 200K 230
// track:             70
// old  : 0K   30   60   90  *120K*   130K ... 200K 230
// new  :                                               260K 290
// after: 0K   30   60   90  *120K*   130K ... 200K 230 260K 290
// track:             70
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_246.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L1984-L2001

```c
< ASCII >
// Test that overlap-appending with a GOP that begins with time of next track
// buffer frame drops that track buffer frame and buffers the new GOP correctly.
// append :    10K   40    70     100
// read the first two buffers
// after  :    10K   40   *70*    100
//
// append : 0K    30    60    90    120
// after  : 0K    30    60    90    120
// track  :               *70*    100
//
// read the buffer at 70ms from track
// after  : 0K    30    60    90    120
// track  :                      *100*
//
// append :                       100K   130
// after  : 0K    30    60    90 *100K*  130
// track  : (empty)
// 100K, not 100, should be the next buffer read.
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_247.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/source_buffer_stream_unittest.cc#L2868-L2887

```c
// Using position based test API:
< ASCII >
// DTS  :  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4
// PTS  :  0 4 1 2 3 5 9 6 7 8 0 4 1 2 3 5 9 6 7 8 0 4 1 2 3
// old  :  A a a a a A a a a a A a a a a*A*a a
//       -- Garbage Collect --
// after:                                A a a
//       -- Read one buffer --
// after:                                A*a*a
// new  :  B b b b b B b b b b B b b b b B b b b b
// track:                                 *a*a
//       -- Garbage Collect --
// after:                                B b b b b
// track:                                 *a*a
//       -- Read 2 buffers -> exhausts track buffer
// after:                                B b b b b
//       (awaiting next keyframe after GOP at position 15)
// new  :                                         *B*b b b b
// after:                                B b b b b*B*b b b b
//       -- Garbage Collect --
// after:                                         *B*b b b b
< ASCII >
```
## Visual type:
- #sequence 


== ./chromium/chromium_248.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L235-L239

```c
< ASCII >
// ┌───────────────────┬────────────────────┐
// │ frame 1           │ frame 2            │
// ┝━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━┥
// │ clear1                  | cipher 1     │
// └────────────────────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_249.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L275-L279

```c
< ASCII >
// ┌─────────────────────────┬────────────────────┐
// │ frame 1                 │ frame 2            │
// ┝━━━━━━━━━━━━━━━━━━━┯━━━━━┷━━━━━━━━━━━━━━━━━━━━┥
// │ clear1 | cipher 1 │ clear 2       | cipher 2 │
// └───────────────────┴──────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_25.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/utility/rounded_window_targeter_unittest.cc#L121-L131

```c
/*
< ASCII >
Window shape (global coordinates)
(0,0)_____________
    |    * | *    |
    | *    |    * |
    |*_____|     *|
    |*     (r,r) *|
    | *        *  |
    |____*___*____|
                  (2r, 2r) <- <- mouse move (2r, 2r)
This mouse event misses both the square and the circular window targeter.*/
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_250.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L318-L322

```c
< ASCII >
// ┌────────────────────────────────────────┬────────────────────┐
// │ frame 1                                │ frame 2            │
// ┝━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━┥
// │ clear1 | cipher 1 │ clear 2 | cipher 2 │ clear 3 | cipher 3 │
// └───────────────────┴────────────────────┴────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_251.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L364-L368

```c
< ASCII >
// ┌─────────────────────────────────────────────┬───────────────┐
// │ frame 1                                     │ frame 2       │
// ┝━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━┯━━━━┷━━━━━━━━━━━━━━━┥
// │ clear1 | cipher 1 │ clear 2 | cipher 2 │ clear 3 | cipher 3 │
// └───────────────────┴────────────────────┴────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_252.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L414-L418

```c
< ASCII >
// ┌───────────────────┬────────────────────┐
// │ frame 1           │ frame 2            │
// ┝━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━┥
// │ clear1      | cipher 1                 │
// └────────────────────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_253.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L448-L452

```c
< ASCII >
// ┌─────────────────────────────────────┬─────────┐
// │ single frame in superframe          │ marker  │
// ┝━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━┥
// │ clear1 = 0  | cipher 1                        │
// └───────────────────────────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_254.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/vp9_parser_unittest.cc#L483-L487

```c
< ASCII >
// ┌─────────────────────────────────────┬─────────┐
// │ single frame in superframe          │ marker  │
// ┝━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━┥
// │ clear1                                        │
// └───────────────────────────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_255.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/filters/android/media_codec_audio_decoder.h#L29-L69

```c
// Implementation notes.
//
// This class provides audio decoding via MediaCodec.  It allocates the
// MediaCodecBridge instance, and hands ownership to MediaCodecLoop to drive I/O
// with the codec.  For encrypted streams, we also talk to the DRM bridge.
//
// Because both dequeuing and enqueuing of an input buffer can fail, the
// implementation puts the input |DecoderBuffer|s and the corresponding decode
// callbacks into an input queue. The decoder has a timer that periodically
// fires the decoding cycle that has two steps. The first step tries to send the
// front buffer from the input queue to MediaCodecLoop. In the case of success
// the element is removed from the queue, the decode callback is fired and the
// decoding process advances. The second step tries to dequeue an output buffer,
// and uses it in the case of success.
//
// An EOS buffer is handled differently.  Success is not signalled to the decode
// callback until the EOS is received at the output.  So, for EOS, the decode
// callback indicates that all previous decodes have completed.
//
// The failures in both steps are normal and they happen periodically since
// both input and output buffers become available at unpredictable moments. The
// timer is here to repeat the dequeueing attempts.
//
< ASCII >
// State diagram.
//
//   [Uninitialized] <-> (init failed)
//     |         |
//   (no enc.)  (encrypted)
//     |         |
//     |        [WaitingForMediaCrypto] -- (OMCR failure) --> [Uninitialized]
//     |               | (OnMediaCryptoReady success)
//     v               v
//   (Create Codec and MediaCodecLoop)
//     |
//     \--> [Ready] -(any error)-> [Error]
//
//     -[Any state]-
//    |             |
// (Reset ok) (Reset fails)
//    |             |
// [Ready]       [Error]
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_256.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/gpu/vaapi/va_surface.h#L19-L84

```c
// A VA-API-specific decode surface used by VaapiH264Decoder to decode into
// and use as reference for decoding other surfaces. It is also handed by the
// decoder to VaapiVideoDecodeAccelerator when the contents of the surface are
// ready and should be displayed. VAVDA converts the surface contents into an
// X/Drm Pixmap bound to a texture for display and releases its reference to it.
// Decoder releases its references to the surface when it's done decoding and
// using it as reference. Note that a surface may still be used for reference
// after it's been sent to output and also after it is no longer used by VAVDA.
// Thus, the surface can be in use by both VAVDA and the Decoder at the same
// time, or by either of them, with the restriction that VAVDA will never get
// the surface until the contents are ready, and it is guaranteed that the
// contents will not change after that.
// When both the decoder and VAVDA release their references to the surface,
// it is freed and the release callback is executed to put the surface back
// into available surfaces pool, which is managed externally.
//
< ASCII >
//   VASurfaceID is allocated in VaapiWrapper.
//        |
// +----->|
// |      v
// | VASurfaceID is put onto VaapiVideoDecodeAccelerator::available_va_surfaces_
// |      |      list.
// |      v
// | VASurfaceID is taken off of the VVDA:available_va_surfaces_ when
// |      |      VaapiH264Decoder requests more output surfaces, is wrapped into
// |      |      a VASurface and passed to VaapiH264Decoder.
// |      v
// | VASurface is put onto VaapiH264Decoder::available_va_surfaces_, keeping
// |      |      the only reference to it until it's needed for decoding.
// |      v
// | VaapiH264Decoder starts decoding a new frame. It takes a VASurface off of
// |      |      VHD::available_va_surfaces_ and assigns it to a DecodeSurface,
// |      |      which now keeps the only reference.
// |      v
// | DecodeSurface is used for decoding, putting data into associated VASurface.
// |      |
// |      |--------------------------------------------------+
// |      |                                                  |
// |      v                                                  v
// | DecodeSurface is to be output.              VaapiH264Decoder uses the
// | VaapiH264Decoder passes the associated      DecodeSurface and associated
// | VASurface to VaapiVideoDecodeAccelerator,   VASurface as reference for
// | which stores it (taking a ref) on           decoding more frames.
// | pending_output_cbs_ queue until an output               |
// | VaapiPicture becomes available.                         v
// |                 |                           Once the DecodeSurface is not
// |                 |                           needed as reference anymore,
// |                 v                           it is released, releasing the
// | A VaapiPicture becomes available after      associated VASurface reference.
// | the client of VVDA returns                              |
// | a PictureBuffer associated with it. VVDA                |
// | puts the contents of the VASurface into                 |
// | it and releases the reference to VASurface.             |
// |                 |                                       |
// |                 '---------------------------------------'
// |                                     |
// |                                     v
// | Neither VVDA nor VHD hold a reference to VASurface. VASurface is released,
// | ReleaseCB gets called in its destructor, which puts the associated
// | VASurfaceID back onto VVDA::available_va_surfaces_.
// |                                     |
// '-------------------------------------|
//                                       |
//                                       v
//                       VaapiWrapper frees VASurfaceID.
< ASCII >
//
```
## Visual type:
- #flowchart


== ./chromium/chromium_257.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/mojo/mojom/video_encode_accelerator.mojom#L15-L37

```c
// This file is the Mojo version of the media::VideoEncodeAccelerator interface
// and describes the communication between a Client and a remote "service"
// VideoEncodeAccelerator (VEA) with the purpose of encoding Video Frames by
// means of hardware accelerated features.
< ASCII >
//
//   Client                                    VideoEncodeAccelerator
//      | ---> Initialize                                       |
//      |                     RequireBitstreamBuffers(N) <---   |
//      | ---> UseOutputBitstreamBuffer(0)                      |
//      | ---> UseOutputBitstreamBuffer(1)                      |
//      |  ...                                                  |
//      =                                                       =
< ASCII >
// The Client requests a remote Encode() and eventually the VEA will leave the
// encoded results in a pre-shared BitstreamBuffer, that is then restored to the
// VEA when the Client is finished with it. Note that there might not be a 1:1
// relationship between Encode() and BitstreamBufferReady() calls.
< ASCII >
//      | ---> Encode()                                         |
//      |                        BitstreamBufferReady(k) <---   |
//      | ---> UseOutputBitstreamBuffer(k)                      |
//      =                                                       =
< ASCII >
// At any time the VEA can send a NotifyError() to the Client. Similarly at any
// time the Client can send a RequestEncodingParametersChange() to the VEA. None
// of these messages are acknowledged.
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_258.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/mojo/services/webrtc_video_perf_history.cc#L105-L109

```c
// Returns a UMA index for logging. The index corresponds to the key and the
// outcome of the smoothness prediction. Each bit in the index has the
// following meaning:
< ASCII >
// bit | 12   11   10  |  9    8    7    6  |  5  |  4  |  3    2    1    0  |
//     |   pixels ix   |    codec profile   | res |is_hw| smooth prediction  |
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_259.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/renderers/shared_image_video_frame_test_utils.h#L31-L35

```c
< ASCII >
// Creates a shared image backed frame in RGBA format, with colors on the shared
// image mapped as follow.
// Bk | R | G | Y
// ---+---+---+---
// Bl | M | C | W
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_26.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/webui/common/resources/keyboard_key.js#L16-L61

```c
/**
 * @fileoverview
 * 'keyboard-key' provides a visual representation of a single key for the
 * 'keyboard-diagram' component. A single 'keyboard-key' can display one "main
 * glyph" and up to four "corner glyphs".
 *
 * The main glyph can be a text string (specified by 'main-glyph') or an icon
 * (specified by 'icon'). By default it is centered vertically and horizontally
 * on the key, and icons are scaled to fill the key while maintaining their
 * aspect ratio. The main glyph supports ellipsing text strings that don't fit
 * on the key, making it suitable for long labels such as "backspace" as well as
 * letter keys with single characters on them.
 *
 * Adding the 'left' or 'right' classes to the key will align the main glyph to
 * the left or right side respectively, and set icon widths to 24px.
 *
 * The four corner glyphs ('top-left-glyph', 'bottom-left-glyph', etc.) must be
 * text strings, generally single characters. If at least one of the right-side
 * glyphs is set, the corner glyphs will be arranged in the four quadrants of
 * the key like so:
 *
< ASCII >
 * +---+
 * |a c|
 * |   |
 * |b d|
 * +---+
 *
 * If neither right-side glyph is set, the "left" glyphs will be centered
 * horizontally, like so:
 *
 * +---+
 * | a |
 * |   |
 * | b |
 * +---+
 *
 * Both a main glyph and one or more corner glyphs may be set, for example for
 * the 'e' key on a German keyboard, which has a centered main glyph but also a
 * Euro symbol in the bottom-right:
 *
 * +---+
 * |   |
 * | e |
 * |  €|
 * +---+
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_260.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/media/video/vpx_video_encoder.cc#L40-L43

```c
< ASCII >
// Frame Pattern:
// Layer Index 0: |0| | | |4| | | |8| |  |  |12|
// Layer Index 1: | | |2| | | |6| | | |10|  |  |
// Layer Index 2: | |1| |3| |5| |7| |9|  |11|  |
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_261.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/mojo/core/embedder_unittest.cc#L181-L194

```c
< ASCII >
// The sequence of messages sent is:
//       server_mp   client_mp   mp0         mp1         mp2         mp3
//   1.  "hello"
//   2.              "world!"
//   3.                          "FOO"
//   4.  "Bar"+mp1
//   5.  (close)
//   6.              (close)
//   7.                                                              "baz"
//   8.                                                              (closed)
//   9.                                      "quux"+mp2
//  10.                          (close)
//  11.                                      (wait/cl.)
//  12.                                                  (wait/cl.)
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_262.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/mojo/core/ipcz_driver/mojo_message.cc#L24-L45

```c
// Data pipe attachments come in two parts within a message's handle list: the
// DataPipe object wherever it was placed by the sender, and its control portal
// as a separate attachment at the end of the handle list. For a message with
// two data pipes endpoints (X and Y) and two message pipe endpoints(A and B),
// sent in the order AXBY, a well-formed message will have 6 total handles
// attached:
< ASCII >
//
// Message Pipe A   Message Pipe B   DataPipe X's portal
//      |               |              |
//     0:A     1:X     2:B     3:Y    4:x    5:y
//              |               |             |
//          DataPipe X       DataPipe Y      DataPipe Y's portal
< ASCII >
//
// This function validates that each DataPipe in `handles` has an associated
// portal, and it fixes up `handles` by stripping those portals off the end of
// the list and passing ownership to their corresponding DataPipe object.
//
// Returns true if and only if the handle list is well-formed in this regard.
//
// TODO(https://crbug.com/1382170): Since boxes now support application objects,
// DataPipe can be migrated out of the driver and we can avoid this whole
// serialization hack.
```
## Visual type:
- #sequence


== ./chromium/chromium_263.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/native_client_sdk/src/libraries/third_party/pthreads-win32/implement.h#L512-L526

```c
< ASCII >
/*
 * --------------------------------------------------------------
 * MAKE_SOFTWARE_EXCEPTION
 *      This macro constructs a software exception code following
 *      the same format as the standard Win32 error codes as defined
 *      in WINERROR.H
 *  Values are 32 bit values laid out as follows:
 *
 *   1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
 *  +---+-+-+-----------------------+-------------------------------+
 *  |Sev|C|R|     Facility          |               Code            |
 *  +---+-+-+-----------------------+-------------------------------+
 *
 * Severity Values:
 */
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_264.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/net/quic/web_transport_client.h#L26-L35

```c
< ASCII >
// Diagram of allowed state transitions:
//
//    NEW -> CONNECTING -> CONNECTED -> CLOSED
//              |                |
//              |                |
//              +---> FAILED <---+
//
< ASCII >
// These values are logged to UMA. Entries should not be renumbered and
// numeric values should never be reused. Please keep in sync with
// "QuicTransportClientState" in src/tools/metrics/histograms/enums.xml.
```
## Visual type:
- #state-machine


== ./chromium/chromium_265.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ppapi/proxy/dispatcher.h#L33-L46

```c
// An interface proxy can represent either end of a cross-process interface
// call. The "source" side is where the call is invoked, and the "target" side
// is where the call ends up being executed.
< ASCII >
//
// Plugin side                          | Browser side
// -------------------------------------|--------------------------------------
//                                      |
//    "Source"                          |    "Target"
//    InterfaceProxy ----------------------> InterfaceProxy
//                                      |
//                                      |
//    "Target"                          |    "Source"
//    InterfaceProxy <---------------------- InterfaceProxy
< ASCII >
//                                      |
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_266.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ppapi/proxy/raw_var_data.h#L38-L53

```c
// Contains the data associated with a graph of connected PP_Vars. Useful for
// serializing/deserializing a graph of PP_Vars. First we compute the transitive
// closure of the given PP_Var to find all PP_Vars which are referenced by that
// var. A RawVarData object is created for each of these vars. We then write
// data contained in each RawVarData to the message. The format looks like this:
< ASCII >
//    idx | size     | (number of vars in the graph)
//     0  | var type |
//        | var data |
//     1  | var type |
//        | var data |
//     2  | var type |
//        | var data |
//        |   ....   |
//
// Vars that reference other vars (such as Arrays or Dictionaries) use indices
// into the message to denote which PP_Var is pointed to.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_267.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/client/display/gl_math.h#L13-L19

```c
< ASCII >
// Transposes matrix [ m0, m1, m2, m3, m4, m5, m6, m7, m8 ]:
//
// | m0, m1, m2, |   | x |
// | m3, m4, m5, | * | y |
// | m6, m7, m8  |   | 1 |
//
// Into [ m0, m3, m6, m1, m4, m7, m2, m5, m8 ].
< ASCII >
```
## Visual type:
- #matrix


== ./chromium/chromium_268.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/client/ui/view_matrix.h#L12-L15

```c
< ASCII >
// A 2D non-skew equally scaled transformation matrix.
// | SCALE, 0,     OFFSET_X, |
// | 0,     SCALE, OFFSET_Y, |
// | 0,     0,     1         |
< ASCII >
```
## Visual type:
- #matrix


== ./chromium/chromium_269.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/client_session_unittest.cc#L387-L393

```c
< ASCII >
// Set up multiple displays:
// +-----------+
// |  800x600  |---------------+
// |     0     |   1024x768    |
// +-----------+       1       |
//             |               |
//             +---------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_27.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/webui/media_app_ui/resources/js/piex_module.js#L32-L77

```c
/**
 * A minimal EXIF header to include an orientation field. Includes the "Start of
 * image" prefix that should already be present, so that this can just form the
 * start of the resulting JPEG; with the quantization table following.
 *
 * The table at https://www.exif.org/Exif2-2.PDF#page=70 was the starting point
 * for this, but details come from all over the document.
< ASCII >
 *
 * | Offset | Code |    Meaning      |
 * | ------ | ---- | --------------- |
 * |   -2   | 0xFF | SOI Prefix      |
 * |   -1   | 0xD8 | Start of Image  |
 * |   +0   | 0xFF | Marker Prefix   |
 * |   +1   | 0xE1 | APP1            |
 * |   +2   | 0x.. | Length[1]       |  // Always big-endian.
 * |   +3   | 0x.. | Length[0]       |  // SIZEOF_APP1_HEADER.
 * |   +4   | 0x45 | 'E'             |
 * |   +5   | 0x78 | 'x'             |
 * |   +6   | 0x69 | 'i'             |
 * |   +7   | 0x66 | 'f'             |
 * |   +8   | 0x00 | NULL            |
 * |   +9   | 0x00 | Padding         |
 * |        |      | TIFF header     |  // 8 bytes. Offsets start from here.
 * |   +10  | 0x49 | ByteOrder ('I') |  // Little-endian (seems more common).
 * |   +11  | 0x49 | ByteOrder ('I') |
 * |   +12  | 0x2a | IFD marker[0]   |
 * |   +13  | 0x00 | IFD marker[1]   |
 * |   +14  | 0x08 | IFD offset[0]   |  // 8 (0th IFD immediately follows).
 * |   +15  | 0x00 | IFD offset[1]   |
 * |   +16  | 0x00 | IFD offset[2]   |
 * |   +17  | 0x00 | IFD offset[3]   |
 * |        |      | IFD frame       |  // 18 bytes
 * |   +18  | 0x01 | Field count[0]  |  // 1 Field (just orientation).
 * |   +19  | 0x00 | Field count[1]  |
 * |   +20  | 0x12 | TAG[0]          |  // E.g. TAG_ORIENTATION (0x0112)
 * |   +21  | 0x01 | TAG[1]          |
 * |   +22  | 0x03 | Data type[0]    |  // 3 = "Short"
 * |   +23  | 0x00 | Data type[1]    |
 * |   +24  | 0x01 | Data count[0]   |  // 1
 * |   +25  | 0x00 | Data count[1]   |
 * |   +26  | 0x00 | Data count[2]   |
 * |   +27  | 0x00 | Data count[3]   |
 * | 28-29  | 0x.. | Value           |  // Orientation goes here! <omitted>
 * | 30-31  | 0x00 | Padding         |
 * | 32-35  | 0x00 | Offset to next  |  // 0 to indicate "no more".
< ASCII >
 */
```
## Visual type:
- #table


== ./chromium/chromium_270.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/client_session_unittest.cc#L406-L411

```c
< ASCII >
// Set up multiple displays that are the same size:
// +-----------+
// |  800x600  |-----------+
// |     0     |  800x600  |
// +-----------+     1     |
//             +-----------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_271.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info.cc#L81-L102

```c
// Calculate the offset from the origin of the desktop to the origin of the
// specified display.
//
// For Mac and ChromeOS, the origin of the desktop is the origin of the default
// display.
//
// For Windows/Linux, the origin of the desktop is the upper-left of the
// entire desktop region.
< ASCII >
//
// x         b-----------+            ---
//           |           |             |  y-offset to c
// a---------+           |             |
// |         +-------c---+-------+    ---
// |         |       |           |
// +---------+       |           |
//                   +-----------+
//
// |-----------------|
//    x-offset to c
//
// x = upper left of desktop
// a,b,c = origin of display A,B,C
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_272.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L46-L50

```c
< ASCII >
// o---------+
// | 0       |
// | 300x200 |
// +---------+
// o = desktop origin
< ASCII >
```
## Visual type:
- #custom  


== ./chromium/chromium_273.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L57-L61

```c
< ASCII >
// o---------+------------+
// | 0       | 1          |
// | 300x200 | 500x400    |
// +---------+            |
//           +------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_274.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L70-L74

```c
< ASCII >
// o---------+------------+
// | 1       | 0          |
// | 300x200 | 500x400    |
// +---------+            |
//           +------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_275.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L83-L87

```c
< ASCII >
// +---------o------------+
// | 1       | 0          |
// | 300x200 | 500x400    |
// +---------+            |
//           +------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_276.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L101-L105

```c
< ASCII >
// +---------o------------+
// | 0       | 1          |
// | 300x200 | 500x400    |
// +---------+            |
//           +------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_277.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L119-L123

```c
< ASCII >
// +---------o------------+
// | 0       | 1          +---------+
// | 300x200 | 500x400    | 2       |
// +---------+            | 400x350 |
//           +------------+---------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_278.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L140-L148

```c
< ASCII >
//  x         o-----------+            - 0
//            | 0         |
//  +---------+ 500x400   |            - 350
//  | 2       +-------+---+-------+    - 400
//  | 300x200 |       | 1         |
//  +---------+       | 600x450   |    - 550
//                    +-----------+    - 950
//  |         |       |   |       |
// -300       0      300 500     900
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_279.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/host/desktop_display_info_unittest.cc#L165-L181

```c
< ASCII >
//  x                     +-------+               -- -50
//                        | 1     |
//          +-------+     | 60x40 |               -- -20
//          | 4     |     +---+---+-----+         -- -10
//          | 40x70 o---------+ 0       |         -- 0
//  +-------+       | 6       | 70x60   |         -- 40
//  | 2     +-------+ 80x55   +------+--+-----+   -- 50
//  | 30x60 |       +---------+      | 3      |   -- 55
//  |       |                        | 55x65  |
//  +-------+               +--------+        |   -- 100
//                          | 5      +--------+   -- 115
//                          | 65x20  |
//                          +--------+            -- 120
//  |       |       |     | | |   |  |  |     |
//  -       -       0     6 7 8   1  1  1     1
//  7       4             0 0 0   2  3  5     9
//  0       0                     0  5  0     0
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_28.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/desks/desk_preview_view.h#L27-L69

```c
// A view that shows the contents of the corresponding desk in its mini_view.
< ASCII >
// This view has the following layer hierarchy:
//
//                +---------------------------+
//                |             <-------------+------  This view's layer.
//                +---------------------------+
//              /    |          |               \  ----->>>>> Higher in Z-order.
//             /     |          |                \
//     +-----+    +-----+    +-----+               +-----+
//     |     |    |     |    |     |               |     |
//     +-----+    +-----+    +-----+               +-----+
//        ^          ^          ^    \                ^
//        |          |          |     \ +-----+       |
//        |          |          |       |     |       |
//        |          |          |       +-----+       |
//        |          |          |          ^          |
//        |          |          |          |   `highlight_overlay_`'s layer:
//        |          |          |          |   A solid color layer that is
//        |          |          |          |   visible when `mini_view_`'s
//        |          |          |          |   `DeskActionContextMenu` is open.
//        |          |          |          |
//        |          |          |          |
//        |          |          |    The root layer of the desk's mirrored
//        |          |          |    contents layer tree. This tree is owned by
//        |          |          |    `desk_mirrored_contents_layer_tree_owner_`.
//        |          |          |
//        |          |          |
//        |          |     `desk_mirrored_contents_view_`'s layer: Will be the
//        |          |      parent layer of the desk's contents mirrored layer
//        |          |      tree.
//        |          |
//        |          |
//        |     `wallpaper_preview_`'s layer: On which the wallpaper is painted
//        |      without the dimming and blur that overview mode adds.
//        |
//        |
//     `shadow_layer_`: A layer that paints a shadow behind this view.
< ASCII >
//
// Note that `desk_mirrored_contents_view_`, `wallpaper_preview_`, and
// `highlight_overlay_` paint to layers with rounded corners. In order to use
// the fast rounded corners implementation we must make them sibling layers,
// rather than one being a descendant of the other. Otherwise, this will trigger
// a render surface.
```
## Visual type:
- #custom


== ./chromium/chromium_280.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/protocol/mouse_input_filter_unittest.cc#L200-L207

```c
< ASCII >
// Default display = Left (A)
// o-------------+-----------------+
// | A           | B               |
// | 2560x1440   | 3840x2160       |
// |             |                 |
// |-------------+                 |
//               +-----------------+
// o = desktop origin
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_281.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/protocol/mouse_input_filter_unittest.cc#L241-L248

```c
< ASCII >
// Default display = Right (A)
// +-----------------o-------------+
// | B               | A           |
// | 3840x2160       | 2560x1440   |
// |                 |             |
// |                 |-------------+
// +-----------------+
// o = desktop origin
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_282.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/remoting/protocol/video_frame_pump.h#L33-L67

```c
// Class responsible for scheduling frame captures from a screen capturer,
// delivering them to a VideoEncoder to encode, and finally passing the encoded
// video packets to the specified VideoStub to send on the network.
//
// THREADING
//
// This class is supplied TaskRunners to use for capture, encode and network
// operations.  Capture, encode and network transmission tasks are interleaved
// as illustrated below:
< ASCII >
//
// |       CAPTURE       ENCODE     NETWORK
// |    .............
// |    .  Capture  .
// |    .............
// |                  ............
// |                  .          .
// |    ............. .          .
// |    .  Capture  . .  Encode  .
// |    ............. .          .
// |                  .          .
// |                  ............
// |    ............. ............ ..........
// |    .  Capture  . .          . .  Send  .
// |    ............. .          . ..........
// |                  .  Encode  .
// |                  .          .
// |                  .          .
// |                  ............
// | Time
// v
< ASCII >
//
// VideoFramePump would ideally schedule captures so as to saturate the slowest
// of the capture, encode and network processes.  However, it also needs to
// rate-limit captures to avoid overloading the host system, either by consuming
// too much CPU, or hogging the host's graphics subsystem.
```
## Visual type:
- #custom


== ./chromium/chromium_283.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/linux/syscall_broker/remote_syscall_arg_handler_unittest.cc#L117-L117

```c
< ASCII >
// | path + null_byte |
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_284.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/linux/syscall_broker/remote_syscall_arg_handler_unittest.cc#L122-L122

```c
< ASCII >
// | zero + path... | ...path + null_byte + zero |
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_285.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/linux/syscall_broker/remote_syscall_arg_handler_unittest.cc#L132-L132

```c
< ASCII >
// | path... | ...path |
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_286.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/linux/syscall_broker/remote_syscall_arg_handler_unittest.cc#L138-L138

```c
< ASCII >
// | path... | null_byte + zero |
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_287.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/crosscall_params.h#L167-L204

```c
// ActualCallParams models an specific IPC call parameters with respect to the
// storage allocation that the packed parameters should need.
// NUMBER_PARAMS: the number of parameters, valid from 1 to N
// BLOCK_SIZE: the total storage that the NUMBER_PARAMS parameters can take,
// typically the block size is defined by the channel size of the underlying
// ipc mechanism.
// In practice this class is used to levergage C++ capacity to properly
// calculate sizes and displacements given the possibility of the packed params
// blob to be complex.
//
< ASCII >
// As is, this class assumes that the layout of the blob is as follows. Assume
// that NUMBER_PARAMS = 2 and a 32-bit build:
//
// [ tag                4 bytes]
// [ IsInOut            4 bytes]
// [ call return       52 bytes]
// [ params count       4 bytes]
// [ parameter 0 type   4 bytes]
// [ parameter 0 offset 4 bytes] ---delta to ---\
// [ parameter 0 size   4 bytes]                |
// [ parameter 1 type   4 bytes]                |
// [ parameter 1 offset 4 bytes] ---------------|--\
// [ parameter 1 size   4 bytes]                |  |
// [ parameter 2 type   4 bytes]                |  |
// [ parameter 2 offset 4 bytes] ----------------------\
// [ parameter 2 size   4 bytes]                |  |   |
// |---------------------------|                |  |   |
// | value 0     (x bytes)     | <--------------/  |   |
// | value 1     (y bytes)     | <-----------------/   |
// |                           |                       |
// | end of buffer             | <---------------------/
// |---------------------------|
< ASCII >
//
// Note that the actual number of params is NUMBER_PARAMS + 1
// so that the size of each actual param can be computed from the difference
// between one parameter and the next down. The offset of the last param
// points to the end of the buffer and the type and size are undefined.
//
```
## Visual type:
- #memory-layout


== ./chromium/chromium_288.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/crosscall_server.h#L18-L46

```c
// This is the IPC server interface for CrossCall: The  IPC for the Sandbox
// On the server, CrossCall needs two things:
// 1) threads: Or better said, someone to provide them, that is what the
//             ThreadPool is for. These thread(s) are
//             the ones that will actually execute the IPC data retrieval.
//
// 2) a dispatcher: This interface represents the way to route and process
//                  an  IPC call given the  IPC tag.
//
// The other class included here CrossCallParamsEx is the server side version
// of the CrossCallParams class of /sandbox/crosscall_params.h The difference
// is that the sever version is paranoid about the correctness of the IPC
// message and will do all sorts of verifications.
//
< ASCII >
// A general diagram of the interaction is as follows:
//
//                                 ------------
//                                 |          |
//  ThreadPool<-------(1)Register--|  IPC     |
//      |                          | Implemen |
//      |                          | -tation  |
//     (2)                         |          |  OnMessage
//     IPC fired --callback ------>|          |--(3)---> Dispatcher
//                                 |          |
//                                 ------------
< ASCII >
//
//  The  IPC implementation sits as a middleman between the handling of the
//  specifics of scheduling a thread to service the  IPC and the multiple
//  entities that can potentially serve each particular IPC.
```
## Visual type:
- #flowchart


== ./chromium/chromium_289.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/policy_engine_opcodes.h#L247-L271

```c
// Helper class to create any opcode sequence. This class is normally invoked
// only by the high level policy module or when you need to handcraft a special
// policy.
// The factory works by creating the opcodes using a chunk of memory given
// in the constructor. The opcodes themselves are allocated from the beginning
// (top) of the memory, while any string that an opcode needs is allocated from
// the end (bottom) of the memory.
//
< ASCII >
// In essence:
//
//   low address ---> [opcode 1]
//                    [opcode 2]
//                    [opcode 3]
//                    |        | <--- memory_top_
//                    | free   |
//                    |        |
//                    |        | <--- memory_bottom_
//                    [string 1]
//   high address --> [string 2]
//
< ASCII >
// Note that this class does not keep track of the number of opcodes made and
// it is designed to be a building block for low-level policy.
//
// Note that any of the MakeOpXXXXX member functions below can return nullptr on
// failure. When that happens opcode sequence creation must be aborted.
```
## Visual type:
- #memory-layout


== ./chromium/chromium_29.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/desks/root_window_desk_switch_animator.h#L30-L179

```c
// Performs the desk switch animation on a root window (i.e. display). Since a
// desk spans all displays, one instance of this object will be created for each
// display when a new desk is activated.
//
// Screenshots of the starting and ending desks are taken, and we animate
// between them such that the starting desk can appear sliding out of the
// screen, while the ending desk is sliding in. We take screenshots to make the
// visible state of the desks seem constant to the user (e.g. if the starting
// desk is in overview, it appears to remain in overview while sliding out).
// This approach makes it possible to show an empty black space separating both
// desks while we animate them (See |kDesksSpacing|). The ending desk may change
// after the animation has started. In this case, a new animation will replace
// the current one and animate to the new ending desk, requesting a new
// screenshot if necessary.
// - `starting` desk: is the currently activated desk which will be deactivated
//    shortly.
// - `ending` desk: is the desk desired to be activated with this animation.
// These can be changed if the enhanced desk animations feature is enabled using
// ReplaceAnimation() or UpdateSwipeAnimation().
//
// The animation goes through the following phases:
//
// - Phase (1) begins by calling TakeStartingDeskScreenshot(), which should be
//   called before the ending desk is activated.
//   * Once the screenshot is taken, it is placed in a layer that covers
//     everything on the screen, so that desk activation can happen without
//     being seen.
//   * Delegate::OnStartingDeskScreenshotTaken() is called, and the owner of
//     this object can check that all animators of all root windows have
//     finished taking the starting desk screenshots (through checking
//     starting_desk_screenshot_taken()), upon which the actual ending desk
//     activation can take place, and phase (2) of the animation can be
//     triggered.
//
// - Phase (2) should begin after the ending desk had been activated,
//   by calling TakeEndingDeskScreenshot().
//   * Once the screenshot is taken, it is placed in a sibling layer to the
//     starting desk screenshot layer, with an offset of |kDesksSpacing| between
//     the two layers.
//   * Delegate::OnEndingDeskScreenshotTaken() will be called, upon which the
//     owner of this object can check if all ending desks screenshots on all
//     roots are taken by all animators (through checking
//     ending_desk_screenshot_taken()), so that it can start phase (3) on all of
//     them at the same time.
//   * Phase (2) can be rentered after starting phase (3) by calling
//     ReplaceAnimation() or UpdateSwipeAnimation(). The new ending desk will
//     change, and if it does not have an associated screenshot layer, the
//     caller will be responsible for requesting one using
//     TakeEndingDeskScreenshot(). The screenshots are taken as needed since
//     their layers are fullscreen and require activating a desk which may be a
//     large operation for something that the user may not see. Once the
//     screenshot is taken, it is kept until |this| is destroyed. If an
//     associated screenshot layer exists already, ReplaceAnimation() and
//     UpdateSwipeAnimation() can proceed without returning to phase (2).
//
// - Phase (3) begins when StartAnimation() is called.
//   * The parent layer of both screenshot layers is animated, either:
//     - To the left (starting_desk_index_ < ending_desk_index_); when the
//       starting desk is on the left.
< ASCII >
//
//              <<<<<-------------------------- move left.
//                       +-----------+
//                       | Animation |
//                       |  layer    |
//                       +-----------+
//                         /        \
//              +------------+      +------------+
//              | start desk |      | end desk   |
//              | screenshot |      | screenshot |
//              |  layer     |      |  layer     |
//              +------------+      +------------+
//                    ^
//                start here
< ASCII >
//
//       Animation layer transforms:
//       * Begin transform: The transform that will make the starting desk
//         screenshot visible. In this case it is a transform with translation
//         (edge_padding_width_dp_, 0).
//       * End transform: The transform that will make the ending desk
//         screenshot visible. In this case it is a transform with translation
//         (-|edge_padding_width_dp_| - |x_translation_offset_| -
//         |kDesksSpacing|, 0).
//
//     - Or to the right (starting_desk_index_ > ending_desk_index_), when the
//       starting desk is on the right.
< ASCII >
//
//          move right. -------------------------->>>>>
//                       +-----------+
//                       | Animation |
//                       |  layer    |
//                       +-----------+
//                         /        \
//              +------------+      +------------+
//              | end desk   |      | start desk |
//              | screenshot |      | screenshot |
//              |  layer     |      |  layer     |
//              +------------+      +------------+
//                                        ^
//                                    start here
< ASCII >
//
//       Animation layer transforms:
//       * Begin transform: The transform that will make the starting desk
//         screenshot visible. In this case it is a transform with translation
//         (-|edge_padding_width_dp_| - |x_translation_offset_| -
//         |kDesksSpacing|, 0).
//       * End transform: The transform that will make the ending desk
//         screenshot visible. In this case it is a transform with translation
//         (edge_padding_width_dp_, 0).
//
//   * The animation always begins such that the starting desk screenshot layer
//     is the one visible on the screen, and the parent (animation layer) always
//     moves in the direction such that the ending desk screenshot becomes
//     visible on the screen.
//   * The children (screenshot layers) are always placed left to right to match
//     desk order. For example, if there are three desks and this class has been
//     instructed to create a screenshot for all three desks, desk 1's
//     screenshot will be on the left, desk 2's screenshot will be in the middle
//     and desk 3's screenshot will be on the right.
//   * Once the animation finishes, Delegate::OnDeskSwitchAnimationFinished() is
//     triggered. The owner of this object can then check that all animators on
//     all roots have finished their animations (by checking
//     animation_finished()), upon which it can delete these animators which
//     will destroy all the screenshot layers and the real screen contents will
//     be visible again.
//
// This cooperative interaction between the animators and their owner
// (DesksController::AbstractDeskSwitchAnimation) is needed for the following
// reasons:
// 1- The new desk is only activated after all starting desk screenshots on all
//    roots have been taken and placed on top of everything (between phase (1)
//    and (2)), so that the effects of desk activation (windows hiding and
//    showing, overview exiting .. etc.) are not visible to the user.
// 2- The animation doesn't start until all ending desk screenshots on all
//    root windows are ready (between phase (2) and (3)). This is needed to
//    synchronize the animations on all displays together (otherwise the
//    animations will lag behind each other).
//
// When this animator is used to implement the remove-active-desk animation
// (which also involves switching desks; from the to-be-removed desk to another
// desk), `for_remove` is set to true in the constructor. The animation is
// slightly tweaked to do the following:
// - Instead of taking a screenshot of the starting desk, we replace it by a
//   black solid color layer, to indicate the desk is being removed.
// - The layer tree of the active-desk container is recreated, and the old
//   layers are detached and animated vertically by
//   `kRemovedDeskWindowYTranslation`.
// - That old layer tree is then translated back down by the same amount while
//   the desks screenshots are animating horizontally.
// This gives the effect that the removed desk windows are jumping from their
// desk to the target desk.
```
## Visual type:
- #custom 


== ./chromium/chromium_290.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/policy_low_level.h#L46-L70

```c
// Defines the memory layout of the policy. This memory is filled by
// LowLevelPolicy object.
< ASCII >
// For example:
//
//  [Service 0] --points to---\
//  [Service 1] --------------|-----\
//   ......                   |     |
//  [Service N]               |     |
//  [data_size]               |     |
//  [Policy Buffer 0] <-------/     |
//  [opcodes of]                    |
//  .......                         |
//  [Policy Buffer 1] <-------------/
//  [opcodes]
//  .......
//  .......
//  [Policy Buffer N]
//  [opcodes]
//  .......
//   <possibly unused space here>
//  .......
//  [opcode string ]
//  [opcode string ]
//  .......
//  [opcode string ]
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_291.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/security_level.h#L38-L80

```c
< ASCII >
// The Token level specifies a set of  security profiles designed to
// provide the bulk of the security of sandbox.
//
//  TokenLevel                 |Restricting   |Deny Only       |Privileges|
//                             |Sids          |Sids            |          |
// ----------------------------|--------------|----------------|----------|
// USER_LOCKDOWN               | Null Sid     | All            | None     |
// ----------------------------|--------------|----------------|----------|
// USER_LIMITED                | Users        | All except:    | Traverse |
//                             | Everyone     | Users          |          |
//                             | RESTRICTED   | Everyone       |          |
//                             |              | Interactive    |          |
// ----------------------------|--------------|----------------|----------|
// USER_INTERACTIVE            | Users        | All except:    | Traverse |
//                             | Everyone     | Users          |          |
//                             | RESTRICTED   | Everyone       |          |
//                             | Owner        | Interactive    |          |
//                             |              | Local          |          |
//                             |              | Authent-users  |          |
//                             |              | User           |          |
// ----------------------------|--------------|----------------|----------|
// USER_RESTRICTED_NON_ADMIN   | Users        | All except:    | Traverse |
//                             | Everyone     | Users          |          |
//                             | Interactive  | Everyone       |          |
//                             | Local        | Interactive    |          |
//                             | Authent-users| Local          |          |
//                             | User         | Authent-users  |          |
//                             |              | User           |          |
// ----------------------------|--------------|----------------|----------|
// USER_RESTRICTED_SAME_ACCESS | All          | None           | All      |
// ----------------------------|--------------|----------------|----------|
// USER_UNPROTECTED            | None         | None           | All      |
// ----------------------------|--------------|----------------|----------|
//
< ASCII >
// The above restrictions are actually a transformation that is applied to
// the existing broker process token. The resulting token that will be
// applied to the target process depends both on the token level selected
// and on the broker token itself.
//
// The LOCKDOWN level is designed to allow access to almost nothing that has
// security associated with and they are the recommended levels to run sandboxed
// code specially if there is a chance that the broker is process might be
// started by a user that belongs to the Admins or power users groups.
```
## Visual type:
- #table 


== ./chromium/chromium_292.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/security_level.h#L91-L125

```c
// The Job level specifies a set of decreasing security profiles for the
// Job object that the target process will be placed into.
< ASCII >
// This table summarizes the security associated with each level:
//
//  JobLevel        |General                            |Quota               |
//                  |restrictions                       |restrictions        |
// -----------------|---------------------------------- |--------------------|
// kNone            | No job is assigned to the         | None               |
//                  | sandboxed process.                |                    |
// -----------------|---------------------------------- |--------------------|
// kUnprotected     | None                              | *Kill on Job close.|
// -----------------|---------------------------------- |--------------------|
// kInteractive     | *Forbid system-wide changes using |                    |
//                  |  SystemParametersInfo().          | *Kill on Job close.|
//                  | *Forbid the creation/switch of    |                    |
//                  |  Desktops.                        |                    |
//                  | *Forbids calls to ExitWindows().  |                    |
// -----------------|---------------------------------- |--------------------|
// kLimitedUser     | Same as kInteractive plus:        | *One active process|
//                  | *Forbid changes to the display    |  limit.            |
//                  |  settings.                        | *Kill on Job close.|
// -----------------|---------------------------------- |--------------------|
// kLockdown        | Same as kLimitedUser plus:        | *One active process|
//                  | * No read/write to the clipboard. |  limit.            |
//                  | * No access to User Handles that  | *Kill on Job close.|
//                  |   belong to other processes.      | *Kill on unhandled |
//                  | * Forbid message broadcasts.      |  exception.        |
//                  | * Forbid setting global hooks.    |                    |
//                  | * No access to the global atoms   |                    |
//                  |   table.                          |                    |
// -----------------|-----------------------------------|--------------------|
//
< ASCII >
// In the context of the above table, 'user handles' refers to the handles of
// windows, bitmaps, menus, etc. Files, treads and registry handles are kernel
// handles and are not affected by the job level settings.
```
## Visual type:
- #table


== ./chromium/chromium_293.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/sandbox/win/src/sharedmem_ipc_client.h#L16-L56

```c
// IPC transport implementation that uses shared memory.
// This is the client side
//
// The shared memory is divided on blocks called channels, and potentially
// it can perform as many concurrent IPC calls as channels. The IPC over
// each channel is strictly synchronous for the client.
//
// Each channel as a channel control section associated with. Each control
// section has two kernel events (known as ping and pong) and a integer
// variable that maintains a state
//
< ASCII >
// this is the state diagram of a channel:
//
//                   locked                in service
//     kFreeChannel---------->BusyChannel-------------->kAckChannel
//          ^                                                 |
//          |_________________________________________________|
//                             answer ready
< ASCII >
//
// The protocol is as follows:
//   1) client finds a free channel: state = kFreeChannel
//   2) does an atomic compare-and-swap, now state = BusyChannel
//   3) client writes the data into the channel buffer
//   4) client signals the ping event and waits (blocks) on the pong event
//   5) eventually the server signals the pong event
//   6) the client awakes and reads the answer from the same channel
//   7) the client updates its InOut parameters with the new data from the
//      shared memory section.
//   8) the client atomically sets the state = kFreeChannel
//
< ASCII >
//  In the shared memory the layout is as follows:
//
//    [ channel count    ]
//    [ channel control 0]
//    [ channel control 1]
//    [ channel control N]
//    [ channel buffer 0 ] 1024 bytes
//    [ channel buffer 1 ] 1024 bytes
//    [ channel buffer N ] 1024 bytes
//
// By default each channel buffer is 1024 bytes
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_294.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/services/audio/output_controller.h#L30-L57

```c
// An OutputController controls an AudioOutputStream and provides data to this
// output stream. It executes audio operations like play, pause, stop, etc. on
// the audio manager thread, while the audio data flow occurs on the platform's
// realtime audio thread.
//
< ASCII >
// Here is a state transition diagram for the OutputController:
//
//   *[ Empty ]  -->  [ Created ]  -->  [ Playing ]  -------.
//        |                |               |    ^           |
//        |                |               |    |           |
//        |                |               |    |           v
//        |                |               |    `-----  [ Paused ]
//        |                |               |                |
//        |                v               v                |
//        `----------->  [      Closed       ]  <-----------'
//
// * Initial state
< ASCII >
//
// At any time after reaching the Created state but before Closed, the
// OutputController may be notified of a device change via OnDeviceChange().  As
// the OnDeviceChange() is processed, state transitions will occur, ultimately
// ending up in an equivalent pre-call state.  E.g., if the state was Paused,
// the new state will be Created, since these states are all functionally
// equivalent and require a Play() call to continue to the next state.
//
// The AudioOutputStream can request data from the OutputController via the
// AudioSourceCallback interface. OutputController uses the SyncReader passed to
// it via construction to synchronously fulfill this read request.
```
## Visual type:
- #state-machine


== ./chromium/chromium_295.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/services/device/usb/webusb_descriptors.cc#L208-L217

```c
// Parses a WebUSB URL Descriptor:
// https://wicg.github.io/webusb/#url-descriptor
//
< ASCII >
//  0                   1                   2                   3
//  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
// +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
// |     length    |     type      |    prefix     |    data[0]    |
// +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
// |     data[1]   |      ...
// +-+-+-+-+-+-+-+-+-+-+-+------
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_296.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/services/proxy_resolver/proxy_resolver_v8.cc#L38-L84

```c
// Notes on the javascript environment:
//
// For the majority of the PAC utility functions, we use the same code
// as Firefox. See the javascript library that pac_js_library.h pulls in.
//
// In addition, we implement a subset of Microsoft's extensions to PAC.
// - myIpAddressEx()
// - dnsResolveEx()
// - isResolvableEx()
// - isInNetEx()
// - sortIpAddressList()
//
// It is worth noting that the original PAC specification does not describe
// the return values on failure. Consequently, there are compatibility
// differences between browsers on what to return on failure, which are
// illustrated below:
//
< ASCII >
// --------------------+-------------+-------------------+--------------
//                     | Firefox3    | InternetExplorer8 |  --> Us <---
// --------------------+-------------+-------------------+--------------
// myIpAddress()       | "127.0.0.1" |  ???              |  "127.0.0.1"
// dnsResolve()        | null        |  false            |  null
// myIpAddressEx()     | N/A         |  ""               |  ""
// sortIpAddressList() | N/A         |  false            |  false
// dnsResolveEx()      | N/A         |  ""               |  ""
// isInNetEx()         | N/A         |  false            |  false
// --------------------+-------------+-------------------+--------------
//
< ASCII >
// TODO(eroman): The cell above reading ??? means I didn't test it.
//
// Another difference is in how dnsResolve() and myIpAddress() are
// implemented -- whether they should restrict to IPv4 results, or
// include both IPv4 and IPv6. The following table illustrates the
// differences:
//
< ASCII >
// --------------------+-------------+-------------------+--------------
//                     | Firefox3    | InternetExplorer8 |  --> Us <---
// --------------------+-------------+-------------------+--------------
// myIpAddress()       | IPv4/IPv6   |  IPv4             |  IPv4/IPv6
// dnsResolve()        | IPv4/IPv6   |  IPv4             |  IPv4
// isResolvable()      | IPv4/IPv6   |  IPv4             |  IPv4
// myIpAddressEx()     | N/A         |  IPv4/IPv6        |  IPv4/IPv6
// dnsResolveEx()      | N/A         |  IPv4/IPv6        |  IPv4/IPv6
// sortIpAddressList() | N/A         |  IPv4/IPv6        |  IPv4/IPv6
// isResolvableEx()    | N/A         |  IPv4/IPv6        |  IPv4/IPv6
// isInNetEx()         | N/A         |  IPv4/IPv6        |  IPv4/IPv6
// -----------------+-------------+-------------------+--------------
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_297.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/services/tracing/public/cpp/perfetto/perfetto_tracing_backend.h#L24-L39

```c
// The Perfetto tracing backend mediates between the Perfetto client library and
// the mojo-based tracing service. It allows any process to emit trace data
// through Perfetto and privileged processes (i.e., the browser) to start
// tracing sessions and read back the resulting data.
< ASCII >
//
//      Perfetto         :   Tracing backend     :    Tracing service
//                       :                       :
//                       :                      mojo
//                calls  : .------------------.  :  .--------------.
//             .---------->| ConsumerEndpoint |<--->| ConsumerHost |
//  .--------------.     : `------------------'  :  `--------------'
//  | TracingMuxer |     :                       :
//  `--------------'     : .------------------.  :  .--------------.
//             `---------->| ProducerEndpoint |<--->| ProducerHost |
//                       : `------------------'  :  `--------------'
//                       :                       :
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_298.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/android_opengl/etc1/etc1.cpp#L31-L120

```c

/* From http://www.khronos.org/registry/gles/extensions/OES/OES_compressed_ETC1_RGB8_texture.txt

 The number of bits that represent a 4x4 texel block is 64 bits if
 <internalformat> is given by ETC1_RGB8_OES.

 The data for a block is a number of bytes,

 {q0, q1, q2, q3, q4, q5, q6, q7}

 where byte q0 is located at the lowest memory address and q7 at
 the highest. The 64 bits specifying the block is then represented
 by the following 64 bit integer:

 int64bit = 256*(256*(256*(256*(256*(256*(256*q0+q1)+q2)+q3)+q4)+q5)+q6)+q7;

 ETC1_RGB8_OES:

< ASCII >
 a) bit layout in bits 63 through 32 if diffbit = 0

 63 62 61 60 59 58 57 56 55 54 53 52 51 50 49 48
 -----------------------------------------------
 | base col1 | base col2 | base col1 | base col2 |
 | R1 (4bits)| R2 (4bits)| G1 (4bits)| G2 (4bits)|
 -----------------------------------------------

 47 46 45 44 43 42 41 40 39 38 37 36 35 34  33  32
 ---------------------------------------------------
 | base col1 | base col2 | table  | table  |diff|flip|
 | B1 (4bits)| B2 (4bits)| cw 1   | cw 2   |bit |bit |
 ---------------------------------------------------


 b) bit layout in bits 63 through 32 if diffbit = 1

 63 62 61 60 59 58 57 56 55 54 53 52 51 50 49 48
 -----------------------------------------------
 | base col1    | dcol 2 | base col1    | dcol 2 |
 | R1' (5 bits) | dR2    | G1' (5 bits) | dG2    |
 -----------------------------------------------

 47 46 45 44 43 42 41 40 39 38 37 36 35 34  33  32
 ---------------------------------------------------
 | base col 1   | dcol 2 | table  | table  |diff|flip|
 | B1' (5 bits) | dB2    | cw 1   | cw 2   |bit |bit |
 ---------------------------------------------------


 c) bit layout in bits 31 through 0 (in both cases)

 31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16
 -----------------------------------------------
 |       most significant pixel index bits       |
 | p| o| n| m| l| k| j| i| h| g| f| e| d| c| b| a|
 -----------------------------------------------

 15 14 13 12 11 10  9  8  7  6  5  4  3   2   1  0
 --------------------------------------------------
 |         least significant pixel index bits       |
 | p| o| n| m| l| k| j| i| h| g| f| e| d| c | b | a |
 --------------------------------------------------


 Add table 3.17.2: Intensity modifier sets for ETC1 compressed textures:

 table codeword                modifier table
 ------------------        ----------------------
 0                     -8  -2  2   8
 1                    -17  -5  5  17
 2                    -29  -9  9  29
 3                    -42 -13 13  42
 4                    -60 -18 18  60
 5                    -80 -24 24  80
 6                   -106 -33 33 106
 7                   -183 -47 47 183


 Add table 3.17.3 Mapping from pixel index values to modifier values for
 ETC1 compressed textures:

 pixel index value
 ---------------
 msb     lsb           resulting modifier value
 -----   -----          -------------------------
 1       1            -b (large negative value)
 1       0            -a (small negative value)
 0       0             a (small positive value)
 0       1             b (large positive value)

< ASCII >

 */
```
## Visual type:
- #memory-layout


== ./chromium/chromium_299.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/perf_tests/speedometer/resources/todomvc/architecture-examples/angularjs/node_modules/angular/angular.js#L23814-L23825

```c
// See valid URLs in RFC3987 (http://tools.ietf.org/html/rfc3987)
// Note: We are being more lenient, because browsers are too.
//   1. Scheme
//   2. Slashes
//   3. Username
//   4. Password
//   5. Hostname
//   6. Port
//   7. Path
//   8. Query
//   9. Fragment
< ASCII >
//                 1111111111111111 222   333333    44444        55555555555555555555555     666     77777777     8888888     999
< ASCII >
```
## Visual type:
- 


== ./chromium/chromium_3.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/accessibility/sticky_keys/sticky_keys_controller.h#L132-L160

```c
// StickyKeysHandler handles key event and controls sticky keysfor specific
// modifier keys. If monitored keyboard events are received, StickyKeysHandler
// changes internal state. If non modifier keyboard events or mouse events are
// received, StickyKeysHandler will append modifier based on internal state.
// For other events, StickyKeysHandler does nothing.
//
// The DISABLED state is default state and any incoming non modifier keyboard
// events will not be modified. The ENABLED state is one shot modification
// state. Only next keyboard event will be modified. After that, internal state
// will be back to DISABLED state with sending modifier keyup event. In the case
// of LOCKED state, all incomming keyboard events will be modified. The LOCKED
// state will be back to DISABLED state by next monitoring modifier key.
//
< ASCII >
// The detailed state flow as follows:
//                                     Current state
//                  |   DISABLED    |    ENABLED     |    LOCKED   |
// ----------------------------------------------------------------|
// Modifier KeyDown |   noop        |    noop(*)     |    noop(*)  |
// Modifier KeyUp   | To ENABLED(*) | To LOCKED(*)   | To DISABLED |
// Normal KeyDown   |   noop        | To DISABLED(#) |    noop(#)  |
// Normal KeyUp     |   noop        |    noop        |    noop(#)  |
// Other KeyUp/Down |   noop        |    noop        |    noop     |
// Mouse Press      |   noop        |    noop(#)     |    noop(#)  |
// Mouse Release    |   noop        | To DISABLED(#) |    noop(#)  |
// Mouse Wheel      |   noop        | To DISABLED(#) |    noop(#)  |
// Other Mouse Event|   noop        |    noop        |    noop     |
//
// Here, (*) means key event will be consumed by StickyKeys, and (#) means event
// is modified.
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_30.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/desks/templates/saved_desk_icon_container.h#L26-L40

```c
// This class determines which app icons/favicons to show for a saved desk and
// creates the according SavedDeskIconView's. The last SavedDeskIconView in the
// layout is used for storing the overflow count of icons. Not every view in the
// container is visible.
< ASCII >
//   _______________________________________________________________________
//   |  _________  _________   _________________   _________   _________   |
//   |  |       |  |       |   |       |       |   |       |   |       |   |
//   |  |   I   |  |   I   |   |   I      + N  |   |   I   |   |  + N  |   |
//   |  |_______|  |_______|   |_______|_______|   |_______|   |_______|   |
//   |_____________________________________________________________________|
< ASCII >
//
// If there are multiple apps associated with a certain icon, the icon is drawn
// once with a +N label attached, up to +99. If there are too many icons to be
// displayed within the given width, we draw as many and a label at the end that
// says +N, up to +99.
```
## Visual type:
- #gui


== ./chromium/chromium_300.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/perf_tests/speedometer/resources/todomvc/architecture-examples/angularjs/node_modules/angular/angular.js#L26439-L26621

```c
/**
 * @ngdoc directive
 * @name ngClass
 * @restrict AC
 *
 * @description
 * The `ngClass` directive allows you to dynamically set CSS classes on an HTML element by databinding
 * an expression that represents all classes to be added.
 *
 * The directive operates in three different ways, depending on which of three types the expression
 * evaluates to:
 *
 * 1. If the expression evaluates to a string, the string should be one or more space-delimited class
 * names.
 *
 * 2. If the expression evaluates to an object, then for each key-value pair of the
 * object with a truthy value the corresponding key is used as a class name.
 *
 * 3. If the expression evaluates to an array, each element of the array should either be a string as in
 * type 1 or an object as in type 2. This means that you can mix strings and objects together in an array
 * to give you more control over what CSS classes appear. See the code below for an example of this.
 *
 *
 * The directive won't add duplicate classes if a particular class was already set.
 *
 * When the expression changes, the previously added classes are removed and only then are the
 * new classes added.
 *
 * @knownIssue
 * You should not use {@link guide/interpolation interpolation} in the value of the `class`
 * attribute, when using the `ngClass` directive on the same element.
 * See {@link guide/interpolation#known-issues here} for more info.
< ASCII >
 *
 * @animations
 * | Animation                        | Occurs                              |
 * |----------------------------------|-------------------------------------|
 * | {@link ng.$animate#addClass addClass}       | just before the class is applied to the element   |
 * | {@link ng.$animate#removeClass removeClass} | just before the class is removed from the element |
< ASCII >
 *
 * @element ANY
 * @param {expression} ngClass {@link guide/expression Expression} to eval. The result
 *   of the evaluation can be a string representing space delimited class
 *   names, an array, or a map of class names to boolean values. In the case of a map, the
 *   names of the properties whose values are truthy will be added as css classes to the
 *   element.
 *
 * @example Example that demonstrates basic bindings via ngClass directive.
   <example name="ng-class">
     <file name="index.html">
       <p ng-class="{strike: deleted, bold: important, 'has-error': error}">Map Syntax Example</p>
       <label>
          <input type="checkbox" ng-model="deleted">
          deleted (apply "strike" class)
       </label><br>
       <label>
          <input type="checkbox" ng-model="important">
          important (apply "bold" class)
       </label><br>
       <label>
          <input type="checkbox" ng-model="error">
          error (apply "has-error" class)
       </label>
       <hr>
       <p ng-class="style">Using String Syntax</p>
       <input type="text" ng-model="style"
              placeholder="Type: bold strike red" aria-label="Type: bold strike red">
       <hr>
       <p ng-class="[style1, style2, style3]">Using Array Syntax</p>
       <input ng-model="style1"
              placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red"><br>
       <input ng-model="style2"
              placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red 2"><br>
       <input ng-model="style3"
              placeholder="Type: bold, strike or red" aria-label="Type: bold, strike or red 3"><br>
       <hr>
       <p ng-class="[style4, {orange: warning}]">Using Array and Map Syntax</p>
       <input ng-model="style4" placeholder="Type: bold, strike" aria-label="Type: bold, strike"><br>
       <label><input type="checkbox" ng-model="warning"> warning (apply "orange" class)</label>
     </file>
     <file name="style.css">
       .strike {
           text-decoration: line-through;
       }
       .bold {
           font-weight: bold;
       }
       .red {
           color: red;
       }
       .has-error {
           color: red;
           background-color: yellow;
       }
       .orange {
           color: orange;
       }
     </file>
     <file name="protractor.js" type="protractor">
       var ps = element.all(by.css('p'));

       it('should let you toggle the class', function() {

         expect(ps.first().getAttribute('class')).not.toMatch(/bold/);
         expect(ps.first().getAttribute('class')).not.toMatch(/has-error/);

         element(by.model('important')).click();
         expect(ps.first().getAttribute('class')).toMatch(/bold/);

         element(by.model('error')).click();
         expect(ps.first().getAttribute('class')).toMatch(/has-error/);
       });

       it('should let you toggle string example', function() {
         expect(ps.get(1).getAttribute('class')).toBe('');
         element(by.model('style')).clear();
         element(by.model('style')).sendKeys('red');
         expect(ps.get(1).getAttribute('class')).toBe('red');
       });

       it('array example should have 3 classes', function() {
         expect(ps.get(2).getAttribute('class')).toBe('');
         element(by.model('style1')).sendKeys('bold');
         element(by.model('style2')).sendKeys('strike');
         element(by.model('style3')).sendKeys('red');
         expect(ps.get(2).getAttribute('class')).toBe('bold strike red');
       });

       it('array with map example should have 2 classes', function() {
         expect(ps.last().getAttribute('class')).toBe('');
         element(by.model('style4')).sendKeys('bold');
         element(by.model('warning')).click();
         expect(ps.last().getAttribute('class')).toBe('bold orange');
       });
     </file>
   </example>

   ## Animations

   The example below demonstrates how to perform animations using ngClass.

   <example module="ngAnimate" deps="angular-animate.js" animations="true" name="ng-class">
     <file name="index.html">
      <input id="setbtn" type="button" value="set" ng-click="myVar='my-class'">
      <input id="clearbtn" type="button" value="clear" ng-click="myVar=''">
      <br>
      <span class="base-class" ng-class="myVar">Sample Text</span>
     </file>
     <file name="style.css">
       .base-class {
         transition:all cubic-bezier(0.250, 0.460, 0.450, 0.940) 0.5s;
       }

       .base-class.my-class {
         color: red;
         font-size:3em;
       }
     </file>
     <file name="protractor.js" type="protractor">
       it('should check ng-class', function() {
         expect(element(by.css('.base-class')).getAttribute('class')).not.
           toMatch(/my-class/);

         element(by.id('setbtn')).click();

         expect(element(by.css('.base-class')).getAttribute('class')).
           toMatch(/my-class/);

         element(by.id('clearbtn')).click();

         expect(element(by.css('.base-class')).getAttribute('class')).not.
           toMatch(/my-class/);
       });
     </file>
   </example>


   ## ngClass and pre-existing CSS3 Transitions/Animations
   The ngClass directive still supports CSS3 Transitions/Animations even if they do not follow the ngAnimate CSS naming structure.
   Upon animation ngAnimate will apply supplementary CSS classes to track the start and end of an animation, but this will not hinder
   any pre-existing CSS transitions already on the element. To get an idea of what happens during a class-based animation, be sure
   to view the step by step details of {@link $animate#addClass $animate.addClass} and
   {@link $animate#removeClass $animate.removeClass}.
 */
```
## Visual type:
- #table


== ./chromium/chromium_301.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/perf_tests/speedometer/resources/todomvc/labs/architecture-examples/react/bower_components/react/react.js#L3899-L3924

```c
/**
 * `ReactCompositeComponent` maintains an auxiliary life cycle state in
 * `this._compositeLifeCycleState` (which can be null).
 *
 * This is different from the life cycle state maintained by `ReactComponent` in
 * `this._lifeCycleState`. The following diagram shows how the states overlap in
 * time. There are times when the CompositeLifeCycle is null - at those times it
 * is only meaningful to look at ComponentLifeCycle alone.
 *
 * Top Row: ReactComponent.ComponentLifeCycle
 * Low Row: ReactComponent.CompositeLifeCycle
< ASCII >
 *
 * +-------+------------------------------------------------------+--------+
 * |  UN   |                    MOUNTED                           |   UN   |
 * |MOUNTED|                                                      | MOUNTED|
 * +-------+------------------------------------------------------+--------+
 * |       ^--------+   +------+   +------+   +------+   +--------^        |
 * |       |        |   |      |   |      |   |      |   |        |        |
 * |    0--|MOUNTING|-0-|RECEIV|-0-|RECEIV|-0-|RECEIV|-0-|   UN   |--->0   |
 * |       |        |   |PROPS |   | PROPS|   | STATE|   |MOUNTING|        |
 * |       |        |   |      |   |      |   |      |   |        |        |
 * |       |        |   |      |   |      |   |      |   |        |        |
 * |       +--------+   +------+   +------+   +------+   +--------+        |
 * |       |                                                      |        |
 * +-------+------------------------------------------------------+--------+
< ASCII >
 */
```
## Visual type:
- #custom


== ./chromium/chromium_302.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/perf_tests/speedometer/resources/todomvc/labs/architecture-examples/react/bower_components/react/react.js#L9430-L9493

```c
/**
 * `Transaction` creates a black box that is able to wrap any method such that
 * certain invariants are maintained before and after the method is invoked
 * (Even if an exception is thrown while invoking the wrapped method). Whoever
 * instantiates a transaction can provide enforcers of the invariants at
 * creation time. The `Transaction` class itself will supply one additional
 * automatic invariant for you - the invariant that any transaction instance
 * should not be ran while it is already being ran. You would typically create a
 * single instance of a `Transaction` for reuse multiple times, that potentially
 * is used to wrap several different methods. Wrappers are extremely simple -
 * they only require implementing two methods.
 *
< ASCII >
 * <pre>
 *                       wrappers (injected at creation time)
 *                                      +        +
 *                                      |        |
 *                    +-----------------|--------|--------------+
 *                    |                 v        |              |
 *                    |      +---------------+   |              |
 *                    |   +--|    wrapper1   |---|----+         |
 *                    |   |  +---------------+   v    |         |
 *                    |   |          +-------------+  |         |
 *                    |   |     +----|   wrapper2  |--------+   |
 *                    |   |     |    +-------------+  |     |   |
 *                    |   |     |                     |     |   |
 *                    |   v     v                     v     v   | wrapper
 *                    | +---+ +---+   +---------+   +---+ +---+ | invariants
 * perform(anyMethod) | |   | |   |   |         |   |   | |   | | maintained
 * +----------------->|-|---|-|---|-->|anyMethod|---|---|-|---|-|-------->
 *                    | |   | |   |   |         |   |   | |   | |
 *                    | |   | |   |   |         |   |   | |   | |
 *                    | |   | |   |   |         |   |   | |   | |
 *                    | +---+ +---+   +---------+   +---+ +---+ |
 *                    |  initialize                    close    |
 *                    +-----------------------------------------+
 * </pre>
< ASCII >
 *
 * Bonus:
 * - Reports timing metrics by method name and wrapper index.
 *
 * Use cases:
 * - Preserving the input selection ranges before/after reconciliation.
 *   Restoring selection even in the event of an unexpected error.
 * - Deactivating events while rearranging the DOM, preventing blurs/focuses,
 *   while guaranteeing that afterwards, the event system is reactivated.
 * - Flushing a queue of collected DOM mutations to the main UI thread after a
 *   reconciliation takes place in a worker thread.
 * - Invoking any collected `componentDidRender` callbacks after rendering new
 *   content.
 * - (Future use case): Wrapping particular flushes of the `ReactWorker` queue
 *   to preserve the `scrollTop` (an automatic scroll aware DOM).
 * - (Future use case): Layout calculations before and after DOM upates.
 *
 * Transactional plugin API:
 * - A module that has an `initialize` method that returns any precomputation.
 * - and a `close` method that accepts the precomputation. `close` is invoked
 *   when the wrapped process is completed, or has failed.
 *
 * @param {Array<TransactionalWrapper>} transactionWrapper Wrapper modules
 * that implement `initialize` and `close`.
 * @return {Transaction} Single transaction for reuse in thread.
 *
 * @class Transaction
 */
```
## Visual type:
- #custom


== ./chromium/chromium_303.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/public/common/privacy_budget/identifiable_surface.h#L20-L57

```c
// An identifiable surface.
//
// See also: ../../../../../docs/privacy_budget/good_identifiable_surface.md
//
// This class intends to be a lightweight wrapper over a simple 64-bit integer.
// It exhibits the following characteristics:
//
//   * All methods are constexpr.
//   * Immutable.
//   * Efficient enough to pass by value.
//
// Internally, an identifiable surface is represented as a 64-bit unsigned
// integer that can be used as the metric hash for reporting metrics via UKM.
//
// The least-significant |kTypeBits| of the value is used to store
// a IdentifiableSurface::Type value. The remainder stores the 56
// least-significant bits of an `IdentifiableToken` as illustrated below:
< ASCII >
//              ✂
//    ┌─────────┊────────────────────────────────────────┐ ┌──────────┐
//    │(discard)✂           IdentifiableToken            │ │   Type   │
//    └─────────┊───────────────────┬────────────────────┘ └────┬─────┘
// Bit 64       ┊55                 ┊                   0   7   ┊    0
//              ✂                   ↓                           ↓
//              ┌────────────────────────────────────────┬──────────┐
//              │                                        │          │
//              └────────────────────────────────────────┴──────────┘
//           Bit 64                                     8 7        0
//              │←────────────── IdentifiableSurface ──────────────→│
//
< ASCII >
// Only the lower 56 bits of `IdentifiableToken` contribute to an
// `IdentifiableSurface`.
//
// See descriptions for the `Type` enum values for details on how the
// `IdentifiableToken` is generated for each type. The descriptions use the
// following notation to indicate how the value is recorded:
//
//     IdentifiableSurface = { IdentifiableToken value, Type value }
//     Value = [description of how the value is constructed]
```
## Visual type:
- #memory-layout


== ./chromium/chromium_304.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/css/check_pseudo_has_cache_scope.h#L20-L120

```c
// To determine whether a :has() pseudo class matches an element or not, we need
// to check the :has() argument selector on the descendants, next siblings or
// next sibling descendants. While checking the :has() argument selector in
// reversed DOM tree traversal order, we can get the :has() pseudo class
// checking result on the elements in the subtree. By caching these results, we
// can prevent unnecessary :has() pseudo class checking operations. (Please
// refer the comments of CheckPseudoHasArgumentTraversalIterator)
//
// Caching the results on all elements in the subtree is a very memory consuming
// approach. To prevent the large and inefficient cache memory consumption,
// ElementCheckPseudoHasResultMap stores following flags for an element.
//
// - flag1 (Checked) : Indicates that the :has() pseudo class was already
//     checked on the element.
//
// - flag2 (Matched) : Indicates that the :has() pseudo class was already
//     checked on the element and it matched.
//
// - flag3 (AllDescendantsOrNextSiblingsChecked) : Indicates that all the
//     not-cached descendant elements (or all the not-cached next sibling
//     elements) of the element were already checked as not-matched.
//     When the :has() argument checking traversal is stopped, this flag is set
//     on the stopped element and the next sibling element of its ancestors to
//     mark already traversed subtree.
//
// - flag4 (SomeChildrenChecked) : Indicates that some children of the element
//     were already checked. This flag is set on the parent of the
//     kCheckPseudoHasResultAllDescendantsOrNextSiblingsChecked element.
//     If the parent of an not-cached element has this flag set, we can
//     determine whether the element is 'already checked as not-matched' or
//     'not yet checked' by checking the AllDescendantsOrNextSiblingsChecked
//     flag of its previous sibling elements.
//
// Example)  subject.match(':has(.a)')
//  - DOM
//      <div id=subject>
//        <div id=d1>
//          <div id=d11></div>
//        </div>
//        <div id=d2>
//          <div id=d21></div>
//          <div id=d22 class=a>
//            <div id=d221></div>
//          </div>
//          <div id=d23></div>
//        </div>
//        <div id=d3>
//          <div id=d31></div>
//        </div>
//        <div id=d4></div>
//      </div>
//
< ASCII >
//  - Cache
//      |    id    |  flag1  |  flag2  |  flag3  |  flag4  | actual state |
//      | -------- | ------- | ------- | ------- | ------- | ------------ |
//      |  subject |    1    |    1    |    0    |    1    |    matched   |
//      |    d1    |    -    |    -    |    -    |    -    |  not checked |
//      |    d11   |    -    |    -    |    -    |    -    |  not checked |
//      |    d2    |    1    |    1    |    0    |    1    |    matched   |
//      |    d21   |    -    |    -    |    -    |    -    |  not checked |
//      |    d22   |    1    |    0    |    1    |    0    |  not matched |
//      |    d221  |    -    |    -    |    -    |    -    |  not matched |
//      |    d23   |    -    |    -    |    -    |    -    |  not matched |
//      |    d3    |    1    |    0    |    1    |    0    |  not matched |
//      |    d31   |    -    |    -    |    -    |    -    |  not matched |
//      |    d4    |    -    |    -    |    -    |    -    |  not matched |
< ASCII >
//
//  - How to check elements that are not in the cache.
//    - d1 :   1. Check parent(subject). Parent is 'SomeChildrenChecked'.
//             2. Traverse to previous siblings to find an element with the
//                flag3 (AllDescendantsOrNextSiblingsChecked).
//             >> not checked because no previous sibling with the flag set.
//    - d11 :  1. Check parent(d1). Parent is not cached.
//             2. Traverse to the parent(p1).
//             3. Check parent(subject). Parent is 'SomeChildrenChecked'.
//             4. Traverse to previous siblings to find an element with the
//                flag3 (AllDescendantsOrNextSiblingsChecked).
//             >> not checked because no previous sibling with the flag set.
//    - d21 :  1. Check parent(d2). Parent is 'SomeChildrenChecked'.
//             2. Traverse to previous siblings to find an element with the
//                flag3 (AllDescendantsOrNextSiblingsChecked).
//             >> not checked because no previous sibling with the flag set.
//    - d221 : 1. Check parent(d2).
//                Parent is 'AllDescendantsOrNextSiblingsChecked'.
//             >> not matched
//    - d23 :  1. Check parent(d2). Parent is 'SomeChildrenChecked'.
//             2. Traverse to previous siblings to find an element with the
//                flag3 (AllDescendantsOrNextSiblingsChecked).
//             >> not matched because d22 is
//                'AllDescendantsOrNextSiblingsChecked'.
//    - d31 :  1. Check parent(d3).
//                Parent is 'AllDescendantsOrNextSiblingsChecked'.
//             >> not matched
//    - d4 :   1. Check parent(subject). Parent is 'SomeChildrenChecked'.
//             2. Traverse to previous siblings to find an element with the
//                flag3 (AllDescendantsOrNextSiblingsChecked).
//             >> not matched because d3 is
//                'AllDescendantsOrNextSiblingsChecked'.
//
// Please refer the check_pseudo_has_cache_scope_context_test.cc for other
// cases.
```
## Visual type:
- #table 


== ./chromium/chromium_305.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/fragment_directive/text_fragment_anchor.h#L28-L76

```c
// TextFragmentAnchor is the coordinator class for applying text directives
// from the URL (also known as "scroll-to-text") to a document. This class'
// purpose is to integrate with Blink's loading and lifecycle states. The
// actual logic of performing the text search and applying highlights is
// delegated out to the core annotation API.
//
// A frame will try to create a TextFragmentAnchor when parsing in a document
// completes. If the URL has a valid text directive an instance of
// TextFragmentAnchor will be created and stored on the LocalFrameView.
//
// The anchor performs its operations via the InvokeSelector method which is
// invoked repeatedly, each time layout finishes in the document. Thus, the
// anchor is guaranteed that layout is clean in InvokeSelector; however,
// end-of-layout is a script-forbidden section so no actions that can result in
// script being run can be invoked from there. Scriptable actions will instead
// cause a BeginMainFrame to be scheduled and run in that frame before the
// lifecycle, where script is allowed.
//
< ASCII >
// TextFragmentAnchor is a state machine that transitions state via
// InvokeSelector (and some external events). Here are the state transitions:
//
//           ┌──────┐
//           │   ┌──┴────────────────────────┐
//           └───►       kSearching          ├────────────┐
//               └─────────────┬─────────────┘            │
//                             │                          │
//         ┌─────┬─────────────▼─────────────┐            │
//         └────►│ kBeforeMatchEventQueued   │            │
//               └─────────────┬─────────────┘            │
//                             │                          │
//               ┌─────────────▼─────────────┐            │
//               │  kBeforeMatchEventFired   ├────────────┤
//               └─────────────┬─────────────┘            │
//                             │                          │
//         ┌─────┬─────────────▼─────────────┐            │
//         └────►│ kEffectsAppliedKeepInView │            │
//               └─────────────┬─────────────┘            │
//                             │                          │
//               ┌------------─▼-------------┐            │
//         ┌─────┤     [[SearchFinished]]    |◄───────────┘
//         │     └-------------┬-------------┘
//         │                   │
//         │     ┌─────────────▼─────────────┬─────┐
//         │     │    kScriptableActions     │◄────┘
//         │     └───────────────────────────┘
//         │                   │
//         │     ┌─────────────▼─────────────┐
//         └─────►           kDone           │
//               └───────────────────────────┘
< ASCII >
```
## Visual type:
- #state-machine


== ./chromium/chromium_306.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/fragment_directive/text_fragment_selector_generator_test.cc#L977-L989

```c
< ASCII >
// Check the case when parent of selection start is a sibling of a node where
// selection ends.
//   :root
//  /      \
// div      p
//  |       |
//  p      "]Second"
//  |
// "[First..."
< ASCII >
// Where [] indicate selection. In this case, when the selection is adjusted, we
// want to ensure it correctly traverses the tree back to the previous text node
// and not to the <div>(sibling of second <p>).
// See crbug.com/1154308 for more context.
```
## Visual type:
- #tree


== ./chromium/chromium_307.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/fragment_directive/text_fragment_selector_generator_test.cc#L1010-L1023

```c
< ASCII >
// Check the case when parent of selection start is a sibling of a node where
// selection ends.
//    :root
//  /    |     \
// div  div     p
//  |    |       \
//  p   "test"   "]Second"
//  |
//"[First..."
< ASCII >
//
// Where [] indicate selection. In this case, when the selection is adjusted, we
// want to ensure it correctly traverses the tree back to the previous text by
// correctly skipping the invisible div but not skipping the second <p>.
// See crbug.com/1154308 for more context.
```
## Visual type:
- #tree


== ./chromium/chromium_308.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/inspector/inspector_diff.cc#L21-L57

```c
// Implements Myer's Algorithm from
// "An O(ND) Difference Algorithm and Its Variations", particularly the
// linear space refinement mentioned in section 4b.
//
// The differ is input agnostic.
//
// The algorithm works by finding the shortest edit string (SES) in the edit
// graph. The SES describes how to get from a string A of length N to a string
// B of length M via deleting from A and inserting from B.
//
< ASCII >
// Example: A = "abbaa", B = "abab"
//
//                  A
//
//          a   b   b   a    a
//        o---o---o---o---o---o
//      a | \ |   |   | \ | \ |
//        o---o---o---o---o---o
//      b |   | \ | \ |   |   |
//  B     o---o---o---o---o---o
//      a | \ |   |   | \ | \ |
//        o---o---o---o---o---o
//      b |   | \ | \ |   |   |
//        o---o---o---o---o---o
//
< ASCII >
// The edit graph is constructed with the characters from string A on the x-axis
// and the characters from string B on the y-axis. Starting from (0, 0) we can:
//
//     - Move right, which is equivalent to deleting from A
//     - Move downwards, which is equivalent to inserting from B
//     - Move diagonally if the characters from string A and B match, which
//       means no insertion or deletion.
//
// Any path from (0, 0) to (N, M) describes a valid edit string, but we try to
// find the path with the most diagonals, conversely that is the path with the
// least insertions or deletions.
// Note that a path with "D" insertions/deletions is called a D-path.
```
## Visual type:
- #custom


== ./chromium/chromium_309.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/layout_box.h#L154-L227

```c
//     width: 10px;
//     height: 20px;
// }
// </style>
// <div style="overflow:scroll; width: 100px; height: 100px">
//
// The <div>'s content box is not 100x100 as specified in the style but 90x80 as
// we remove the scrollbars from the box.
//
// The presence of scrollbars is determined by the 'overflow' property and can
// be conditioned on having layout overflow (see OverflowModel for more details
// on how we track overflow).
//
// There are 2 types of scrollbars:
// - non-overlay scrollbars take space from the content box.
// - overlay scrollbars don't and just overlay hang off from the border box,
//   potentially overlapping with the padding box's content.
// For more details on scrollbars, see PaintLayerScrollableArea.
//
//
// ***** THE BOX MODEL *****
// The CSS box model is based on a series of nested boxes:
// http://www.w3.org/TR/CSS21/box.html
< ASCII >
//
//       |----------------------------------------------------|
//       |                                                    |
//       |                   margin-top                       |
//       |                                                    |
//       |     |-----------------------------------------|    |
//       |     |                                         |    |
//       |     |             border-top                  |    |
//       |     |                                         |    |
//       |     |    |--------------------------|----|    |    |
//       |     |    |                          |    |    |    |
//       |     |    |       padding-top        |####|    |    |
//       |     |    |                          |####|    |    |
//       |     |    |    |----------------|    |####|    |    |
//       |     |    |    |                |    |    |    |    |
//       | ML  | BL | PL |  content box   | PR | SW | BR | MR |
//       |     |    |    |                |    |    |    |    |
//       |     |    |    |----------------|    |    |    |    |
//       |     |    |                          |    |    |    |
//       |     |    |      padding-bottom      |    |    |    |
//       |     |    |--------------------------|----|    |    |
//       |     |    |                      ####|    |    |    |
//       |     |    |     scrollbar height ####| SC |    |    |
//       |     |    |                      ####|    |    |    |
//       |     |    |-------------------------------|    |    |
//       |     |                                         |    |
//       |     |           border-bottom                 |    |
//       |     |                                         |    |
//       |     |-----------------------------------------|    |
//       |                                                    |
//       |                 margin-bottom                      |
//       |                                                    |
//       |----------------------------------------------------|
< ASCII >
//
// BL = border-left
// BR = border-right
// ML = margin-left
// MR = margin-right
// PL = padding-left
// PR = padding-right
// SC = scroll corner (contains UI for resizing (see the 'resize' property)
// SW = scrollbar width
//
// Note that the vertical scrollbar (if existing) will be on the left in
// right-to-left direction and horizontal writing-mode. The horizontal scrollbar
// (if existing) is always at the bottom.
//
// Those are just the boxes from the CSS model. Extra boxes are tracked by Blink
// (e.g. the overflows). Thus it is paramount to know which box a function is
// manipulating. Also of critical importance is the coordinate system used (see
// the COORDINATE SYSTEMS section in LayoutBoxModelObject).
```
## Visual type:
- #gui


== ./chromium/chromium_31.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/desks/templates/saved_desk_item_view.h#L32-L63

```c
// A view that represents each individual saved desk item in the saved desk
// grid. The view has different shown contents depending on whether the mouse is
// hovered over it.
< ASCII >
//   _________________________          _________________________
//   |  _______________  _   |          |                    _  |
//   |  |_____________| |_|  |          |                   |_| |
//   |  |_______|            |          |     ______________    |
//   |   _________________   |          |     |            |    |
//   |  |                 |  |          |     |____________|    |
//   |  |_________________|  |          |                       |
//   |_______________________|          |_______________________|
//            regular                             hover
< ASCII >
//
// In the regular view we have the:
// `name_view_`: top-left: SavedDeskNameView: It's an editable textbox that
// contains the name of the saved desk.
// `time_view_`: middle-left: Label: A label that lets the user know when the
// saved desk was created.
// `icon_container_view_`: bottom-center: SavedDeskIconContainer: A
// container that houses a couple icons/text that give an indication of which
// apps are part of the saved desk.
// `managed_status_indicator`: top-right: ImageView: A icon that is visible if
// the saved desk was created by an admin.
//
// In the hover view we have the:
// `delete_button_`: top-right: Button: Shows a confirmation for deleting the
// saved desk when clicked.
// `launch_button_`: bottom-center: Button: Launches the apps associated with
// the saved desk when clicked.
//
// The whole view is also a button which does the same thing as `launch_button_`
// when clicked.
```
## Visual type:
- #gui


== ./chromium/chromium_310.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/layout_shift_region_test.cc#L106-L113

```c
< ASCII >
// Creates a region like this:
//   █ █ █
//  ███████
//   █ █ █
//  ███████
//   █ █ █
//  ███████
//   █ █ █
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_311.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/layout_table_cell.h#L49-L90

```c
// LayoutTableCell is used to represent a table cell (display: table-cell).
//
// Because rows are as tall as the tallest cell, cells need to be aligned into
// the enclosing row space. To achieve this, LayoutTableCell introduces the
// concept of 'intrinsic padding'. Those 2 paddings are used to shift the box
// into the row as follows:
< ASCII >
//
//        --------------------------------
//        ^  ^
//        |  |
//        |  |    cell's border before
//        |  |
//        |  v
//        |  ^
//        |  |
//        |  | m_intrinsicPaddingBefore
//        |  |
//        |  v
//        |  -----------------------------
//        |  |                           |
// row    |  |   cell's padding box      |
// height |  |                           |
//        |  -----------------------------
//        |  ^
//        |  |
//        |  | m_intrinsicPaddingAfter
//        |  |
//        |  v
//        |  ^
//        |  |
//        |  |    cell's border after
//        |  |
//        v  v
//        ---------------------------------
< ASCII >
//
// Note that this diagram is not impacted by collapsing or separate borders
// (see 'border-collapse').
// Also there is no margin on table cell (or any internal table element).
//
// LayoutTableCell is positioned with respect to the enclosing
// LayoutTableSection. See callers of
// LayoutTableSection::setLogicalPositionForCell() for when it is placed.
```
## Visual type:
- #gui


== ./chromium/chromium_312.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/exclusions/ng_exclusion_space_test.cc#L205-L219

```c
// Tests the "solid edge" behaviour. When "NEW" is added a new layout
// opportunity shouldn't be created above it.
//
// NOTE: This is the same example given in the code.
< ASCII >
//
//    0 1 2 3 4 5 6 7 8
// 0  +---+  X----X+---+
//    |xxx|  .     |xxx|
// 10 |xxx|  .     |xxx|
//    +---+  .     +---+
// 20        .     .
//      +---+. .+---+
// 30   |xxx|   |NEW|
//      |xxx|   +---+
// 40   +---+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_313.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/exclusions/ng_exclusion_space_test.cc#L257-L270

```c
// Tests that if a new exclusion doesn't overlap with a shelf, we don't add a
// new layout opportunity.
//
// NOTE: This is the same example given in the code.
//
< ASCII >
//    0 1 2 3 4 5 6 7 8
// 0  +---+X------X+---+
//    |xxx|        |xxx|
// 10 |xxx|        |xxx|
//    +---+        +---+
// 20
//                  +---+
// 30               |NEW|
//                  +---+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_314.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/exclusions/ng_exclusion_space_test.cc#L302-L317

```c
// Tests that a shelf is properly inserted between two other shelves.
//
// Additionally tests that an inserted exclusion is correctly inserted in a
// shelve's line_left_edges/line_right_edges list.
//
// NOTE: This is the same example given in the code.
//
< ASCII >
//    0 1 2 3 4 5 6 7 8
// 0  +-----+X----X+---+
//    |xxxxx|      |xxx|
// 10 +-----+      |xxx|
//      +---+      |xxx|
// 20   |NEW|      |xxx|
//    X-----------X|xxx|
// 30              |xxx|
//    X----------------X
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_315.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/table/ng_table_borders.h#L26-L60

```c
// When table has collapsed borders, computing borders for table parts is
// complex, and costly.
// NGTableBorders precomputes collapsed borders. It exposes the API for
// border access. If borders are not collapsed, the API returns regular
// borders.
//
// NGTableBorders methods often take rowspan/colspan arguments.
// Rowspan must never be taller than the section.
// Colspan must never be wider than the table.
// To enforce this, NGTableBorders keeps track of section dimensions,
// and table's last column.
//
// Collapsed borders are stored as edges.
// Edges are stored in a 1D array. The array does not grow if borders are
// not set.
// Each edge represents a cell border.
// Mapping between edges and cells is best understood like this:
// - each cell stores only two edges, left edge, and a top edge.
// - cell's right edge is the left edge of the next cell.
// - cell's bottom edge is the top edge of the cell in the next row.
//
// To store all last row/col edges, an extra imaginary cell is used.
//
// A grid with R rows and C columns has |2 * (R+1) * (C+1)| edges.
// Example; R=1, C=3, 2*(1+1)*(3+1) = 16 edges.
// Edges 7, 9, 11, 13, 14, 15 are unused.
< ASCII >
//
//     1    3   5   7
//   ------------------    <= edges for 3 cols X 1 row
//   |0  |2  |4   |6
//   |   |   |    |
//   ------------------
//   | 8 | 10| 12 | 14
//   |   |   |    |
//   |9  |11 |13  |15
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_316.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/table/ng_table_layout_algorithm.cc#L885-L896

```c
< ASCII >
// Generated fragment structure
// +---- table wrapper fragment ----+
// |     top caption fragments      |
// |     table border/padding       |
// |       block_spacing            |
// |         section                |
// |       block_spacing            |
// |         section                |
// |       block_spacing            |
// |     table border/padding       |
// |     bottom caption fragments   |
// +--------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_317.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/ng/table/ng_table_section_layout_algorithm.cc#L21-L32

```c
< ASCII >
// Generated fragment structure:
// +-----section--------------+
// |       vspacing           |
// |  +--------------------+  |
// |  |      row           |  |
// |  +--------------------+  |
// |       vspacing           |
// |  +--------------------+  |
// |  |      row           |  |
// |  +--------------------+  |
// |       vspacing           |
// +--------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_318.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/shapes/box_shape_test.cc#L68-L77

```c
/* The BoxShape is based on a 100x50 rectangle at 0,0. The shape-margin value is
 * 10, so the shape is a rectangle (120x70 at -10,-10) with rounded corners
 * (radius=10):
< ASCII >
 *
 *   -10,-10   110,-10
 *       (--------)
 *       |        |
 *       (--------)
 *   -10,60    110,60
< ASCII >
 */
```
## Visual type:
- #custom


== ./chromium/chromium_319.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/layout/shapes/box_shape_test.cc#L126-L138

```c
/* BoxShape geometry for this test. Corner radii are in parens, x and y
 * intercepts for the elliptical corners are noted. The rectangle itself is at
 * 0,0 with width and height 100.
< ASCII >
 *
 *         (10, 15)  x=10      x=90 (10, 20)
 *                (--+---------+--)
 *           y=15 +--|         |-+ y=20
 *                |               |
 *                |               |
 *           y=85 + -|         |- + y=70
 *                (--+---------+--)
 *       (25, 15)  x=25      x=80  (20, 30)
< ASCII >
 */
```
## Visual type:
- #custom


== ./chromium/chromium_32.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/desks/templates/saved_desk_unittest.cc#L1649-L1656

```c
< ASCII >
// Tests that apps with multiple window are counted correctly.
//  ______________________________________________________
//  |  ________   ________   ________________   ________ |
//  | |       |  |       |  |       |       |  |       | |
//  | |   I   |  |   I   |  |   I      + 1  |  |  + 5  | |
//  | |_______|  |_______|  |_______|_______|  |_______| |
//  |____________________________________________________|
//
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_320.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/page/focusgroup_controller_utils.cc#L190-L224

```c
// This function is called whenever the |element| passed by parameter has fallen
// into a subtree while navigating backward. Its objective is to prevent
// |element| from having descended into a non-extending focusgroup. When it
// detects its the case, it returns |element|'s first ancestor who is still part
// of the same focusgroup as |stop_ancestor|. The returned element is
// necessarily an element part of the previous focusgroup, but not necessarily a
// focusgroup item.
//
// |stop_ancestor| might be a focusgroup root itself or be a descendant of one.
// Regardless, given the assumption that |stop_ancestor| is always part of the
// previous focusgroup, we can stop going up |element|'s ancestors chain as soon
// as we reached it.
//
< ASCII >
// Let's consider this example:
//           fg1
//      ______|_____
//      |          |
//      a1       a2
//      |
//     fg2
//    __|__
//    |   |
//    b1  b2
< ASCII >
//
// where |fg2| is a focusgroup that doesn't extend the focusgroup |fg1|. While
// |fg2| is part of the focusgroup |fg1|, its subtree isn't. If the focus is on
// |a2|, the second item of the top-most focusgroup, and we go backward using
// the arrow keys, the focus should move to |fg2|. It shouldn't go inside of
// |fg2|, since it's a different focusgroup that doesn't extend its parent
// focusgroup.
//
// However, the previous element in preorder traversal from |a2| is |b2|, which
// isn't part of the same focusgroup. This function aims at fixing this by
// moving the current element to its parent, which is part of the previous
// focusgroup we were in (when we were on |a2|), |fg1|.
```
## Visual type:
- #tree


== ./chromium/chromium_321.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/page/scrolling/snap_coordinator.cc#L88-L101

```c
// Adding a snap container means that the element's descendant snap areas need
// to be reassigned to it. First we find the snap container ancestor of the new
// snap container, then check its snap areas to see if their closest ancestor is
// changed to the new snap container.
// E.g., if A1 and A2's ancestor, C2, is C1's descendant and becomes scrollable
// then C2 becomes a snap container then the snap area assignments change:
< ASCII >
//  Before            After adding C2
//       C1              C1
//       |               |
//   +-------+         +-----+
//   |  |    |         C2    |
//   A1 A2   A3      +---+   A3
//                   |   |
//                   A1  A2
< ASCII >
```
## Visual type:
- #tree 


== ./chromium/chromium_322.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/page/scrolling/sticky_position_scrolling_constraints.h#L18-L71

```c
// Encapsulates the constraint information for a position: sticky element and
// does calculation of the sticky offset for a given overflow clip rectangle.
//
// To avoid slowing down scrolling we cannot make the offset calculation a
// layout-inducing event. Instead constraint information is cached during layout
// and used as the scroll position changes to determine the current offset. In
// most cases the only information that is needed is the sticky element's layout
// rectangle and its containing block rectangle (both respective to the nearest
// ancestor scroller which the element is sticking to), and the set of sticky
// edge constraints (i.e. the distance from each edge the element should stick).
//
// For a given (non-cached) overflow clip rectangle, calculating the current
// offset in most cases just requires sliding the (cached) sticky element
// rectangle until it satisfies the (cached) sticky edge constraints for the
// overflow clip rectangle, whilst not letting the sticky element rectangle
// escape its (cached) containing block rect. For example, consider the
// following situation (where positions are relative to the scroll container):
< ASCII >
//
//    +---------------------+ <-- Containing Block (150x70 at 10,0)
//    | +-----------------+ |
//    | |    top: 50px;   |<-- Sticky Box (130x10 at 20,0)
//    | +-----------------+ |
//    |                     |
//  +-------------------------+ <-- Overflow Clip Rectangle (170x60 at 0,50)
//  | |                     | |
//  | |                     | |
//  | +---------------------+ |
//  |                         |
//  |                         |
//  |                         |
//  +-------------------------+
< ASCII >
//
// Here the cached sticky box would be moved downwards to try and be at position
// (20,100) - 50 pixels down from the top of the clip rectangle. However doing
// so would take it outside the cached containing block rectangle, so the final
// sticky position would be capped to (20,20).
//
// Unfortunately this approach breaks down in the presence of nested sticky
// elements, as the cached locations would be moved by ancestor sticky elements.
// Consider:
< ASCII >
//
//  +------------------------+ <-- Outer sticky (top: 10px, 150x50 at 0,0)
//  |  +------------------+  |
//  |  |                  | <-- Inner sticky (top: 25px, 100x20 at 20,0)
//  |  +------------------+  |
//  |                        |
//  +------------------------+
< ASCII >
//
// Assume the overflow clip rectangle is centered perfectly over the outer
// sticky. We would then want to move the outer sticky element down by 10
// pixels, and the inner sticky element down by only 15 pixels - because it is
// already being shifted by its ancestor. To correctly handle such situations we
// apply more complicated logic which is explained in the implementation of
// |ComputeStickyOffset|.
```
## Visual type:
- #gui


== ./chromium/chromium_323.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/paint/nine_piece_image_grid.h#L42-L62

```c
// The NinePieceImageGrid class is responsible for computing drawing information
// for the nine piece image.
//
// https://drafts.csswg.org/css-backgrounds/#border-image-process
//
< ASCII >
// Given an image, a set of slices and a border area:
//
//       |         |
//   +---+---------+---+          +------------------+
//   | 1 |    7    | 4 |          |      border      |
// --+---+---------+---+---       |  +------------+  |
//   |   |         |   |          |  |            |  |
//   | 3 |    9    | 6 |          |  |    css     |  |
//   |   |  image  |   |          |  |    box     |  |
//   |   |         |   |          |  |            |  |
// --+---+---------+---+---       |  |            |  |
//   | 2 |    8    | 5 |          |  +------------+  |
//   +---+---------+---+          |                  |
//       |         |              +------------------+
< ASCII >
//
// it generates drawing information for the nine border pieces.
```
## Visual type:
- #gui


== ./chromium/chromium_324.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/paint/paint_layer_clipper.h#L115-L158

```c
// Motivation for this class:
//
// The main reason for this cache is that we compute the clip rects during
// a layout tree walk but need them during a paint tree walk (see example
// below for some explanations).
//
// A lot of complexity in this class come from the difference in inheritance
// between 'overflow' and 'clip':
// * 'overflow' applies based on the containing blocks chain.
//    (http://www.w3.org/TR/CSS2/visufx.html#propdef-overflow)
// * 'clip' applies to all descendants.
//    (http://www.w3.org/TR/CSS2/visufx.html#propdef-clip)
//
// Let's take an example:
// <!DOCTYPE html>
// <div id="container" style="position: absolute; height: 100px; width: 100px">
//   <div id="inflow" style="height: 200px; width: 200px;
//       background-color: purple"></div>
//   <div id="fixed" style="height: 200px; width: 200px; position: fixed;
//       background-color: orange"></div>
// </div>
//
< ASCII >
// The paint tree looks like:
//               html
//              /   |
//             /    |
//            /     |
//      container  fixed
//         |
//         |
//       inflow
< ASCII >
//
// If we add "overflow: hidden" to #container, the overflow clip will apply to
// #inflow but not to #fixed. That's because #fixed's containing block is above
// #container and thus 'overflow' doesn't apply to it. During our tree walk,
// #fixed is a child of #container, which is the reason why we keep 3 clip rects
// depending on the 'position' of the elements.
//
// Now instead if we add "clip: rect(0px, 100px, 100px, 0px)" to #container,
// the clip will apply to both #inflow and #fixed. That's because 'clip'
// applies to any descendant, regardless of containing blocks. Note that
// #container and #fixed are siblings in the paint tree but #container does
// clip #fixed. This is the reason why we compute the painting clip rects during
// a layout tree walk and cache them for painting.
```
## Visual type:
- #tree


== ./chromium/chromium_325.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/core/paint/text_decoration_info.cc#L183-L209

```c
// Prepares a path for a cubic Bezier curve repeated three times, yielding a
// wavy pattern that we can cut into a tiling shader (PrepareWavyTileRecord).
//
// The result ignores the local origin, line offset, and (wavy) double offset,
// so the midpoints are always at y=0.5, while the phase is shifted for either
// wavy or spelling/grammar decorations so the desired pattern starts at x=0.
//
< ASCII >
// The start point, control points (cp1 and cp2), and end point of each curve
// form a diamond shape:
//
//            cp2                      cp2                      cp2
// ---         +                        +                        +
// |               x=0
// | control         |--- spelling/grammar ---|
// | point          . .                      . .                      . .
// | distance     .     .                  .     .                  .     .
// |            .         .              .         .              .         .
// +-- y=0.5   .            +           .            +           .            +
//  .         .              .         .              .         .
//    .     .                  .     .                  .     .
//      . .                      . .                      . .
//                          |-------- other ---------|
//                        x=0
//             +                        +                        +
//            cp1                      cp1                      cp1
// |-----------|------------|
//     step         step
< ASCII >
```
## Visual type:
- #plot


== ./chromium/chromium_326.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/modules/canvas/canvas2d/canvas_path.cc#L283-L315

```c
/*
 * degenerateEllipse() handles a degenerated ellipse using several lines.
 *
< ASCII >
 * Let's see a following example: line to ellipse to line.
 *        _--^\
 *       (     )
 * -----(      )
 *            )
 *           /--------
 *
 * If radiusX becomes zero, the ellipse of the example is degenerated.
 *         _
 *        // P
 *       //
 * -----//
 *      /
 *     /--------
< ASCII >
 *
 * To draw the above example, need to get P that is a local maximum point.
 * Angles for P are 0.5Pi and 1.5Pi in the ellipse coordinates.
 *
< ASCII >
 * If radiusY becomes zero, the result is as follows.
 * -----__
 *        --_
 *          ----------
 *            ``P
 * Angles for P are 0 and Pi in the ellipse coordinates.
< ASCII >
 *
 * To handle both cases, degenerateEllipse() lines to start angle, local maximum
 * points(every 0.5Pi), and end angle.
 * NOTE: Before ellipse() calls this function, adjustEndAngle() is called, so
 * endAngle - startAngle must be equal to or less than 2Pi.
 */
```
## Visual type:
- #custom


== ./chromium/chromium_327.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/modules/peerconnection/adapters/ice_transport_host.h#L20-L41

```c
// This class is the host side correspondent to the IceTransportProxy. See the
// IceTransportProxy documentation for background. This class lives on the host
// thread and proxies calls between the IceTransportProxy and the
// P2PTransportChannel (which is single-threaded).
< ASCII >
//
//     proxy thread                               host thread
// +------------------+   unique_ptr   +------------------------------+
// |                  |   =========>   |                              |
// | client <-> Proxy |                | Host <-> P2PTransportChannel |
// |                  |   <---------   |                              |
// +------------------+    WeakPtr     +------------------------------+
< ASCII >
//
// Since the client code controls the Proxy lifetime, the Proxy has a unique_ptr
// to the Host that lives on the host thread. The unique_ptr has an
// OnTaskRunnerDeleter so that when the Proxy is destroyed a task will be queued
// to delete the Host as well (and the P2PTransportChannel with it). The Host
// needs a pointer back to the Proxy to post callbacks, and by using a WeakPtr
// any callbacks run on the proxy thread after the proxy has been deleted will
// be safely dropped.
//
// The Host can be constructed on any thread but after that point all methods
// must be called on the host thread.
```
## Visual type:
- #custom


== ./chromium/chromium_328.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/modules/service_worker/service_worker_event_queue_test.cc#L229-L238

```c
< ASCII >
// Tests whether idle_time_ won't be updated in Start() when there was an
// event. The timeline is something like:
// [StartEvent] [EndEvent]
//       +----------+
//                  ^
//                  +-- idle_time_ --+
//                                   v
//                           [TimerStart]         [UpdateStatus]
//                                 +-- kUpdateInterval --+
// In the first UpdateStatus() the idle callback should be triggered.
< ASCII >
```
## Visual type:
- #flowchart


== ./chromium/chromium_329.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/modules/webdatabase/sql_transaction_backend.cc#L48-L235

```c
// How does a SQLTransaction work?
// ==============================
// The SQLTransaction is a state machine that executes a series of states /
// steps.
//
// The work of the transaction states are defined in section of 4.3.2 of the
// webdatabase spec: http://dev.w3.org/html5/webdatabase/#processing-model
//
< ASCII >
// the State Transition Graph at a glance:
// ======================================
//
//     Backend                        .   Frontend
//     (works with SQLiteDatabase)    .   (works with Script)
//     ===========================    .   ===================
//                                    .
//     1. Idle                        .
//         v                          .
//     2. AcquireLock                 .
//         v                          .
//     3. OpenTransactionAndPreflight -----------------------------------.
//         |                        .                                    |
//         `-------------------------> 8. DeliverTransactionCallback --. |
//                                  .     |                           v v
//         ,------------------------------' 9. DeliverTransactionErrorCallback +
//         |                        .                                  ^ ^ ^   |
//         v                        .                                  | | |   |
//     4. RunStatements -----------------------------------------------' | |   |
//         |        ^  ^ |  ^ |     .                                    | |   |
//         |--------'  | |  | `------> 10. DeliverStatementCallback +----' |   |
//         |           | |  `---------------------------------------'      |   |
//         |           | `-----------> 11. DeliverQuotaIncreaseCallback +  |   |
//         |            `-----------------------------------------------'  |   |
//         v                        .                                      |   |
//     5. PostflightAndCommit --+------------------------------------------'   |
//                              |----> 12. DeliverSuccessCallback +            |
//         ,--------------------'   .                             |            |
//         v                        .                             |            |
//     6. CleanupAndTerminate <-----------------------------------'            |
//         v           ^            .                                          |
//     0. End          |            .                                          |
//                     |            .                                          |
//                7: CleanupAfterTransactionErrorCallback <--------------------'
//                                  .
< ASCII >
//
// the States and State Transitions:
// ================================
//     0. SQLTransactionState::End
//         - the end state.
//
//     1. SQLTransactionState::Idle
//         - placeholder state while waiting on frontend/backend, etc. See
//           comment on "State transitions between SQLTransaction and
//           SQLTransactionBackend" below.
//
//     2. SQLTransactionState::AcquireLock (runs in backend)
//         - this is the start state.
//         - acquire the "lock".
//         - on "lock" acquisition, goto
//           SQLTransactionState::OpenTransactionAndPreflight.
//
//     3. SQLTransactionState::openTransactionAndPreflight (runs in backend)
//         - Sets up an SQLiteTransaction.
//         - begin the SQLiteTransaction.
//         - call the SQLTransactionWrapper preflight if available.
//         - schedule script callback.
//         - on error, goto
//           SQLTransactionState::DeliverTransactionErrorCallback.
//         - goto SQLTransactionState::DeliverTransactionCallback.
//
//     4. SQLTransactionState::DeliverTransactionCallback (runs in frontend)
//         - invoke the script function callback() if available.
//         - on error, goto
//           SQLTransactionState::DeliverTransactionErrorCallback.
//         - goto SQLTransactionState::RunStatements.
//
//     5. SQLTransactionState::DeliverTransactionErrorCallback (runs in
//        frontend)
//         - invoke the script function errorCallback if available.
//         - goto SQLTransactionState::CleanupAfterTransactionErrorCallback.
//
//     6. SQLTransactionState::RunStatements (runs in backend)
//         - while there are statements {
//             - run a statement.
//             - if statementCallback is available, goto
//               SQLTransactionState::DeliverStatementCallback.
//             - on error,
//               goto SQLTransactionState::DeliverQuotaIncreaseCallback, or
//               goto SQLTransactionState::DeliverStatementCallback, or
//               goto SQLTransactionState::deliverTransactionErrorCallback.
//           }
//         - goto SQLTransactionState::PostflightAndCommit.
//
//     7. SQLTransactionState::DeliverStatementCallback (runs in frontend)
//         - invoke script statement callback (assume available).
//         - on error, goto
//           SQLTransactionState::DeliverTransactionErrorCallback.
//         - goto SQLTransactionState::RunStatements.
//
//     8. SQLTransactionState::DeliverQuotaIncreaseCallback (runs in frontend)
//         - give client a chance to increase the quota.
//         - goto SQLTransactionState::RunStatements.
//
//     9. SQLTransactionState::PostflightAndCommit (runs in backend)
//         - call the SQLTransactionWrapper postflight if available.
//         - commit the SQLiteTansaction.
//         - on error, goto
//           SQLTransactionState::DeliverTransactionErrorCallback.
//         - if successCallback is available, goto
//           SQLTransactionState::DeliverSuccessCallback.
//           else goto SQLTransactionState::CleanupAndTerminate.
//
//     10. SQLTransactionState::DeliverSuccessCallback (runs in frontend)
//         - invoke the script function successCallback() if available.
//         - goto SQLTransactionState::CleanupAndTerminate.
//
//     11. SQLTransactionState::CleanupAndTerminate (runs in backend)
//         - stop and clear the SQLiteTransaction.
//         - release the "lock".
//         - goto SQLTransactionState::End.
//
//     12. SQLTransactionState::CleanupAfterTransactionErrorCallback (runs in
//         backend)
//         - rollback the SQLiteTransaction.
//         - goto SQLTransactionState::CleanupAndTerminate.
//
// State transitions between SQLTransaction and SQLTransactionBackend
// ==================================================================
// As shown above, there are state transitions that crosses the boundary between
// the frontend and backend. For example,
//
//     OpenTransactionAndPreflight (state 3 in the backend)
//     transitions to DeliverTransactionCallback (state 8 in the frontend),
//     which in turn transitions to RunStatements (state 4 in the backend).
//
// This cross boundary transition is done by posting transition requests to the
// other side and letting the other side's state machine execute the state
// transition in the appropriate thread (i.e. the script thread for the
// frontend, and the database thread for the backend).
//
// Logically, the state transitions work as shown in the graph above. But
// physically, the transition mechanism uses the Idle state (both in the
// frontend and backend) as a waiting state for further activity. For example,
// taking a closer look at the 3 state transition example above, what actually
// happens is as follows:
//
//     Step 1:
//     ======
//     In the frontend thread:
//     - waiting quietly is Idle. Not doing any work.
//
//     In the backend:
//     - is in OpenTransactionAndPreflight, and doing its work.
//     - when done, it transits to the backend DeliverTransactionCallback.
//     - the backend DeliverTransactionCallback sends a request to the frontend
//       to transit to DeliverTransactionCallback, and then itself transits to
//       Idle.
//
//     Step 2:
//     ======
//     In the frontend thread:
//     - transits to DeliverTransactionCallback and does its work.
//     - when done, it transits to the frontend RunStatements.
//     - the frontend RunStatements sends a request to the backend to transit
//       to RunStatements, and then itself transits to Idle.
//
//     In the backend:
//     - waiting quietly in Idle.
//
//     Step 3:
//     ======
//     In the frontend thread:
//     - waiting quietly is Idle. Not doing any work.
//
//     In the backend:
//     - transits to RunStatements, and does its work.
//        ...
//
// So, when the frontend or backend are not active, they will park themselves in
// their Idle states. This means their m_nextState is set to Idle, but they
// never actually run the corresponding state function. Note: for both the
// frontend and backend, the state function for Idle is unreachableState().
//
// The states that send a request to their peer across the front/back boundary
// are implemented with just 2 functions: SQLTransaction::sendToBackendState()
// and SQLTransactionBackend::sendToFrontendState(). These state functions do
// nothing but sends a request to the other side to transit to the current
// state (indicated by m_nextState), and then transits itself to the Idle state
// to wait for further action.
```
## Visual type:
- #state-machine 


== ./chromium/chromium_33.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/wm/overview/overview_highlight_controller.h#L20-L41

```c
// Manages highlighting items while in overview. Responsible for telling
// highlightable items to show or hide their focus ring borders, when tabbing
// through highlightable items with arrow keys and trackpad swipes, or when tab
// dragging. In this context, an highlightable item can represent anything
// focusable in overview mode such as a desk textfield, saved desk button and an
// `OverviewItem`. The idea behind the movement strategy is that it should be
// possible to access any highlightable view via keyboard by pressing the tab or
// arrow keys repeatedly.
< ASCII >
// +-------+  +-------+  +-------+
// |   0   |  |   1   |  |   2   |
// +-------+  +-------+  +-------+
// +-------+  +-------+  +-------+
// |   3   |  |   4   |  |   5   |
// +-------+  +-------+  +-------+
// +-------+
// |   6   |
// +-------+
< ASCII >
// Example sequences:
//  - Going right to left
//    0, 1, 2, 3, 4, 5, 6
// The highlight is switched to the next window grid (if available) or wrapped
// if it reaches the end of its movement sequence.
```
## Visual type:
- #custom


== ./chromium/chromium_330.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/audio/sinc_resampler.cc#L40-L54

```c
< ASCII >
// Input buffer layout, dividing the total buffer into regions (r0 - r5):
//
// |----------------|-----------------------------------------|----------------|
//
//                                     blockSize + kernelSize / 2
//                   <--------------------------------------------------------->
//                                                r0
//
//   kernelSize / 2   kernelSize / 2          kernelSize / 2     kernelSize / 2
// <---------------> <--------------->       <---------------> <--------------->
//         r1                r2                      r3               r4
//
//                                                     blockSize
//                                    <---------------------------------------->
//                                                         r5
< ASCII >
```
## Visual type:
- #sequence


== ./chromium/chromium_331.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/fonts/simple_font_data.cc#L311-L322

```c
< ASCII >
// Internal leadings can be distributed to ascent and descent.
// -------------------------------------------
//           | - Internal Leading (in ascent)
//           |--------------------------------
//  Ascent - |              |
//           |              |
//           |              | - Em height
// ----------|--------------|
//           |              |
// Descent - |--------------------------------
//           | - Internal Leading (in descent)
// -------------------------------------------
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_332.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/fonts/shaping/harfbuzz_font_cache.h#L23-L36

```c
// The HarfBuzzFontCache is thread specific cache for mapping
//  from |FontPlatformData| to |HarfBuzzFace|, and
//  from |FontPlatformData::UniqueID()| to |HarfBuzzFontData|.
//
//  |HarfBuzzFace| holds shared |HarfBuzzData| per unique id.
< ASCII >
//
//  |FontPlatformData-1| |FontPlatformData-2|
//         |                    |
//    |HarfBuzzFace-1|     |HarfBuzzFace-2|
//         |                    |
//         +----------+---------+
//                    |
//               |HarfBuzzFontData|
< ASCII >
//
```
## Visual type:
- #tree


== ./chromium/chromium_333.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/geometry/float_polygon_test.cc#L76-L86

```c
/**
< ASCII >
 * Checks a right triangle. This test covers all of the trivial FloatPolygon
 * accessors.
 *
 *                        200,100
 *                          /|
 *                         / |
 *                        /  |
 *                       -----
 *                 100,200   200,200
< ASCII >
 */
```
## Visual type:
- #custom


== ./chromium/chromium_334.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/geometry/float_rounded_rect_test.cc#L116-L129

```c
/*
 * FloatRoundedRect geometry for this test. Corner radii are in parens, x and y
 * intercepts for the elliptical corners are noted. The rectangle itself is at
 * 0,0 with width and height 100.
< ASCII >
 *
 *         (10, 15)  x=10      x=90 (10, 20)
 *                (--+---------+--)
 *           y=15 +--|         |-+ y=20
 *                |               |
 *                |               |
 *           y=85 + -|         |- + y=70
 *                (--+---------+--)
 *       (25, 15)  x=25      x=80  (20, 30)
< ASCII >
 */
```
## Visual type:
- #custom


== ./chromium/chromium_335.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/graphics/dark_mode_lab_color_space.h#L20-L25

```c
< ASCII >
// All matrices here are 3x3 matrices.
// They are stored in SkM44 which is 4x4 matrix in the following form.
// |a b c 0|
// |d e f 0|
// |g h i 0|
// |0 0 0 1|
< ASCII >
```
## Visual type:
- #matrix


== ./chromium/chromium_336.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/heap/thread_local.h#L22-L26

```c
< ASCII >
//         |_____component_____|___non-component___|
// ________|_tls_model__|_hide_|_tls_model__|_hide_|
// Windows | local-dyn  | yes  | init-exec  |  no  |
// Android | local-dyn  | yes  | local-dyn  |  no  |
// Other   | local-dyn  | yes  | local-exec |  no  |
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_337.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/loader/subresource_integrity.cc#L255-L266

```c
< ASCII >
// Before:
//
// [algorithm]-[hash]      OR     [algorithm]-[hash]?[options]
//             ^     ^                        ^               ^
//      position   end                 position             end
//
// After (if successful: if the method returns false, we make no promises and
// the caller should exit early):
//
// [algorithm]-[hash]      OR     [algorithm]-[hash]?[options]
//                   ^                              ^         ^
//        position/end                       position       end
< ASCII >
```
## Visual type:
- #memory-layout


== ./chromium/chromium_338.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/p2p/socket_dispatcher.h#L5-L19

```c
// P2PSocketDispatcher is a per-renderer object that dispatchers all
// P2P messages received from the browser and relays all P2P messages
// sent to the browser. P2PSocketClient instances register themselves
// with the dispatcher using RegisterClient() and UnregisterClient().
//
// Relationship of classes.
< ASCII >
//
//       P2PSocketHost                     P2PSocketClient
//            ^                                   ^
//            |                                   |
//            v                  IPC              v
//  P2PSocketDispatcherHost  <--------->  P2PSocketDispatcher
< ASCII >
//
// P2PSocketDispatcher receives and dispatches messages on the
// IO thread.
```
## Visual type:
- #class-diagram


== ./chromium/chromium_339.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/renderer/platform/widget/input/main_thread_event_queue.h#L61-L96

```c
// MainThreadEventQueue implements a queue for events that need to be
// queued between the compositor and main threads. This queue is managed
// by a lock where events are enqueued by the compositor thread
// and dequeued by the main thread.
//
// Below some example flows are how the code behaves.
// Legend: B=Browser, C=Compositor, M=Main Thread, NB=Non-blocking
//         BL=Blocking, PT=Post Task, ACK=Acknowledgement
//
< ASCII >
// Normal blocking event sent to main thread.
//   B        C        M
//   ---(BL)-->
//         (queue)
//            ---(PT)-->
//                  (deque)
//   <-------(ACK)------
//
// Non-blocking event sent to main thread.
//   B        C        M
//   ---(NB)-->
//         (queue)
//            ---(PT)-->
//                  (deque)
//
// Non-blocking followed by blocking event sent to main thread.
//   B        C        M
//   ---(NB)-->
//         (queue)
//            ---(PT)-->
//   ---(BL)-->
//         (queue)
//            ---(PT)-->
//                  (deque)
//                  (deque)
//   <-------(ACK)------
//
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_34.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/allocator/partition_allocator/gwp_asan_support.h#L19-L109

```c
// This class allows GWP-ASan allocations to be backed by PartitionAlloc and,
// consequently, protected by MiraclePtr.
//
// GWP-ASan mainly operates at the system memory page granularity. During
// process startup, it reserves a certain number of consecutive system pages.
//
< ASCII >
// The standard layout is as follows:
//
//   +-------------------+--------
//   |                   | ▲   ▲
//   |   system page 0   |(a) (c)
//   |                   | ▼   ▼
//   +-------------------+--------
//   |                   | ▲   ▲
//   |   system page 1   |(b)  |
//   |                   | ▼   |
//   +-------------------+--- (d)    (a) inaccessible
//   |                   | ▲   |     (b) accessible
//   |   system page 2   |(a)  |     (c) initial guard page
//   |                   | ▼   ▼     (d) allocation slot
//   +-------------------+--------
//   |                   | ▲   ▲
//   |   system page 3   |(b)  |
//   |                   | ▼   |
//   +-------------------+--- (d)
//   |                   | ▲   |
//   |   system page 4   |(a)  |
//   |                   | ▼   ▼
//   |-------------------|--------
//   |                   | ▲   ▲
//   |        ...        |(a) (d)
< ASCII >
//
// Unfortunately, PartitionAlloc can't provide GWP-ASan an arbitrary number of
// consecutive allocation slots. Allocations need to be grouped into 2MB super
// pages so that the allocation metadata can be easily located.
//
< ASCII >
// Below is the new layout:
//
//   +-----------------------------------
//   |                   |         ▲   ▲
//   |   system page 0   |         |   |
//   |                   |         |   |
//   +-------------------+         |   |
//   |                   |         |   |
//   |        ...        |        (e)  |
//   |                   |         |   |
//   +-------------------+-------  |   |
//   |                   | ▲   ▲   |   |
//   |  system page k-1  |(a) (c)  |   |
//   |                   | ▼   ▼   ▼   |
//   +-------------------+----------- (f)
//   |                   | ▲   ▲       |
//   |   system page k   |(b)  |       |
//   |                   | ▼   |       |
//   +-------------------+--- (d)      |
//   |                   | ▲   |       |
//   |  system page k+1  |(a)  |       |
//   |                   | ▼   ▼       |
//   +-------------------+-----------  |
//   |                   |             |    (a) inaccessible
//   |        ...        |             |    (b) accessible
//   |                   |             ▼    (c) initial guard page
//   +-----------------------------------   (d) allocation slot
//   |                   |         ▲   ▲    (e) super page metadata
//   |   system page m   |         |   |    (f) super page
//   |                   |         |   |    (g) pseudo allocation slot
//   +-------------------+-------  |   |
//   |                   |     ▲   |   |
//   |        ...        |     |  (e)  |
//   |                   |     |   |   |
//   +-------------------+--- (g)  |   |
//   |                   | ▲   |   |   |
//   | system page m+k-1 |(a)  |   |   |
//   |                   | ▼   ▼   ▼   |
//   +-------------------+----------- (f)
//   |                   | ▲   ▲       |
//   |  system page m+k  |(b)  |       |
//   |                   | ▼   |       |
//   +-------------------+--- (d)      |
//   |                   | ▲   |       |
//   | system page m+k+1 |(a)  |       |
//   |                   | ▼   ▼       |
//   +-------------------+-----------  |
//   |                   |             |
//   |        ...        |             |
//   |                   |             ▼
//   +-------------------+---------------
< ASCII >
//
// This means some allocation slots will be reserved to hold PA
// metadata. We exclude these pseudo slots from the GWP-ASan free list so that
// they are never used for anything other that storing the metadata.
```
## Visual type:
- #memory-layout


== ./chromium/chromium_340.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/editing/caret/caret-direction-auto.html#L15-L19

```c
< ASCII >
// <textarea> looks like:
// +-----------------+
// |               !A|
// |helloCBbye_      |
// +-----------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_341.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/common/security-features/resources/common.sub.js#L321-L374

```c
/**
  requestVia*(url, additionalAttributes) functions send a subresource
  request from the current environment settings object.

  @param {string} url
    The URL of the subresource.
  @param {Object<string, string>} additionalAttributes
    Additional attributes set to DOM elements
    (element-initiated requests only).

  @returns {Promise} that are resolved with a RequestResult object
  on successful requests.

  - Category 1:
      `headers`: set.
      `referrer`: set via `document.referrer`.
      `location`: set via `document.location`.
      See `template/document.html.template`.
  - Category 2:
      `headers`: set.
      `referrer`: set to `headers.referer` by `wrapResult()`.
      `location`: not set.
  - Category 3:
      All the keys listed above are NOT set.
  `sourceContextUrl` is not set here.
< ASCII >

  -------------------------------- -------- --------------------------
  Function name                    Category Used in
                                            -------- ------- ---------
                                            referrer mixed-  upgrade-
                                            policy   content insecure-
                                            policy   content request
  -------------------------------- -------- -------- ------- ---------
  requestViaAnchor                 1        Y        Y       -
  requestViaArea                   1        Y        Y       -
  requestViaAudio                  3        -        Y       -
  requestViaDedicatedWorker        2        Y        Y       Y
  requestViaFetch                  2        Y        Y       -
  requestViaForm                   2        -        Y       -
  requestViaIframe                 1        Y        Y       -
  requestViaImage                  2        Y        Y       -
  requestViaLinkPrefetch           3        -        Y       -
  requestViaLinkStylesheet         3        -        Y       -
  requestViaObject                 3        -        Y       -
  requestViaPicture                3        -        Y       -
  requestViaScript                 2        Y        Y       -
  requestViaSendBeacon             3        -        Y       -
  requestViaSharedWorker           2        Y        Y       Y
  requestViaVideo                  3        -        Y       -
  requestViaWebSocket              3        -        Y       -
  requestViaWorklet                3        -        Y       Y
  requestViaXhr                    2        Y        Y       -
  -------------------------------- -------- -------- ------- ---------
< ASCII >
*/
```
## Visual type:
- #table


== ./chromium/chromium_342.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/css/css-transforms/animation/transform-interpolation-005.html#L44-L53

```c
< ASCII >
// 2D matrix transforms:
//
// [m11 m21 0 m41]   [1 0 0 Tx] [cos(R) -sin(R) 0 0] [1 K 0 0] [Sx 0  0 0]
// [m12 m22 0 m42] = [0 1 0 Ty] [sin(R)  cos(R) 0 0] [0 1 0 0] [0  Sy 0 0]
// [ 0   0  1  0 ]   [0 0 1 0 ] [  0       0    1 0] [0 0 1 0] [0  0  1 0]
// [ 0   0  0  1 ]   [0 0 0 1 ] [  0       0    0 1] [0 0 0 1] [0  0  0 1]
//
< ASCII >
// M = translate * rotate * skew * scale
// See also webkit-transform-interpolation-005.html
//
```
## Visual type:
- #matrix 


== ./chromium/chromium_343.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/IndexedDB/resources/interleaved-cursors-common.js#L78-L93

```c
// Reads cursors in an interleaved fashion, as shown below.
//
// Given N cursors, each of which points to the beginning of a K-item sequence,
// the following accesses will be made.
< ASCII >
//
// OC(i)    = open cursor i
// RD(i, j) = read result of cursor i, which should be at item j
// CC(i)    = continue cursor i
// |        = wait for onsuccess on the previous OC or CC
//
// OC(1)            | RD(1, 1) OC(2) | RD(2, 1) OC(3) | ... | RD(n-1, 1) CC(n) |
// RD(n, 1)   CC(1) | RD(1, 2) CC(2) | RD(2, 2) CC(3) | ... | RD(n-1, 2) CC(n) |
// RD(n, 2)   CC(1) | RD(1, 3) CC(2) | RD(2, 3) CC(3) | ... | RD(n-1, 3) CC(n) |
// ...
// RD(n, k-1) CC(1) | RD(1, k) CC(2) | RD(2, k) CC(3) | ... | RD(n-1, k) CC(n) |
// RD(n, k)           done
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_344.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/shadow-dom/Extensions-to-Event-Interface.html#L40-L48

```c
/*
< ASCII >
-SR: ShadowRoot  -S: Slot  target: (~)  *: indicates start  digit: event path order
A (4) --------------------------- A-SR (3)
+ B ------------ B-SR             + A1 (2) --- A1-SR (1)
  + C            + B1 --- B1-SR   + A2-S       + A1a (*; 0)
  + D --- D-SR     + B1a    + B1b --- B1b-SR
          + D1              + B1c-S   + B1b1
                                      + B1b2
< ASCII >
*/
```
## Visual type:
- #tree


== ./chromium/chromium_345.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/shadow-dom/Extensions-to-Event-Interface.html#L68-L76

```c
/*
< ASCII >
-SR: ShadowRoot  -S: Slot  target: (~)  *: indicates start  digit: event path order
A ------------------------------- A-SR
+ B ------------ B-SR             + A1 --- A1-SR (1)
  + C            + B1 --- B1-SR   + A2-S   + A1a (*; 0)
  + D --- D-SR     + B1a  + B1b --- B1b-SR
          + D1            + B1c-S   + B1b1
                                    + B1b2
< ASCII >
*/
```
## Visual type:
- #tree


== ./chromium/chromium_346.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/shadow-dom/Extensions-to-Event-Interface.html#L94-L102

```c
/*
< ASCII >
-SR: ShadowRoot  -S: Slot  target: (~)  relatedTarget: [~]  *: indicates start  digit: event path order
A ------------------------------- A-SR
+ B ------------ B-SR             + A1 ----------- A1-SR (1)
  + C            + B1 --- B1-SR   + A2-S [*; 0-1]  + A1a (*; 0)
  + D --- D-SR     + B1a  + B1b --- B1b-SR
          + D1            + B1c-S   + B1b1
                                    + B1b2
< ASCII >
*/
```
## Visual type:
- #tree


== ./chromium/chromium_347.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/shadow-dom/Extensions-to-Event-Interface.html#L152-L160

```c
/*
< ASCII >
-SR: ShadowRoot  -S: Slot  target: (~)  relatedTarget: [~]  *: indicates start  digit: event path order
A ------------------------------- A-SR (3)
+ B ------------ B-SR             + A1 (2) ------- A1-SR (1)
  + C            + B1 --- B1-SR   + A2-S [*; 0-3]  + A1a (*; 0)
  + D --- D-SR     + B1a  + B1b --- B1b-SR
          + D1            + B1c-S   + B1b1
                                    + B1b2
< ASCII >
*/
```
## Visual type:
- #tree


== ./chromium/chromium_348.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/external/wpt/shadow-dom/Extensions-to-Event-Interface.html#L182-L190

```c
/*
< ASCII >
-SR: ShadowRoot  -S: Slot  target: (~)  relatedTarget: [~]  *: indicates start  digit: event path order
A (8) [0-5,8] ---------------------------------------- A-SR (7)
+ B (5)  ------- B-SR (4)                              + A1 [6,7] --- A1-SR
  + C            + B1 (3) ------- B1-SR (2)            + A2-S (6)     + A1a [*]
  + D --- D-SR     + B1a (*; 0)   + B1b ------- B1b-SR
          + D1                    + B1c-S (1)   + B1b1
                                                + B1b2
< ASCII >
*/
```
## Visual type:
- #tree


== ./chromium/chromium_349.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/blink/web_tests/fast/layout/common-ancestor-relayout-boundary.html#L35-L40

```c
< ASCII >
// The tree appears as following, with the starred nodes dirty:
//       div [relayout-common-ancestor]
//      /   \
//   *div  *div
//    /      /
// *div   *div
< ASCII >
```
## Visual type:
- #tree 


== ./chromium/chromium_35.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/allocator/partition_allocator/partition_alloc_constants.h#L165-L254

```c
// We reserve virtual address space in 2 MiB chunks (aligned to 2 MiB as well).
// These chunks are called *super pages*. We do this so that we can store
// metadata in the first few pages of each 2 MiB-aligned section. This makes
// freeing memory very fast. 2 MiB size & alignment were chosen, because this
// virtual address block represents a full but single page table allocation on
// ARM, ia32 and x64, which may be slightly more performance&memory efficient.
// (Note, these super pages are backed by 4 KiB system pages and have nothing to
// do with OS concept of "huge pages"/"large pages", even though the size
// coincides.)
//
< ASCII >
// The layout of the super page is as follows. The sizes below are the same for
// 32- and 64-bit platforms.
//
//     +-----------------------+
//     | Guard page (4 KiB)    |
//     | Metadata page (4 KiB) |
//     | Guard pages (8 KiB)   |
//     | TagBitmap             |
//     | Free Slot Bitmap      |
//     | *Scan State Bitmap    |
//     | Slot span             |
//     | Slot span             |
//     | ...                   |
//     | Slot span             |
//     | Guard pages (16 KiB)  |
//     +-----------------------+
//
< ASCII >
// TagBitmap is only present when
// PA_CONFIG(ENABLE_MTE_CHECKED_PTR_SUPPORT_WITH_64_BITS_POINTERS) is true.
// Free Slot Bitmap is only present when USE_FREESLOT_BITMAP is true. State
// Bitmap is inserted for partitions that may have quarantine enabled.
//
// If refcount_at_end_allocation is enabled, RefcountBitmap(4KiB) is inserted
// after the Metadata page for BackupRefPtr. The guard pages after the bitmap
// will be 4KiB.
//
< ASCII >
//...
//     | Metadata page (4 KiB) |
//     | RefcountBitmap (4 KiB)|
//     | Guard pages (4 KiB)   |
//...
< ASCII >
//
// Each slot span is a contiguous range of one or more `PartitionPage`s. Note
// that slot spans of different sizes may co-exist with one super page. Even
// slot spans of the same size may support different slot sizes. However, all
// slots within a span have to be of the same size.
//
// The metadata page has the following format. Note that the `PartitionPage`
// that is not at the head of a slot span is "unused" (by most part, it only
// stores the offset from the head page). In other words, the metadata for the
// slot span is stored only in the first `PartitionPage` of the slot span.
// Metadata accesses to other `PartitionPage`s are redirected to the first
// `PartitionPage`.
//
< ASCII >
//     +---------------------------------------------+
//     | SuperPageExtentEntry (32 B)                 |
//     | PartitionPage of slot span 1 (32 B, used)   |
//     | PartitionPage of slot span 1 (32 B, unused) |
//     | PartitionPage of slot span 1 (32 B, unused) |
//     | PartitionPage of slot span 2 (32 B, used)   |
//     | PartitionPage of slot span 3 (32 B, used)   |
//     | ...                                         |
//     | PartitionPage of slot span N (32 B, used)   |
//     | PartitionPage of slot span N (32 B, unused) |
//     | PartitionPage of slot span N (32 B, unused) |
//     +---------------------------------------------+
//
< ASCII >
// A direct-mapped page has an identical layout at the beginning to fake it
// looking like a super page:
< ASCII >
//
//     +---------------------------------+
//     | Guard page (4 KiB)              |
//     | Metadata page (4 KiB)           |
//     | Guard pages (8 KiB)             |
//     | Direct mapped object            |
//     | Guard page (4 KiB, 32-bit only) |
//     +---------------------------------+
< ASCII >
//
// A direct-mapped page's metadata page has the following layout (on 64 bit
// architectures. On 32 bit ones, the layout is identical, some sizes are
// different due to smaller pointers.):
< ASCII >
//
//     +----------------------------------+
//     | SuperPageExtentEntry (32 B)      |
//     | PartitionPage (32 B)             |
//     | PartitionBucket (40 B)           |
//     | PartitionDirectMapExtent (32 B)  |
//     +----------------------------------+
< ASCII >
//
// See |PartitionDirectMapMetadata| for details.
```
## Visual type:
- #memory-layout


== ./chromium/chromium_350.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/brotli/common/context.h#L7-L86

```c
/* Lookup table to map the previous two bytes to a context id.

  There are four different context modeling modes defined here:
    CONTEXT_LSB6: context id is the least significant 6 bits of the last byte,
    CONTEXT_MSB6: context id is the most significant 6 bits of the last byte,
    CONTEXT_UTF8: second-order context model tuned for UTF8-encoded text,
    CONTEXT_SIGNED: second-order context model tuned for signed integers.

  If |p1| and |p2| are the previous two bytes, and |mode| is current context
  mode, we calculate the context as:

    context = ContextLut(mode)[p1] | ContextLut(mode)[p2 + 256].

  For CONTEXT_UTF8 mode, if the previous two bytes are ASCII characters
  (i.e. < 128), this will be equivalent to

    context = 4 * context1(p1) + context2(p2),

  where context1 is based on the previous byte in the following way:

    0  : non-ASCII control
    1  : \t, \n, \r
    2  : space
    3  : other punctuation
    4  : " '
    5  : %
    6  : ( < [ {
    7  : ) > ] }
    8  : , ; :
    9  : .
    10 : =
    11 : number
    12 : upper-case vowel
    13 : upper-case consonant
    14 : lower-case vowel
    15 : lower-case consonant

  and context2 is based on the second last byte:

    0 : control, space
    1 : punctuation
    2 : upper-case letter, number
    3 : lower-case letter

  If the last byte is ASCII, and the second last byte is not (in a valid UTF8
  stream it will be a continuation byte, value between 128 and 191), the
  context is the same as if the second last byte was an ASCII control or space.

  If the last byte is a UTF8 lead byte (value >= 192), then the next byte will
  be a continuation byte and the context id is 2 or 3 depending on the LSB of
  the last byte and to a lesser extent on the second last byte if it is ASCII.

  If the last byte is a UTF8 continuation byte, the second last byte can be:
    - continuation byte: the next byte is probably ASCII or lead byte (assuming
      4-byte UTF8 characters are rare) and the context id is 0 or 1.
    - lead byte (192 - 207): next byte is ASCII or lead byte, context is 0 or 1
    - lead byte (208 - 255): next byte is continuation byte, context is 2 or 3

  The possible value combinations of the previous two bytes, the range of
  context ids and the type of the next byte is summarized in the table below:
< ASCII >

  |--------\-----------------------------------------------------------------|
  |         \                         Last byte                              |
  | Second   \---------------------------------------------------------------|
  | last byte \    ASCII            |   cont. byte        |   lead byte      |
  |            \   (0-127)          |   (128-191)         |   (192-)         |
  |=============|===================|=====================|==================|
  |  ASCII      | next: ASCII/lead  |  not valid          |  next: cont.     |
  | (0-127)       | context: 4 - 63     |                       | context: 2 - 3     |
  | ------------- | ------------------- | --------------------- | ------------------ |
  | cont. byte    | next: ASCII/lead    | next: ASCII/lead      | next: cont.        |
  | (128-191)     | context: 4 - 63     | context: 0 - 1        | context: 2 - 3     |
  | ------------- | ------------------- | --------------------- | ------------------ |
  | lead byte     | not valid           | next: ASCII/lead      | not valid          |
  | (192-207)     |                     | context: 0 - 1        |                    |
  | ------------- | ------------------- | --------------------- | ------------------ |
  | lead byte     | not valid           | next: cont.           | not valid          |
  | (208-)        |                     | context: 2 - 3        |                    |
  | ------------- | ------------------- | --------------------- | ------------------ |
< ASCII >
*/
```
## Visual type:
- #table 


== ./chromium/chromium_351.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L268-L300

```c
/* __kernel_cos( x,  y )
 * kernel cos function on [-pi/4, pi/4], pi/4 ~ 0.785398164
 * Input x is assumed to be bounded by ~pi/4 in magnitude.
 * Input y is the tail of x.
 *
 * Algorithm
 *      1. Since cos(-x) = cos(x), we need only to consider positive x.
 *      2. if x < 2^-27 (hx<0x3E400000 0), return 1 with inexact if x!=0.
 *      3. cos(x) is approximated by a polynomial of degree 14 on
 *         [0,pi/4]
 *                                       4            14
 *              cos(x) ~ 1 - x*x/2 + C1*x + ... + C6*x
 *         where the remez error is
< ASCII >
 *
 *      |              2     4     6     8     10    12     14 |     -58
 *      |cos(x)-(1-.5*x +C1*x +C2*x +C3*x +C4*x +C5*x  +C6*x  )| <= 2
 *      |                                                      |
 *
< ASCII >
 *                     4     6     8     10    12     14
 *      4. let r = C1*x +C2*x +C3*x +C4*x +C5*x  +C6*x  , then
 *             cos(x) = 1 - x*x/2 + r
 *         since cos(x+y) ~ cos(x) - sin(x)*y
 *                        ~ cos(x) - x*y,
 *         a correction term is necessary in cos(x) and hence
 *              cos(x+y) = 1 - (x*x/2 - (r - x*y))
 *         For better accuracy when x > 0.3, let qx = |x|/4 with
 *         the last 32 bits mask off, and if x > 0.78125, let qx = 0.28125.
 *         Then
 *              cos(x+y) = (1-qx) - ((x*x/2-qx) - (r-x*y)).
 *         Note that 1-qx and (x*x/2-qx) is EXACT here, and the
 *         magnitude of the latter is at least a quarter of x*x/2,
 *         thus, reducing the rounding error in the subtraction.
 */
```
## Visual type:
- #formula


== ./chromium/chromium_352.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L642-L668

```c
/* __kernel_sin( x, y, iy)
 * kernel sin function on [-pi/4, pi/4], pi/4 ~ 0.7854
 * Input x is assumed to be bounded by ~pi/4 in magnitude.
 * Input y is the tail of x.
 * Input iy indicates whether y is 0. (if iy=0, y assume to be 0).
 *
 * Algorithm
 *      1. Since sin(-x) = -sin(x), we need only to consider positive x.
 *      2. if x < 2^-27 (hx<0x3E400000 0), return x with inexact if x!=0.
 *      3. sin(x) is approximated by a polynomial of degree 13 on
 *         [0,pi/4]
 *                               3            13
 *              sin(x) ~ x + S1*x + ... + S6*x
 *         where
< ASCII >
 *
 *      |sin(x)         2     4     6     8     10     12  |     -58
 *      |----- - (1+S1*x +S2*x +S3*x +S4*x +S5*x  +S6*x   )| <= 2
 *      |  x                                               |
 *
< ASCII >
 *      4. sin(x+y) = sin(x) + sin'(x')*y
 *                  ~ sin(x) + (1-x*x/2)*y
 *         For better accuracy, let
 *                   3      2      2      2      2
 *              r = x *(S2+x *(S3+x *(S4+x *(S5+x *S6))))
 *         then                   3    2
 *              sin(x) = x + (S1*x + (x *(r-y/2)+y))
 */
```
## Visual type:
- #formula


== ./chromium/chromium_353.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L1317-L1346

```c
/* cos(x)
 * Return cosine function of x.
 *
 * kernel function:
 *      __kernel_sin            ... sine function on [-pi/4,pi/4]
 *      __kernel_cos            ... cosine function on [-pi/4,pi/4]
 *      __ieee754_rem_pio2      ... argument reduction routine
 *
 * Method.
 *      Let S,C and T denote the sin, cos and tan respectively on
 *      [-PI/4, +PI/4]. Reduce the argument x to y1+y2 = x-k*pi/2
 *      in [-pi/4 , +pi/4], and let n = k mod 4.
 *      We have
< ASCII >
 *
 *          n        sin(x)      cos(x)        tan(x)
 *     ----------------------------------------------------------
 *          0          S           C             T
 *          1          C          -S            -1/T
 *          2         -S          -C             T
 *          3         -C           S            -1/T
 *     ----------------------------------------------------------
< ASCII >
 *
 * Special cases:
 *      Let trig be any of sin, cos, or tan.
 *      trig(+-INF)  is NaN, with signals;
 *      trig(NaN)    is that NaN;
 *
 * Accuracy:
 *      TRIG(x) returns trig(x) nearly rounded
 */
```
## Visual type:
- #table  


== ./chromium/chromium_354.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L1532-L1548

```c
/*
 * Method :
 *    1.Reduced x to positive by atanh(-x) = -atanh(x)
 *    2.For x>=0.5
< ASCII >
 *              1              2x                          x
 *  atanh(x) = --- * log(1 + -------) = 0.5 * log1p(2 * --------)
 *              2             1 - x                      1 - x
< ASCII >
 *
 *   For x<0.5
 *  atanh(x) = 0.5*log1p(2x+2x*x/(1-x))
 *
 * Special cases:
 *  atanh(x) is NaN if |x| > 1 with signal;
 *  atanh(NaN) is that NaN with no signal;
 *  atanh(+-1) is +-INF with signal.
 *
 */
```
## Visual type:
- #formula


== ./chromium/chromium_355.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L1580-L1629

```c
/* log(x)
 * Return the logrithm of x
 *
 * Method :
 *   1. Argument Reduction: find k and f such that
 *     x = 2^k * (1+f),
 *     where  sqrt(2)/2 < 1+f < sqrt(2) .
 *
 *   2. Approximation of log(1+f).
 *  Let s = f/(2+f) ; based on log(1+f) = log(1+s) - log(1-s)
 *     = 2s + 2/3 s**3 + 2/5 s**5 + .....,
 *         = 2s + s*R
 *      We use a special Reme algorithm on [0,0.1716] to generate
 *  a polynomial of degree 14 to approximate R The maximum error
 *  of this polynomial approximation is bounded by 2**-58.45. In
 *  other words,
 *            2      4      6      8      10      12      14
 *      R(z) ~ Lg1*s +Lg2*s +Lg3*s +Lg4*s +Lg5*s  +Lg6*s  +Lg7*s
 *    (the values of Lg1 to Lg7 are listed in the program)
 *  and
< ASCII >
 *      |      2          14          |     -58.45
 *      | Lg1*s +...+Lg7*s    -  R(z) | <= 2
 *      |                             |
< ASCII >
 *  Note that 2s = f - s*f = f - hfsq + s*hfsq, where hfsq = f*f/2.
 *  In order to guarantee error in log below 1ulp, we compute log
 *  by
 *    log(1+f) = f - s*(f - R)  (if f is not too large)
 *    log(1+f) = f - (hfsq - s*(hfsq+R)). (better accuracy)
 *
 *  3. Finally,  log(x) = k*ln2 + log(1+f).
 *          = k*ln2_hi+(f-(hfsq-(s*(hfsq+R)+k*ln2_lo)))
 *     Here ln2 is split into two floating point number:
 *      ln2_hi + ln2_lo,
 *     where n*ln2_hi is always exact for |n| < 2000.
 * 
 * Special cases:
 *  log(x) is NaN with signal if x < 0 (including -INF) ;
 *  log(+INF) is +INF; log(0) is -INF with signal;
 *  log(NaN) is that NaN with no signal.
 *
 * Accuracy:
 *  according to an error analysis, the error is always less than
 *  1 ulp (unit in the last place).
 *
 * Constants:
 * The hexadecimal values are the intended ones for the following
 * constants. The decimal values may be used, provided that the
 * compiler will convert from decimal to binary accurately enough
 * to produce the hexadecimal values shown.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_356.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L1711-L1774

```c
/* double log1p(double x)
 *
 * Method :
 *   1. Argument Reduction: find k and f such that
 *      1+x = 2^k * (1+f),
 *     where  sqrt(2)/2 < 1+f < sqrt(2) .
 *
 *      Note. If k=0, then f=x is exact. However, if k!=0, then f
 *  may not be representable exactly. In that case, a correction
 *  term is need. Let u=1+x rounded. Let c = (1+x)-u, then
 *  log(1+x) - log(u) ~ c/u. Thus, we proceed to compute log(u),
 *  and add back the correction term c/u.
 *  (Note: when x > 2**53, one can simply return log(x))
 *
 *   2. Approximation of log1p(f).
 *  Let s = f/(2+f) ; based on log(1+f) = log(1+s) - log(1-s)
 *     = 2s + 2/3 s**3 + 2/5 s**5 + .....,
 *         = 2s + s*R
 *      We use a special Reme algorithm on [0,0.1716] to generate
 *  a polynomial of degree 14 to approximate R The maximum error
 *  of this polynomial approximation is bounded by 2**-58.45. In
 *  other words,
 *            2      4      6      8      10      12      14
 *      R(z) ~ Lp1*s +Lp2*s +Lp3*s +Lp4*s +Lp5*s  +Lp6*s  +Lp7*s
 *    (the values of Lp1 to Lp7 are listed in the program)
 *  and
< ASCII >
 *      |      2          14          |     -58.45
 *      | Lp1*s +...+Lp7*s    -  R(z) | <= 2
 *      |                             |
< ASCII >
 *  Note that 2s = f - s*f = f - hfsq + s*hfsq, where hfsq = f*f/2.
 *  In order to guarantee error in log below 1ulp, we compute log
 *  by
 *    log1p(f) = f - (hfsq - s*(hfsq+R)).
 *
 *  3. Finally, log1p(x) = k*ln2 + log1p(f).
 *           = k*ln2_hi+(f-(hfsq-(s*(hfsq+R)+k*ln2_lo)))
 *     Here ln2 is split into two floating point number:
 *      ln2_hi + ln2_lo,
 *     where n*ln2_hi is always exact for |n| < 2000.
 *
 * Special cases:
 *  log1p(x) is NaN with signal if x < -1 (including -INF) ;
 *  log1p(+INF) is +INF; log1p(-1) is -INF with signal;
 *  log1p(NaN) is that NaN with no signal.
 *
 * Accuracy:
 *  according to an error analysis, the error is always less than
 *  1 ulp (unit in the last place).
 *
 * Constants:
 * The hexadecimal values are the intended ones for the following
 * constants. The decimal values may be used, provided that the
 * compiler will convert from decimal to binary accurately enough
 * to produce the hexadecimal values shown.
 *
 * Note: Assuming log() return accurate answer, the following
 *   algorithm can be used to compute log1p(x) to within a few ULP:
 *
 *    u = 1+x;
 *    if(u==1.0) return x ; else
 *         return log(u)*(x/(u-1.0));
 *
 *   See HP-15C Advanced Functions Handbook, p.193.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_357.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L1874-L1929

```c
/*
 * k_log1p(f):
 * Return log(1+f) - f for 1+f in ~[sqrt(2)/2, sqrt(2)].
 *
 * The following describes the overall strategy for computing
 * logarithms in base e.  The argument reduction and adding the final
 * term of the polynomial are done by the caller for increased accuracy
 * when different bases are used.
 *
 * Method :
 *   1. Argument Reduction: find k and f such that
 *         x = 2^k * (1+f),
 *         where  sqrt(2)/2 < 1+f < sqrt(2) .
 *
 *   2. Approximation of log(1+f).
 *      Let s = f/(2+f) ; based on log(1+f) = log(1+s) - log(1-s)
 *            = 2s + 2/3 s**3 + 2/5 s**5 + .....,
 *            = 2s + s*R
 *      We use a special Reme algorithm on [0,0.1716] to generate
 *      a polynomial of degree 14 to approximate R The maximum error
 *      of this polynomial approximation is bounded by 2**-58.45. In
 *      other words,
 *          2      4      6      8      10      12      14
 *          R(z) ~ Lg1*s +Lg2*s +Lg3*s +Lg4*s +Lg5*s  +Lg6*s  +Lg7*s
 *      (the values of Lg1 to Lg7 are listed in the program)
 *      and
< ASCII >
 *          |      2          14          |     -58.45
 *          | Lg1*s +...+Lg7*s    -  R(z) | <= 2
 *          |                             |
< ASCII >
 *      Note that 2s = f - s*f = f - hfsq + s*hfsq, where hfsq = f*f/2.
 *      In order to guarantee error in log below 1ulp, we compute log
 *      by
 *          log(1+f) = f - s*(f - R)            (if f is not too large)
 *          log(1+f) = f - (hfsq - s*(hfsq+R)). (better accuracy)
 *
 *   3. Finally,  log(x) = k*ln2 + log(1+f).
 *          = k*ln2_hi+(f-(hfsq-(s*(hfsq+R)+k*ln2_lo)))
 *      Here ln2 is split into two floating point number:
 *          ln2_hi + ln2_lo,
 *      where n*ln2_hi is always exact for |n| < 2000.
 *
 * Special cases:
 *      log(x) is NaN with signal if x < 0 (including -INF) ;
 *      log(+INF) is +INF; log(0) is -INF with signal;
 *      log(NaN) is that NaN with no signal.
 *
 * Accuracy:
 *      according to an error analysis, the error is always less than
 *      1 ulp (unit in the last place).
 *
 * Constants:
 * The hexadecimal values are the intended ones for the following
 * constants. The decimal values may be used, provided that the
 * compiler will convert from decimal to binary accurately enough
 * to produce the hexadecimal values shown.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_358.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2111-L2204

```c
/* expm1(x)
 * Returns exp(x)-1, the exponential of x minus 1.
 *
 * Method
 *   1. Argument reduction:
 *  Given x, find r and integer k such that
 *
 *               x = k*ln2 + r,  |r| <= 0.5*ln2 ~ 0.34658
 *
 *      Here a correction term c will be computed to compensate
 *  the error in r when rounded to a floating-point number.
 *
 *   2. Approximating expm1(r) by a special rational function on
 *  the interval [0,0.34658]:
 *  Since
 *      r*(exp(r)+1)/(exp(r)-1) = 2+ r^2/6 - r^4/360 + ...
 *  we define R1(r*r) by
 *      r*(exp(r)+1)/(exp(r)-1) = 2+ r^2/6 * R1(r*r)
 *  That is,
 *      R1(r**2) = 6/r *((exp(r)+1)/(exp(r)-1) - 2/r)
 *         = 6/r * ( 1 + 2.0*(1/(exp(r)-1) - 1/r))
 *         = 1 - r^2/60 + r^4/2520 - r^6/100800 + ...
 *      We use a special Reme algorithm on [0,0.347] to generate
 *   a polynomial of degree 5 in r*r to approximate R1. The
 *  maximum error of this polynomial approximation is bounded
 *  by 2**-61. In other words,
 *      R1(z) ~ 1.0 + Q1*z + Q2*z**2 + Q3*z**3 + Q4*z**4 + Q5*z**5
 *  where   Q1  =  -1.6666666666666567384E-2,
 *     Q2  =   3.9682539681370365873E-4,
 *     Q3  =  -9.9206344733435987357E-6,
 *     Q4  =   2.5051361420808517002E-7,
 *     Q5  =  -6.2843505682382617102E-9;
 *    z   =  r*r,
 *  with error bounded by
< ASCII >
 *      |                  5           |     -61
 *      | 1.0+Q1*z+...+Q5*z   -  R1(z) | <= 2
 *      |                              |
 *
< ASCII >
 *  expm1(r) = exp(r)-1 is then computed by the following
 *   specific way which minimize the accumulation rounding error:
 *             2     3
 *            r     r    [ 3 - (R1 + R1*r/2)  ]
 *        expm1(r) = r + --- + --- * [--------------------]
 *                  2     2    [ 6 - r*(3 - R1*r/2) ]
 *
 *  To compensate the error in the argument reduction, we use
 *    expm1(r+c) = expm1(r) + c + expm1(r)*c
 *         ~ expm1(r) + c + r*c
 *  Thus c+r*c will be added in as the correction terms for
 *  expm1(r+c). Now rearrange the term to avoid optimization
 *   screw up:
< ASCII >
 *            (      2                                    2 )
 *            ({  ( r    [ R1 -  (3 - R1*r/2) ]  )  }    r  )
 *   expm1(r+c)~r - ({r*(--- * [--------------------]-c)-c} - --- )
 *                  ({  ( 2    [ 6 - r*(3 - R1*r/2) ]  )  }    2  )
 *                      (                                             )
 *
 *       = r - E
< ASCII >
 *   3. Scale back to obtain expm1(x):
 *  From step 1, we have
 *     expm1(x) = either 2^k*[expm1(r)+1] - 1
 *        = or     2^k*[expm1(r) + (1-2^-k)]
 *   4. Implementation notes:
 *  (A). To save one multiplication, we scale the coefficient Qi
 *       to Qi*2^i, and replace z by (x^2)/2.
 *  (B). To achieve maximum accuracy, we compute expm1(x) by
 *    (i)   if x < -56*ln2, return -1.0, (raise inexact if x!=inf)
 *    (ii)  if k=0, return r-E
 *    (iii) if k=-1, return 0.5*(r-E)-0.5
 *        (iv)  if k=1 if r < -0.25, return 2*((r+0.5)- E)
 *                  else       return  1.0+2.0*(r-E);
 *    (v)   if (k<-2||k>56) return 2^k(1-(E-r)) - 1 (or exp(x)-1)
 *    (vi)  if k <= 20, return 2^k((1-2^-k)-(E-r)), else
 *    (vii) return 2^k(1-((E+2^-k)-r))
 *
 * Special cases:
 *  expm1(INF) is INF, expm1(NaN) is NaN;
 *  expm1(-INF) is -1, and
 *  for finite argument, only expm1(0)=0 is exact.
 *
 * Accuracy:
 *  according to an error analysis, the error is always less than
 *  1 ulp (unit in the last place).
 *
 * Misc. info.
 *  For IEEE double
 *      if x >  7.09782712893383973096e+02 then expm1(x) overflow
 *
 * Constants:
 * The hexadecimal values are the intended ones for the following
 * constants. The decimal values may be used, provided that the
 * compiler will convert from decimal to binary accurately enough
 * to produce the hexadecimal values shown.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_359.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2409-L2438

```c
/* sin(x)
 * Return sine function of x.
 *
 * kernel function:
 *      __kernel_sin            ... sine function on [-pi/4,pi/4]
 *      __kernel_cos            ... cose function on [-pi/4,pi/4]
 *      __ieee754_rem_pio2      ... argument reduction routine
 *
 * Method.
 *      Let S,C and T denote the sin, cos and tan respectively on
 *      [-PI/4, +PI/4]. Reduce the argument x to y1+y2 = x-k*pi/2
 *      in [-pi/4 , +pi/4], and let n = k mod 4.
 *      We have
< ASCII >
 *
 *          n        sin(x)      cos(x)        tan(x)
 *     ----------------------------------------------------------
 *          0          S           C             T
 *          1          C          -S            -1/T
 *          2         -S          -C             T
 *          3         -C           S            -1/T
 *     ----------------------------------------------------------
 *
< ASCII >
 * Special cases:
 *      Let trig be any of sin, cos, or tan.
 *      trig(+-INF)  is NaN, with signals;
 *      trig(NaN)    is that NaN;
 *
 * Accuracy:
 *      TRIG(x) returns trig(x) nearly rounded
 */
```
## Visual type:
- #table  


== ./chromium/chromium_36.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/allocator/partition_allocator/starscan/state_bitmap.h#L24-L58

```c
// Bitmap which tracks allocation states. An allocation can be in one of 3
// states:
// - freed (00),
// - allocated (11),
// - quarantined (01 or 10, depending on the *Scan epoch).
//
< ASCII >
// The state machine of allocation states:
//         +-------------+                +-------------+
//         |             |    malloc()    |             |
//         |    Freed    +--------------->|  Allocated  |
//         |    (00)     |    (or 11)     |    (11)     |
//         |             |                |             |
//         +-------------+                +------+------+
//                ^                              |
//                |                              |
//    real_free() | (and 00)              free() | (and 01(10))
//                |                              |
//                |       +-------------+        |
//                |       |             |        |
//                +-------+ Quarantined |<-------+
//                        |   (01,10)   |
//                        |             |
//                        +-------------+
//                         ^           |
//                         |  mark()   |
//                         +-----------+
//                           (xor 11)
< ASCII >
//
// The bitmap can be safely accessed from multiple threads, but this doesn't
// imply visibility on the data (i.e. no ordering guaranties, since relaxed
// atomics are used underneath). The bitmap itself must be created inside a
// page, size and alignment of which are specified as template arguments
// |PageSize| and |PageAlignment|. |AllocationAlignment| specifies the minimal
// alignment of objects that are allocated inside a page (serves as the
// granularity in the bitmap).
```
## Visual type:
- #state-machine


== ./chromium/chromium_360.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2469-L2497

```c
/* tan(x)
 * Return tangent function of x.
 *
 * kernel function:
 *      __kernel_tan            ... tangent function on [-pi/4,pi/4]
 *      __ieee754_rem_pio2      ... argument reduction routine
 *
 * Method.
 *      Let S,C and T denote the sin, cos and tan respectively on
 *      [-PI/4, +PI/4]. Reduce the argument x to y1+y2 = x-k*pi/2
 *      in [-pi/4 , +pi/4], and let n = k mod 4.
 *      We have
 *
< ASCII >
 *          n        sin(x)      cos(x)        tan(x)
 *     ----------------------------------------------------------
 *          0          S           C             T
 *          1          C          -S            -1/T
 *          2         -S          -C             T
 *          3         -C           S            -1/T
 *     ----------------------------------------------------------
 *
< ASCII >
 * Special cases:
 *      Let trig be any of sin, cos, or tan.
 *      trig(+-INF)  is NaN, with signals;
 *      trig(NaN)    is that NaN;
 *
 * Accuracy:
 *      TRIG(x) returns trig(x) nearly rounded
 */
```
## Visual type:
- #table  


== ./chromium/chromium_361.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2520-L2541

```c
/*
 * ES6 draft 09-27-13, section 20.2.2.12.
 * Math.cosh
 * Method :
 * mathematically cosh(x) if defined to be (exp(x)+exp(-x))/2
 *      1. Replace x by |x| (cosh(x) = cosh(-x)).
< ASCII >
 *      2.
 *                                                      [ exp(x) - 1 ]^2
 *          0        <= x <= ln2/2  :  cosh(x) := 1 + -------------------
 *                                                         2*exp(x)
 *
 *                                                 exp(x) + 1/exp(x)
 *          ln2/2    <= x <= 22     :  cosh(x) := -------------------
 *                                                        2
 *          22       <= x <= lnovft :  cosh(x) := exp(x)/2
 *          lnovft   <= x <= ln2ovft:  cosh(x) := exp(x/2)/2 * exp(x/2)
 *          ln2ovft  <  x           :  cosh(x) := huge*huge (overflow)
< ASCII >
 *
 * Special cases:
 *      cosh(x) is |x| if x is +INF, -INF, or NaN.
 *      only cosh(0)=1 is exact for finite x.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_362.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2896-L2914

```c
/*
 * ES6 draft 09-27-13, section 20.2.2.30.
 * Math.sinh
 * Method :
 * mathematically sinh(x) if defined to be (exp(x)-exp(-x))/2
 *      1. Replace x by |x| (sinh(-x) = -sinh(x)).
< ASCII >
 *      2.
 *                                                  E + E/(E+1)
 *          0        <= x <= 22     :  sinh(x) := --------------, E=expm1(x)
 *                                                      2
 *
 *          22       <= x <= lnovft :  sinh(x) := exp(x)/2
 *          lnovft   <= x <= ln2ovft:  sinh(x) := exp(x/2)/2 * exp(x/2)
 *          ln2ovft  <  x           :  sinh(x) := x*shuge (overflow)
 *
< ASCII >
 * Special cases:
 *      sinh(x) is |x| if x is +Infinity, -Infinity, or NaN.
 *      only sinh(0)=0 is exact for finite x.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_363.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/fdlibm/ieee754.cc#L2948-L2970

```c
/* Tanh(x)
 * Return the Hyperbolic Tangent of x
 *
< ASCII >
 * Method :
 *                                 x    -x
 *                                e  - e
 *  0. tanh(x) is defined to be -----------
 *                                 x    -x
 *                                e  + e
 *  1. reduce x to non-negative by tanh(-x) = -tanh(x).
 *  2.  0      <= x <  2**-28 : tanh(x) := x with inexact if x != 0
 *                                          -t
 *      2**-28 <= x <  1      : tanh(x) := -----; t = expm1(-2x)
 *                                         t + 2
 *                                               2
 *      1      <= x <  22     : tanh(x) := 1 - -----; t = expm1(2x)
 *                                             t + 2
 *      22     <= x <= INF    : tanh(x) := 1.
< ASCII >
 *
 * Special cases:
 *      tanh(NaN) is NaN;
 *      only tanh(0)=0 is exact for finite argument.
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_364.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/events/keyhandler.js#L7-L95

```c
/**
 * @fileoverview This file contains a class for working with keyboard events
 * that repeat consistently across browsers and platforms. It also unifies the
 * key code so that it is the same in all browsers and platforms.
 *
 * Different web browsers have very different keyboard event handling. Most
 * importantly is that only certain browsers repeat keydown events:
 * IE, Opera, FF/Win32, and Safari 3 repeat keydown events.
 * FF/Mac and Safari 2 do not.
 *
 * For the purposes of this code, "Safari 3" means WebKit 525+, when WebKit
 * decided that they should try to match IE's key handling behavior.
 * Safari 3.0.4, which shipped with Leopard (WebKit 523), has the
 * Safari 2 behavior.
 *
 * Firefox, Safari, Opera prevent on keypress
 *
 * IE prevents on keydown
 *
 * Firefox does not fire keypress for shift, ctrl, alt
 * Firefox does fire keydown for shift, ctrl, alt, meta
 * Firefox does not repeat keydown for shift, ctrl, alt, meta
 *
 * Firefox does not fire keypress for up and down in an input
 *
 * Opera fires keypress for shift, ctrl, alt, meta
 * Opera does not repeat keypress for shift, ctrl, alt, meta
 *
 * Safari 2 and 3 do not fire keypress for shift, ctrl, alt
 * Safari 2 does not fire keydown for shift, ctrl, alt
 * Safari 3 *does* fire keydown for shift, ctrl, alt
 *
 * IE provides the keycode for keyup/down events and the charcode (in the
 * keycode field) for keypress.
 *
 * Mozilla provides the keycode for keyup/down and the charcode for keypress
 * unless it's a non text modifying key in which case the keycode is provided.
 *
 * Safari 3 provides the keycode and charcode for all events.
 *
 * Opera provides the keycode for keyup/down event and either the charcode or
 * the keycode (in the keycode field) for keypress events.
 *
 * Firefox x11 doesn't fire keydown events if a another key is already held down
 * until the first key is released. This can cause a key event to be fired with
 * a keyCode for the first key and a charCode for the second key.
 *
 * Safari in keypress
 *
< ASCII >
 *        charCode keyCode which
 * ENTER:       13      13    13
 * F1:       63236   63236 63236
 * F8:       63243   63243 63243
 * ...
 * p:          112     112   112
 * P:           80      80    80
 *
 * Firefox, keypress:
 *
 *        charCode keyCode which
 * ENTER:        0      13    13
 * F1:           0     112     0
 * F8:           0     119     0
 * ...
 * p:          112       0   112
 * P:           80       0    80
 *
 * Opera, Mac+Win32, keypress:
 *
 *         charCode keyCode which
 * ENTER: undefined      13    13
 * F1:    undefined     112     0
 * F8:    undefined     119     0
 * ...
 * p:     undefined     112   112
 * P:     undefined      80    80
 *
 * IE7, keydown
 *
 *         charCode keyCode     which
 * ENTER: undefined      13 undefined
 * F1:    undefined     112 undefined
 * F8:    undefined     119 undefined
 * ...
 * p:     undefined      80 undefined
 * P:     undefined      80 undefined
< ASCII >
 *
 * @see ../demos/keyhandler.html
 */
```
## Visual type:
- #table 


== ./chromium/chromium_365.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/fx/abstractdragdrop.js#L983-L1040

```c
/**
 * Creates a dummy target for the given cursor position. The assumption is to
 * create as big dummy target box as possible, the only constraints are:
 * - The dummy target box cannot overlap any of real target boxes.
 * - The dummy target has to contain a point with current mouse coordinates.
 *
 * NOTE: For performance reasons the box construction algorithm is kept simple
 * and it is not optimal (see example below). Currently it is O(n) in regard to
 * the number of real drop target boxes, but its result depends on the order
 * of those boxes being processed (the order in which they're added to the
 * targetList_ collection).
 *
 * The algorithm.
 * a) Assumptions
 * - Mouse pointer is in the bounding box of real target boxes.
 * - None of the boxes have negative coordinate values.
 * - Mouse pointer is not contained by any of "real target" boxes.
 * - For targets inside a scrollable container, the box used is the
 *   intersection of the scrollable container's box and the target's box.
 *   This is because the part of the target that extends outside the scrollable
 *   container should not be used in the clipping calculations.
 *
 * b) Outline
 * - Initialize the fake target to the bounding box of real targets.
 * - For each real target box - clip the fake target box so it does not contain
 *   that target box, but does contain the mouse pointer.
 *   -- Project the real target box, mouse pointer and fake target box onto
 *      both axes and calculate the clipping coordinates.
 *   -- Only one coordinate is used to clip the fake target box to keep the
 *      fake target as big as possible.
 *   -- If the projection of the real target box contains the mouse pointer,
 *      clipping for a given axis is not possible.
 *   -- If both clippings are possible, the clipping more distant from the
 *      mouse pointer is selected to keep bigger fake target area.
 * - Save the created fake target only if it has a big enough area.
 *
 *
 * c) Example
 * <pre>
< ASCII >
 *        Input:           Algorithm created box:        Maximum box:
 * +---------------------+ +---------------------+ +---------------------+
 * | B1      |        B2 | | B1               B2 | | B1               B2 |
 * |         |           | |   +-------------+   | |+-------------------+|
 * |---------x-----------| |   |             |   | ||                   ||
 * |         |           | |   |             |   | ||                   ||
 * |         |           | |   |             |   | ||                   ||
 * |         |           | |   |             |   | ||                   ||
 * |         |           | |   |             |   | ||                   ||
 * |         |           | |   +-------------+   | |+-------------------+|
 * | B4      |        B3 | | B4               B3 | | B4               B3 |
 * +---------------------+ +---------------------+ +---------------------+
< ASCII >
 * </pre>
 *
 * @param {number} x Cursor position on the x-axis.
 * @param {number} y Cursor position on the y-axis.
 * @return {goog.fx.ActiveDropTarget_} Dummy drop target.
 * @private
 */
```
## Visual type:
- #custom


== ./chromium/chromium_366.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/graphics/affinetransform.js#L18-L46

```c
/**
 * Creates a 2D affine transform. An affine transform performs a linear
 * mapping from 2D coordinates to other 2D coordinates that preserves the
 * "straightness" and "parallelness" of lines.
 *
 * Such a coordinate transformation can be represented by a 3 row by 3 column
 * matrix with an implied last row of [ 0 0 1 ]. This matrix transforms source
 * coordinates (x,y) into destination coordinates (x',y') by considering them
 * to be a column vector and multiplying the coordinate vector by the matrix
 * according to the following process:
< ASCII >
 * <pre>
 *      [ x']   [  m00  m01  m02  ] [ x ]   [ m00x + m01y + m02 ]
 *      [ y'] = [  m10  m11  m12  ] [ y ] = [ m10x + m11y + m12 ]
 *      [ 1 ]   [   0    0    1   ] [ 1 ]   [         1         ]
 * </pre>
< ASCII >
 *
 * This class is optimized for speed and minimizes calculations based on its
 * knowledge of the underlying matrix (as opposed to say simply performing
 * matrix multiplication).
 *
 * @param {number=} opt_m00 The m00 coordinate of the transform.
 * @param {number=} opt_m10 The m10 coordinate of the transform.
 * @param {number=} opt_m01 The m01 coordinate of the transform.
 * @param {number=} opt_m11 The m11 coordinate of the transform.
 * @param {number=} opt_m02 The m02 coordinate of the transform.
 * @param {number=} opt_m12 The m12 coordinate of the transform.
 * @constructor
 * @final
 */
```
## Visual type:
- #matrix 


== ./chromium/chromium_367.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/graphics/affinetransform.js#L151-L164

```c
/**
 * Pre-concatenates this transform with a scaling transformation,
 * i.e. calculates the following matrix product:
 *
< ASCII >
 * <pre>
 * [sx  0 0] [m00 m01 m02]
 * [ 0 sy 0] [m10 m11 m12]
 * [ 0  0 1] [  0   0   1]
 * </pre>
< ASCII >
 *
 * @param {number} sx The x-axis scaling factor.
 * @param {number} sy The y-axis scaling factor.
 * @return {!goog.graphics.AffineTransform} This affine transform.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_368.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/graphics/affinetransform.js#L192-L205

```c
/**
 * Pre-concatenates this transform with a translate transformation,
 * i.e. calculates the following matrix product:
 *
 * <pre>
< ASCII >
 * [1 0 dx] [m00 m01 m02]
 * [0 1 dy] [m10 m11 m12]
 * [0 0  1] [  0   0   1]
< ASCII >
 * </pre>
 *
 * @param {number} dx The distance to translate in the x direction.
 * @param {number} dy The distance to translate in the y direction.
 * @return {!goog.graphics.AffineTransform} This affine transform.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_369.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/graphics/affinetransform.js#L265-L278

```c
/**
 * Pre-concatenates this transform with a shear transformation.
 * i.e. calculates the following matrix product:
 *
 * <pre>
< ASCII >
 * [  1 shx 0] [m00 m01 m02]
 * [shy   1 0] [m10 m11 m12]
 * [  0   0 1] [  0   0   1]
< ASCII >
 * </pre>
 *
 * @param {number} shx The x shear factor.
 * @param {number} shy The y shear factor.
 * @return {!goog.graphics.AffineTransform} This affine transform.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_37.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/debug/debugger_posix.cc#L239-L252

```c
// We want to break into the debugger in Debug mode, and cause a crash dump in
// Release mode. Breakpad behaves as follows:
//
< ASCII >
// +-------+-----------------+-----------------+
// | OS    | Dump on SIGTRAP | Dump on SIGABRT |
// +-------+-----------------+-----------------+
// | Linux |       N         |        Y        |
// | Mac   |       Y         |        N        |
// +-------+-----------------+-----------------+
< ASCII >
//
// Thus we do the following:
// Linux: Debug mode if a debugger is attached, send SIGTRAP; otherwise send
//        SIGABRT
// Mac: Always send SIGTRAP.
```
## Visual type:
- #table


== ./chromium/chromium_370.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/i18n/datetimeformat.js#L26-L88

```c
/**
 * Datetime formatting functions following the pattern specification as defined
 * in JDK, ICU and CLDR, with minor modification for typical usage in JS.
 * Pattern specification:
 * {@link http://userguide.icu-project.org/formatparse/datetime}
 * <pre>
< ASCII >
 * Symbol   Meaning                    Presentation       Example
 * ------   -------                    ------------       -------
 * G#       era designator             (Text)             AD
 * y#       year                       (Number)           1996
 * Y        year (week of year)        (Number)           1997
 * u*       extended year              (Number)           4601
 * Q#       quarter                    (Text)             Q3 & 3rd quarter
 * M        month in year              (Text & Number)    July & 07
 * L        month in year (standalone) (Text & Number)    July & 07
 * d        day in month               (Number)           10
 * h        hour in am/pm (1~12)       (Number)           12
 * H        hour in day (0~23)         (Number)           0
 * m        minute in hour             (Number)           30
 * s        second in minute           (Number)           55
 * S        fractional second          (Number)           978
 * E#       day of week                (Text)             Tue & Tuesday
 * e*       day of week (local 1~7)    (Number)           2
 * c#       day of week (standalone)   (Text & Number)    2 & Tues & Tuesday & T
 * D*       day in year                (Number)           189
 * F*       day of week in month       (Number)           2 (2nd Wed in July)
 * w        week in year               (Number)           27
 * W*       week in month              (Number)           2
 * a        am/pm marker               (Text)             PM
 * k        hour in day (1~24)         (Number)           24
 * K        hour in am/pm (0~11)       (Number)           0
 * z        time zone                  (Text)             Pacific Standard Time
 * Z#       time zone (RFC 822)        (Number)           -0800
 * v#       time zone (generic)        (Text)             America/Los_Angeles
 * V#       time zone                  (Text)             Los Angeles Time
 * g*       Julian day                 (Number)           2451334
 * A*       milliseconds in day        (Number)           69540000
 * '        escape for text            (Delimiter)        'Date='
 * ''       single quote               (Literal)          'o''clock'
< ASCII >
 *
 * Item marked with '*' are not supported yet.
 * Item marked with '#' works different than java
 *
 * The count of pattern letters determine the format.
 * (Text): 4 or more, use full form, <4, use short or abbreviated form if it
 * exists. (e.g., "EEEE" produces "Monday", "EEE" produces "Mon")
 *
 * (Number): the minimum number of digits. Shorter numbers are zero-padded to
 * this amount (e.g. if "m" produces "6", "mm" produces "06"). Year is handled
 * specially; that is, if the count of 'y' is 2, the Year will be truncated to
 * 2 digits. (e.g., if "yyyy" produces "1997", "yy" produces "97".) Unlike other
 * fields, fractional seconds are padded on the right with zero.
 *
 * :(Text & Number) 3 or over, use text, otherwise use number. (e.g., "M"
 * produces "1", "MM" produces "01", "MMM" produces "Jan", and "MMMM" produces
 * "January".)
 *
 * Any characters in the pattern that are not in the ranges of ['a'..'z'] and
 * ['A'..'Z'] will be treated as quoted text. For instance, characters like ':',
 * '.', ' ', '#' and '@' will appear in the resulting time text even they are
 * not embraced within single quotes.
 * </pre>
 */
```
## Visual type:
- #table 


== ./chromium/chromium_371.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/i18n/datetimeparse.js#L24-L115

```c
/**
 * DateTimeParse is for parsing date in a locale-sensitive manner. It allows
 * user to use any customized patterns to parse date-time string under certain
 * locale. Things varies across locales like month name, weekname, field
 * order, etc.
 *
 * This module is the counter-part of DateTimeFormat. They use the same
 * date/time pattern specification, which is borrowed from ICU/JDK.
 *
 * This implementation could parse partial date/time.
 *
 * Time Format Syntax: To specify the time format use a time pattern string.
 * In this pattern, following letters are reserved as pattern letters, which
 * are defined as the following:
 *
 * <pre>
< ASCII >
 * Symbol   Meaning                 Presentation        Example
 * ------   -------                 ------------        -------
 * G        era designator          (Text)              AD
 * y#       year                    (Number)            1996
 * M        month in year           (Text & Number)     July & 07
 * d        day in month            (Number)            10
 * h        hour in am/pm (1~12)    (Number)            12
 * H        hour in day (0~23)      (Number)            0
 * m        minute in hour          (Number)            30
 * s        second in minute        (Number)            55
 * S        fractional second       (Number)            978
 * E        day of week             (Text)              Tuesday
 * D        day in year             (Number)            189
 * a        am/pm marker            (Text)              PM
 * k        hour in day (1~24)      (Number)            24
 * K        hour in am/pm (0~11)    (Number)            0
 * z        time zone               (Text)              Pacific Standard Time
 * Z        time zone (RFC 822)     (Number)            -0800
 * v        time zone (generic)     (Text)              Pacific Time
 * '        escape for text         (Delimiter)         'Date='
 * ''       single quote            (Literal)           'o''clock'
< ASCII >
 * </pre>
 *
 * The count of pattern letters determine the format. <p>
 * (Text): 4 or more pattern letters--use full form,
 *         less than 4--use short or abbreviated form if one exists.
 *         In parsing, we will always try long format, then short. <p>
 * (Number): the minimum number of digits. <p>
 * (Text & Number): 3 or over, use text, otherwise use number. <p>
 * Any characters that not in the pattern will be treated as quoted text. For
 * instance, characters like ':', '.', ' ', '#' and '@' will appear in the
 * resulting time text even they are not embraced within single quotes. In our
 * current pattern usage, we didn't use up all letters. But those unused
 * letters are strongly discouraged to be used as quoted text without quote.
 * That's because we may use other letter for pattern in future. <p>
 *
< ASCII >
 * Examples Using the US Locale:
 *
 * Format Pattern                         Result
 * --------------                         -------
 * "yyyy.MM.dd G 'at' HH:mm:ss vvvv" ->>  1996.07.10 AD at 15:08:56 Pacific Time
 * "EEE, MMM d, ''yy"                ->>  Wed, July 10, '96
 * "h:mm a"                          ->>  12:08 PM
 * "hh 'o''clock' a, zzzz"           ->>  12 o'clock PM, Pacific Daylight Time
 * "K:mm a, vvv"                     ->>  0:00 PM, PT
 * "yyyyy.MMMMM.dd GGG hh:mm aaa"    ->>  01996.July.10 AD 12:08 PM
< ASCII >
 *
 * <p> When parsing a date string using the abbreviated year pattern ("yy"),
 * DateTimeParse must interpret the abbreviated year relative to some
 * century. It does this by adjusting dates to be within 80 years before and 20
 * years after the time the parse function is called. For example, using a
 * pattern of "MM/dd/yy" and a DateTimeParse instance created on Jan 1, 1997,
 * the string "01/11/12" would be interpreted as Jan 11, 2012 while the string
 * "05/04/64" would be interpreted as May 4, 1964. During parsing, only
 * strings consisting of exactly two digits, as defined by {@link
 * java.lang.Character#isDigit(char)}, will be parsed into the default
 * century. Any other numeric string, such as a one digit string, a three or
 * more digit string will be interpreted as its face value.
 *
 * <p> If the year pattern does not have exactly two 'y' characters, the year is
 * interpreted literally, regardless of the number of digits. So using the
 * pattern "MM/dd/yyyy", "01/11/12" parses to Jan 11, 12 A.D.
 *
 * <p> When numeric fields abut one another directly, with no intervening
 * delimiter characters, they constitute a run of abutting numeric fields. Such
 * runs are parsed specially. For example, the format "HHmmss" parses the input
 * text "123456" to 12:34:56, parses the input text "12345" to 1:23:45, and
 * fails to parse "1234". In other words, the leftmost field of the run is
 * flexible, while the others keep a fixed width. If the parse fails anywhere in
 * the run, then the leftmost field is shortened by one character, and the
 * entire run is parsed again. This is repeated until either the parse succeeds
 * or the leftmost field is one character in length. If the parse still fails at
 * that point, the parse of the run fails.
 *
 * <p> Now timezone parsing only support GMT:hhmm, GMT:+hhmm, GMT:-hhmm
 */
```
## Visual type:
- #table 


== ./chromium/chromium_372.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/math/affinetransform.js#L152-L165

```c
/**
 * Pre-concatenates this transform with a scaling transformation,
 * i.e. calculates the following matrix product:
 *
 * <pre>
< ASCII >
 * [sx  0 0] [m00 m01 m02]
 * [ 0 sy 0] [m10 m11 m12]
 * [ 0  0 1] [  0   0   1]
< ASCII >
 * </pre>
 *
 * @param {number} sx The x-axis scaling factor.
 * @param {number} sy The y-axis scaling factor.
 * @return {!goog.math.AffineTransform} This affine transform.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_373.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/math/affinetransform.js#L193-L206

```c
/**
 * Pre-concatenates this transform with a translate transformation,
 * i.e. calculates the following matrix product:
 *
 * <pre>
< ASCII >
 * [1 0 dx] [m00 m01 m02]
 * [0 1 dy] [m10 m11 m12]
 * [0 0  1] [  0   0   1]
< ASCII >
 * </pre>
 *
 * @param {number} dx The distance to translate in the x direction.
 * @param {number} dy The distance to translate in the y direction.
 * @return {!goog.math.AffineTransform} This affine transform.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_374.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/math/matrix.js#L21-L48

```c
/**
 * Class for representing and manipulating matrices.
 *
 * The entry that lies in the i-th row and the j-th column of a matrix is
 * typically referred to as the i,j entry of the matrix.
 *
< ASCII >
 * The m-by-n matrix A would have its entries referred to as:
 *   [ a0,0   a0,1   a0,2   ...   a0,j  ...  a0,n ]
 *   [ a1,0   a1,1   a1,2   ...   a1,j  ...  a1,n ]
 *   [ a2,0   a2,1   a2,2   ...   a2,j  ...  a2,n ]
 *   [  .      .      .            .          .   ]
 *   [  .      .      .            .          .   ]
 *   [  .      .      .            .          .   ]
 *   [ ai,0   ai,1   ai,2   ...   ai,j  ...  ai,n ]
 *   [  .      .      .            .          .   ]
 *   [  .      .      .            .          .   ]
 *   [  .      .      .            .          .   ]
 *   [ am,0   am,1   am,2   ...   am,j  ...  am,n ]
< ASCII >
 *
 * @param {!goog.math.Matrix|!Array<!Array<number>>|!goog.math.Size|number} m
 *     A matrix to copy, a 2D-array to take as a template, a size object for
 *     dimensions, or the number of rows.
 * @param {number=} opt_n Number of columns of the matrix (only applicable if
 *     the first argument is also numeric).
 * @struct
 * @constructor
 * @final
 */
```
## Visual type:
- #matrix 


== ./chromium/chromium_375.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/math/matrix.js#L73-L82

```c
/**
 * Creates a square identity matrix. i.e. for n = 3:
 * <pre>
< ASCII >
 * [ 1 0 0 ]
 * [ 0 1 0 ]
 * [ 0 0 1 ]
< ASCII >
 * </pre>
 * @param {number} n The size of the square identity matrix.
 * @return {!goog.math.Matrix} Identity matrix of width and height `n`.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_376.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/math/tdma.js#L17-L39

```c
/**
 * Solves a linear system where the matrix is square tri-diagonal. That is,
 * given a system of equations:
 *
 * A * result = vecRight,
 *
 * this class computes result = inv(A) * vecRight, where A has the special form
 * of a tri-diagonal matrix:
 *
< ASCII >
 *    |dia(0) sup(0)   0    0     ...   0|
 *    |sub(0) dia(1) sup(1) 0     ...   0|
 * A =|                ...               |
 *    |0 ... 0 sub(n-2) dia(n-1) sup(n-1)|
 *    |0 ... 0    0     sub(n-1)   dia(n)|
< ASCII >
 *
 * @param {!Array<number>} subDiag The sub diagonal of the matrix.
 * @param {!Array<number>} mainDiag The main diagonal of the matrix.
 * @param {!Array<number>} supDiag The super diagonal of the matrix.
 * @param {!Array<number>} vecRight The right vector of the system
 *     of equations.
 * @param {Array<number>=} opt_result The optional array to store the result.
 * @return {!Array<number>} The vector that is the solution to the system.
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_377.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/structs/queue.js#L7-L36

```c
/**
 * @fileoverview Datastructure: Queue.
 *
 *
 * This file provides the implementation of a FIFO Queue structure.
 * API is similar to that of com.google.common.collect.IntQueue
 *
 * The implementation is a classic 2-stack queue.
 * There's a "front" stack and a "back" stack.
 * Items are pushed onto "back" and popped from "front".
 * When "front" is empty, we replace "front" with reverse(back).
 *
< ASCII >
 * Example:
 * front                         back            op
 * []                            []              enqueue 1
 * []                            [1]             enqueue 2
 * []                            [1,2]           enqueue 3
 * []                            [1,2,3]         dequeue -> ...
 * [3,2,1]                       []              ... -> 1
 * [3,2]                         []              enqueue 4
 * [3,2]                         [4]             dequeue -> 2
 * [3]                           [4]
< ASCII >
 *
 * Front and back are simple javascript arrays. We rely on
 * Array.push and Array.pop being O(1) amortized.
 *
 * Note: In V8, queues, up to a certain size, can be implemented
 * just fine using Array.push and Array.shift, but other JavaScript
 * engines do not have the optimization of Array.shift.
 */
```
## Visual type:
- #table  


== ./chromium/chromium_378.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/google-closure-library/closure/goog/uri/utils.js#L124-L188

```c
/**
 * A regular expression for breaking a URI into its component parts.
 *
 * {@link http://www.ietf.org/rfc/rfc3986.txt} says in Appendix B
 * As the "first-match-wins" algorithm is identical to the "greedy"
 * disambiguation method used by POSIX regular expressions, it is natural and
 * commonplace to use a regular expression for parsing the potential five
 * components of a URI reference.
 *
 * The following line is the regular expression for breaking-down a
 * well-formed URI reference into its components.
 *
 * <pre>
< ASCII >
 * ^(([^:/?#]+):)?(//([^/?#]*))?([^?#]*)(\?([^#]*))?(#(.*))?
 *  12            3  4          5       6  7        8 9
< ASCII >
 * </pre>
 *
 * The numbers in the second line above are only to assist readability; they
 * indicate the reference points for each subexpression (i.e., each paired
 * parenthesis). We refer to the value matched for subexpression <n> as $<n>.
 * For example, matching the above expression to
 * <pre>
 *     http://www.ics.uci.edu/pub/ietf/uri/#Related
 * </pre>
 * results in the following subexpression matches:
 * <pre>
 *    $1 = http:
 *    $2 = http
 *    $3 = //www.ics.uci.edu
 *    $4 = www.ics.uci.edu
 *    $5 = /pub/ietf/uri/
 *    $6 = <undefined>
 *    $7 = <undefined>
 *    $8 = #Related
 *    $9 = Related
 * </pre>
 * where <undefined> indicates that the component is not present, as is the
 * case for the query component in the above example. Therefore, we can
 * determine the value of the five components as
 * <pre>
 *    scheme    = $2
 *    authority = $4
 *    path      = $5
 *    query     = $7
 *    fragment  = $9
 * </pre>
 *
 * The regular expression has been modified slightly to expose the
 * userInfo, domain, and port separately from the authority.
 * The modified version yields
 * <pre>
 *    $1 = http              scheme
 *    $2 = <undefined>       userInfo -\
 *    $3 = www.ics.uci.edu   domain     | authority
 *    $4 = <undefined>       port     -/
 *    $5 = /pub/ietf/uri/    path
 *    $6 = <undefined>       query without ?
 *    $7 = Related           fragment without #
 * </pre>
 *
 * TODO(user): separate out the authority terminating characters once this
 * file is moved to ES6.
 * @type {!RegExp}
 * @private
 */
```
## Visual type:
- #sequence


== ./chromium/chromium_379.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/ipcz/include/ipcz/ipcz.h#L8-L123

```c
// ipcz is a cross-platform C library for interprocess communication (IPC) which
// supports efficient routing and data transfer over a large number of
// dynamically relocatable messaging endpoints.
//
// ipcz operates in terms of a small handful of abstractions encapsulated in
// this header: nodes, portals, parcels, drivers, boxes, and traps.
//
// NOTE: This header is intended to compile under C++11 or newer, and C99 or
// newer. The ABI defined here can be considered stable.
//
// Glossary
// --------
// *Nodes* are used by ipcz to model isolated units of an application. A typical
// application will create one ipcz node within each OS process it controls.
//
// *Portals* are messaging endpoints which belong to a specific node. Portals
// are created in entangled pairs: whatever goes into one portal comes out the
// other (its "peer"). Pairs may be created local to a single node, or they may
// be created to span two nodes. Portals may also be transferred freely through
// other portals.
//
// *Parcels* are the unit of communication between portals. Parcels can contain
// arbitrary application data as well as ipcz handles. Handles within a parcel
// are used to transfer objects (namely other portals, or driver-defined
// objects) from one portal to another, potentially on a different node.
//
// *Traps* provide a flexible mechanism to observe interesting portal state
// changes such as new parcels arriving or a portal's peer being closed.
//
// *Drivers* are provided by applications to implement platform-specific IPC
// details. They may also define new types of objects to be transmitted in
// parcels via boxes.
//
// *Boxes* are opaque objects used to wrap driver- or application-defined
// objects for seamless transmission across portals. Applications use the Box()
// and Unbox() APIs to go between concrete objects and transferrable box
// handles, and ipcz delegates to the driver or application to serialize boxed
// objects as needed for transmission.
//
// Overview
// --------
// To use ipcz effectively, an application must create multiple nodes to be
// interconnected. One node must be designated as the "broker" by the
// application (see CreateNode() flags). The broker is used by ipcz to
// coordinate certain types of internal transactions which demand a heightened
// level of trust and capability, so a broker node should always live in a more
// trustworthy process. For example in Chrome, the browser process is
// designated as the broker.
//
// In order for a node to communicate with other nodes in the system, the
// application must explicitly connect it to ONE other node using the
// ConnectNode() API. Once this is done, ipcz can automatically connect the node
// to additional other nodes as needed for efficient portal operation.
//
// In the example below, assume node A is designated as the broker. Nodes A and
// B have been connected directly by ConnectNode() calls in the application.
// Nodes A and C have been similarly connected:
< ASCII >
//
//                    ┌───────┐
//     ConnectNode()  │       │  ConnectNode()
//        ┌──────────>O   A   O<───────────┐
//        │           │       │            │
//        │           └───────┘            │
//        │                                │
//        v ConnectNode()                  v ConnectNode()
//    ┌───O───┐                        ┌───O───┐
//    │       │                        │       │
//    │   B   │                        │   C   │
//    │       │                        │       │
//    └───────┘                        └───────┘
< ASCII >
//
// ConnectNode() establishes initial portal pairs to link the two nodes
// together, illustrated above as "O"s. Once ConnectNode() returns, the
// application may immediately begin transmitting parcels through these portals.
//
// Now suppose node B creates a new local pair of portals (using OpenPortals())
// and sends one of those new portals in a parcel through its
// already-established portal to node A. The sent portal is effectively
// transferred to node A, and because its entangled peer still lives on node B
// there are now TWO portal pairs between nodes A and B:
< ASCII >
//
//                    ┌───────┐
//                    │       │
//        ┌──────────>O   A   O<───────────┐
//        │ ┌────────>O       │            │
//        │ │         └───────┘            │
//        │ │                              │
//        v v                              v
//    ┌───O─O─┐                        ┌───O───┐
//    │       │                        │       │
//    │   B   │                        │   C   │
//    │       │                        │       │
//    └───────┘                        └───────┘
< ASCII >
//
// Finally, suppose now the application takes this new portal on node A and
// sends it further along, through node A's already-established portal to node
// C. Because the transferred portal's peer still lives on node B, there is now
// a portal pair spanning nodes B and C:
//
< ASCII >
//                    ┌───────┐
//                    │       │
//        ┌──────────>O   A   O<───────────┐
//        │           │       │            │
//        │           └───────┘            │
//        │                                │
//        v                                v
//    ┌───O───┐                        ┌───O───┐
//    │       │                        │       │
//    │   B   O────────────────────────O   C   │
//    │       │                        │       │
//    └───────┘                        └───────┘
< ASCII >
//
// These two nodes were never explicitly connected by the application, but ipcz
// ensures that the portals will operate as expected. Behind the scenes, ipcz
// achieves this by establishing a direct, secure, and efficient communication
// channel between nodes B and C.
```
## Visual type:
- #topology 


== ./chromium/chromium_38.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/third_party/symbolize/demangle.h#L35-L68

```c
// The demangler is implemented to be used in async signal handlers to
// symbolize stack traces.  We cannot use libstdc++'s
// abi::__cxa_demangle() in such signal handlers since it's not async
// signal safe (it uses malloc() internally).
//
// Note that this demangler doesn't support full demangling.  More
// specifically, it doesn't print types of function parameters and
// types of template arguments.  It just skips them.  However, it's
// still very useful to extract basic information such as class,
// function, constructor, destructor, and operator names.
//
// See the implementation note in demangle.cc if you are interested.
//
// Example:
//
< ASCII >
// | Mangled Name  | The Demangler | abi::__cxa_demangle()
// |---------------|---------------|-----------------------
// | _Z1fv         | f()           | f()
// | _Z1fi         | f()           | f(int)
// | _Z3foo3bar    | foo()         | foo(bar)
// | _Z1fIiEvi     | f<>()         | void f<int>(int)
// | _ZN1N1fE      | N::f          | N::f
// | _ZN3Foo3BarEv | Foo::Bar()    | Foo::Bar()
// | _Zrm1XS_"     | operator%()   | operator%(X, X)
// | _ZN3FooC1Ev   | Foo::Foo()    | Foo::Foo()
// | _Z1fSs        | f()           | f(std::basic_string<char,
// |               |               |   std::char_traits<char>,
// |               |               |   std::allocator<char> >)
< ASCII >
//
// See the unit test for more examples.
//
// Note: we might want to write demanglers for ABIs other than Itanium
// C++ ABI in the future.
//
```
## Visual type:
- #table


== ./chromium/chromium_380.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/ipcz/src/ipcz/node_messages_generator.h#L412-L431

```c
// Provides a router with a new outward link to replace its existing outward
// link to some other node. Given routers X and Y on the central link, and a
// router Z as Y's inward peer:
//
//     X ==== (central) ==== Y ======== Z
//
// Z sends this message to X's node to establish a new direct link to X. Both
// X's and Z's existing links to Y are left intact in a decaying state:
< ASCII >
//
//         - - - Y - - -
//       /               \
//     X === (central) === Z
< ASCII >
//
// The recipient of this message must send a StopProxying message to Y, as well
// as a ProxyWillStop message to Z, in order for those decaying links to be
// phased out.
//
// Z must send this message to X only after receiving a BypassPeer request from
// Y. That request signifies that X's node has been adequately prepared by Y to
// authenticate this request from Z.
```
## Visual type:
- #topology


== ./chromium/chromium_381.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/ipcz/src/ipcz/node_messages_generator.h#L455-L466

```c
// Informs a router about how many more parcels it can expect to receive from
// its inward and outward peers before it can stop proxying between them and
// cease to exist. Given routers X, Y, and Z in a configuration resulting from
// a BypassPeer from Y to Z, followed by an AcceptBypassLink from Z to X:
< ASCII >
//
//         - - - Y - - -
//       /               \
//     X === (central) === Z
< ASCII >
//
// This message is sent from X to Y to provide the final length of the sequence
// of parcels routed through Y in either direction, now that X and Z have
// established a direct connection.
```
## Visual type:
- #topology


== ./chromium/chromium_382.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/ipcz/src/ipcz/node_messages_generator.h#L502-L518

```c
// Informs a router that its outward peer can be bypassed, and provides a new
// link with which to execute that bypass. Given the following arrangement where
// X, Y, and Z are routers; AND X and Y live on the same node as each other:
< ASCII >
//
//     X ==== (central) ==== Y ======== Z
< ASCII >
//
// Y sends this to Z to establish a new link (over the same NodeLink) directly
// between Z and X. Both X's and Z's existing links to Y are left intact in a
// decaying state:
< ASCII >
//
//         - - - Y - - -
//       /               \
//     X === (central) === Z
< ASCII >
//
// Note that unlike with BypassPeer/AcceptBypassLink, there is no need to
// authenticate this request, as it's only swapping one sublink out for another
// along the same NodeLink.
```
## Visual type:
- #topology


== ./chromium/chromium_383.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/ipcz/src/ipcz/node_messages_generator.h#L538-L548

```c
// Provides a router with the final length of the sequence of outbound parcels
// that will be routed to it via its decaying inward link. Given the following
// arrangement where X, Y, and Z are routers; X and Y live on the same node
// as each other: and Y has already sent a BypassPeerWithLink message to Z:
< ASCII >
//
//         - - - Y - - -
//       /               \
//     X === (central) === Z
//
< ASCII >
// This message is sent from Z back to Y, informing Y of the last parcel it can
// expect to receive from Z, now that X and Z are connected directly.
```
## Visual type:
- #topology


== ./chromium/chromium_384.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/leveldatabase/env_chromium.cc#L1191-L1216

```c
// Reports live databases and in-memory env's to memory-infra. For each live
// database the following information is reported:
// 1. Instance pointer (to disambiguate databases).
// 2. Memory taken by the database, with the shared cache being attributed
// equally to each database sharing 3. The name of the database (when not in
// BACKGROUND mode to avoid exposing
//    PIIs in slow reports).
//
// Example report (as seen after clicking "leveldatabase" in "Overview" pane
// in Chrome tracing UI):
< ASCII >
//
// Component                  effective_size  size        name
// ---------------------------------------------------------------------------
// leveldatabase              390 KiB         490 KiB
//   db_0x7FE70F2040A0        100 KiB         100 KiB     Users/.../Sync
//     block_cache (browser)  40 KiB          40 KiB
//   db_0x7FE70F530D80        150 KiB         150 KiB     Users/.../Data Proxy
//     block_cache (web)      30 KiB          30 KiB
//   db_0x7FE70F530D80        140 KiB         140 KiB     Users/.../Extensions
//     block_cache (web)      30 KiB          30 KiB
//   block_cache              0 KiB           100 KiB
//     browser                0 KiB           40 KiB
//     web                    0 KiB           60 KiB
//   memenv_0x7FE80F2040A0    4 KiB           4 KiB
//   memenv_0x7FE80F3040A0    4 KiB           4 KiB
< ASCII >
//
```
## Visual type:
- #table


== ./chromium/chromium_385.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/libpng/png.h#L189-L264

```c
/* Note about libpng version numbers:
 *
 *    Due to various miscommunications, unforeseen code incompatibilities
 *    and occasional factors outside the authors' control, version numbering
 *    on the library has not always been consistent and straightforward.
 *    The following table summarizes matters since version 0.89c, which was
 *    the first widely used release:
 *
< ASCII >
 *    source                 png.h  png.h  shared-lib
 *    version                string   int  version
 *    -------                ------ -----  ----------
 *    0.89c "1.0 beta 3"     0.89      89  1.0.89
 *    0.90  "1.0 beta 4"     0.90      90  0.90  [should have been 2.0.90]
 *    0.95  "1.0 beta 5"     0.95      95  0.95  [should have been 2.0.95]
 *    0.96  "1.0 beta 6"     0.96      96  0.96  [should have been 2.0.96]
 *    0.97b "1.00.97 beta 7" 1.00.97   97  1.0.1 [should have been 2.0.97]
 *    0.97c                  0.97      97  2.0.97
 *    0.98                   0.98      98  2.0.98
 *    0.99                   0.99      98  2.0.99
 *    0.99a-m                0.99      99  2.0.99
 *    1.00                   1.00     100  2.1.0 [100 should be 10000]
 *    1.0.0      (from here on, the   100  2.1.0 [100 should be 10000]
 *    1.0.1       png.h string is   10001  2.1.0
 *    1.0.1a-e    identical to the  10002  from here on, the shared library
 *    1.0.2       source version)   10002  is 2.V where V is the source code
 *    1.0.2a-b                      10003  version, except as noted.
 *    1.0.3                         10003
 *    1.0.3a-d                      10004
 *    1.0.4                         10004
 *    1.0.4a-f                      10005
 *    1.0.5 (+ 2 patches)           10005
 *    1.0.5a-d                      10006
 *    1.0.5e-r                      10100 (not source compatible)
 *    1.0.5s-v                      10006 (not binary compatible)
 *    1.0.6 (+ 3 patches)           10006 (still binary incompatible)
 *    1.0.6d-f                      10007 (still binary incompatible)
 *    1.0.6g                        10007
 *    1.0.6h                        10007  10.6h (testing xy.z so-numbering)
 *    1.0.6i                        10007  10.6i
 *    1.0.6j                        10007  2.1.0.6j (incompatible with 1.0.0)
 *    1.0.7beta11-14        DLLNUM  10007  2.1.0.7beta11-14 (binary compatible)
 *    1.0.7beta15-18           1    10007  2.1.0.7beta15-18 (binary compatible)
 *    1.0.7rc1-2               1    10007  2.1.0.7rc1-2 (binary compatible)
 *    1.0.7                    1    10007  (still compatible)
 *    ...
 *    1.0.69                  10    10069  10.so.0.69[.0]
 *    ...
 *    1.2.59                  13    10259  12.so.0.59[.0]
 *    ...
 *    1.4.20                  14    10420  14.so.0.20[.0]
 *    ...
 *    1.5.30                  15    10530  15.so.15.30[.0]
 *    ...
 *    1.6.37                  16    10637  16.so.16.37[.0]
< ASCII >
 *
 *    Henceforth the source version will match the shared-library major and
 *    minor numbers; the shared-library major version number will be used for
 *    changes in backward compatibility, as it is intended.
 *    The PNG_LIBPNG_VER macro, which is not used within libpng but is
 *    available for applications, is an unsigned integer of the form XYYZZ
 *    corresponding to the source version X.Y.Z (leading zeros in Y and Z).
 *    Beta versions were given the previous public release number plus a
 *    letter, until version 1.0.6j; from then on they were given the upcoming
 *    public release number plus "betaNN" or "rcNN".
 *
 *    Binary incompatibility exists only when applications make direct access
 *    to the info_ptr or png_ptr members through png.h, and the compiled
 *    application is loaded with a different version of the library.
 *
 *    DLLNUM will change each time there are forward or backward changes
 *    in binary compatibility (e.g., when a new feature is added).
 *
 * See libpng.txt or libpng.3 for more information.  The PNG specification
 * is available as a W3C Recommendation and as an ISO/IEC Standard; see
 * <https://www.w3.org/TR/2003/REC-PNG-20031110/>
 */
```
## Visual type:
- #table


== ./chromium/chromium_386.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/lzma_sdk/C/Bra.h#L11-L52

```c
/*
These functions convert relative addresses to absolute addresses
in CALL instructions to increase the compression ratio.
  
  In:
    data     - data buffer
    size     - size of data
    ip       - current virtual Instruction Pinter (IP) value
    state    - state variable for x86 converter
    encoding - 0 (for decoding), 1 (for encoding)
  
  Out:
    state    - state variable for x86 converter

  Returns:
    The number of processed bytes. If you call these functions with multiple calls,
    you must start next call with first byte after block of processed bytes.
< ASCII >
  
  Type   Endian  Alignment  LookAhead
  
  x86    little      1          4
  ARMT   little      2          2
  ARM    little      4          0
  PPC     big        4          0
  SPARC   big        4          0
  IA64   little     16          0
< ASCII >

  size must be >= Alignment + LookAhead, if it's not last block.
  If (size < Alignment + LookAhead), converter returns 0.

  Example:

    UInt32 ip = 0;
    for ()
    {
      ; size must be >= Alignment + LookAhead, if it's not last block
      SizeT processed = Convert(data, size, ip, 1);
      data += processed;
      size -= processed;
      ip += processed;
    }
*/
```
## Visual type:
- #table 


== ./chromium/chromium_387.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/opus/src/celt/cwrs.c#L74-L191

```c
/*Although derived separately, the pulse vector coding scheme is equivalent to
   a Pyramid Vector Quantizer \cite{Fis86}.
  Some additional notes about an early version appear at
   https://people.xiph.org/~tterribe/notes/cwrs.html, but the codebook ordering
   and the definitions of some terms have evolved since that was written.

  The conversion from a pulse vector to an integer index (encoding) and back
   (decoding) is governed by two related functions, V(N,K) and U(N,K).

  V(N,K) = the number of combinations, with replacement, of N items, taken K
   at a time, when a sign bit is added to each item taken at least once (i.e.,
   the number of N-dimensional unit pulse vectors with K pulses).
  One way to compute this is via
    V(N,K) = K>0 ? sum(k=1...K,2**k*choose(N,k)*choose(K-1,k-1)) : 1,
   where choose() is the binomial function.
  A table of values for N<10 and K<10 looks like:
< ASCII >
  V[10][10] = {
    {1,  0,   0,    0,    0,     0,     0,      0,      0,       0},
    {1,  2,   2,    2,    2,     2,     2,      2,      2,       2},
    {1,  4,   8,   12,   16,    20,    24,     28,     32,      36},
    {1,  6,  18,   38,   66,   102,   146,    198,    258,     326},
    {1,  8,  32,   88,  192,   360,   608,    952,   1408,    1992},
    {1, 10,  50,  170,  450,  1002,  1970,   3530,   5890,    9290},
    {1, 12,  72,  292,  912,  2364,  5336,  10836,  20256,   35436},
    {1, 14,  98,  462, 1666,  4942, 12642,  28814,  59906,  115598},
    {1, 16, 128,  688, 2816,  9424, 27008,  68464, 157184,  332688},
    {1, 18, 162,  978, 4482, 16722, 53154, 148626, 374274,  864146}
  };
< ASCII >

  U(N,K) = the number of such combinations wherein N-1 objects are taken at
   most K-1 at a time.
  This is given by
    U(N,K) = sum(k=0...K-1,V(N-1,k))
           = K>0 ? (V(N-1,K-1) + V(N,K-1))/2 : 0.
  The latter expression also makes clear that U(N,K) is half the number of such
   combinations wherein the first object is taken at least once.
  Although it may not be clear from either of these definitions, U(N,K) is the
   natural function to work with when enumerating the pulse vector codebooks,
   not V(N,K).
  U(N,K) is not well-defined for N=0, but with the extension
    U(0,K) = K>0 ? 0 : 1,
   the function becomes symmetric: U(N,K) = U(K,N), with a similar table:
< ASCII >
  U[10][10] = {
    {1, 0,  0,   0,    0,    0,     0,     0,      0,      0},
    {0, 1,  1,   1,    1,    1,     1,     1,      1,      1},
    {0, 1,  3,   5,    7,    9,    11,    13,     15,     17},
    {0, 1,  5,  13,   25,   41,    61,    85,    113,    145},
    {0, 1,  7,  25,   63,  129,   231,   377,    575,    833},
    {0, 1,  9,  41,  129,  321,   681,  1289,   2241,   3649},
    {0, 1, 11,  61,  231,  681,  1683,  3653,   7183,  13073},
    {0, 1, 13,  85,  377, 1289,  3653,  8989,  19825,  40081},
    {0, 1, 15, 113,  575, 2241,  7183, 19825,  48639, 108545},
    {0, 1, 17, 145,  833, 3649, 13073, 40081, 108545, 265729}
  };
< ASCII >

  With this extension, V(N,K) may be written in terms of U(N,K):
    V(N,K) = U(N,K) + U(N,K+1)
   for all N>=0, K>=0.
  Thus U(N,K+1) represents the number of combinations where the first element
   is positive or zero, and U(N,K) represents the number of combinations where
   it is negative.
  With a large enough table of U(N,K) values, we could write O(N) encoding
   and O(min(N*log(K),N+K)) decoding routines, but such a table would be
   prohibitively large for small embedded devices (K may be as large as 32767
   for small N, and N may be as large as 200).

  Both functions obey the same recurrence relation:
    V(N,K) = V(N-1,K) + V(N,K-1) + V(N-1,K-1),
    U(N,K) = U(N-1,K) + U(N,K-1) + U(N-1,K-1),
   for all N>0, K>0, with different initial conditions at N=0 or K=0.
  This allows us to construct a row of one of the tables above given the
   previous row or the next row.
  Thus we can derive O(NK) encoding and decoding routines with O(K) memory
   using only addition and subtraction.

  When encoding, we build up from the U(2,K) row and work our way forwards.
  When decoding, we need to start at the U(N,K) row and work our way backwards,
   which requires a means of computing U(N,K).
  U(N,K) may be computed from two previous values with the same N:
    U(N,K) = ((2*N-1)*U(N,K-1) - U(N,K-2))/(K-1) + U(N,K-2)
   for all N>1, and since U(N,K) is symmetric, a similar relation holds for two
   previous values with the same K:
    U(N,K>1) = ((2*K-1)*U(N-1,K) - U(N-2,K))/(N-1) + U(N-2,K)
   for all K>1.
  This allows us to construct an arbitrary row of the U(N,K) table by starting
   with the first two values, which are constants.
  This saves roughly 2/3 the work in our O(NK) decoding routine, but costs O(K)
   multiplications.
  Similar relations can be derived for V(N,K), but are not used here.

  For N>0 and K>0, U(N,K) and V(N,K) take on the form of an (N-1)-degree
   polynomial for fixed N.
  The first few are
    U(1,K) = 1,
    U(2,K) = 2*K-1,
    U(3,K) = (2*K-2)*K+1,
    U(4,K) = (((4*K-6)*K+8)*K-3)/3,
    U(5,K) = ((((2*K-4)*K+10)*K-8)*K+3)/3,
   and
    V(1,K) = 2,
    V(2,K) = 4*K,
    V(3,K) = 4*K*K+2,
    V(4,K) = 8*(K*K+2)*K/3,
    V(5,K) = ((4*K*K+20)*K*K+6)/3,
   for all K>0.
  This allows us to derive O(N) encoding and O(N*log(K)) decoding routines for
   small N (and indeed decoding is also O(N) for N<3).

  @ARTICLE{Fis86,
    author="Thomas R. Fischer",
    title="A Pyramid Vector Quantizer",
    journal="IEEE Transactions on Information Theory",
    volume="IT-32",
    number=4,
    pages="568--583",
    month=Jul,
    year=1986
  }*/
```
## Visual type:



== ./chromium/chromium_388.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/opus/src/silk/resampler.c#L32-L48

```c
/*
< ASCII >
 * Matrix of resampling methods used:
 *                                 Fs_out (kHz)
 *                        8      12     16     24     48
 *
 *               8        C      UF     U      UF     UF
 *              12        AF     C      UF     U      UF
 * Fs_in (kHz)  16        D      AF     C      UF     UF
 *              24        AF     D      AF     C      U
 *              48        AF     AF     AF     D      C
 *
< ASCII >
 * C   -> Copy (no resampling)
 * D   -> Allpass-based 2x downsampling
 * U   -> Allpass-based 2x upsampling
 * UF  -> Allpass-based 2x upsampling followed by FIR interpolation
 * AF  -> AR2 filter followed by FIR interpolation
 */
```
## Visual type:
- #matrix


== ./chromium/chromium_389.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/protobuf/src/google/protobuf/generated_message_tctable_impl.h#L56-L74

```c
// Field layout enums.
//
// Structural information about fields is packed into a 16-bit value. The enum
// types below represent bitwise fields, along with their respective widths,
// shifts, and masks.
< ASCII >
//
//     Bit:
//     +-----------------------+-----------------------+
//     |15        ..          8|7         ..          0|
//     +-----------------------+-----------------------+
//     :  .  :  .  :  .  :  .  :  .  :  .  : 3|========| [3] FieldType
//     :     :     :     :     :     : 5|=====|  :     : [2] FieldCardinality
//     :  .  :  .  :  .  :  . 8|========|  :  .  :  .  : [3] FieldRep
//     :     :     :   10|=====|     :     :     :     : [2] TransformValidation
//     :  .  :  .12|=====|  .  :  .  :  .  :  .  :  .  : [2] FormatDiscriminator
//     +-----------------------+-----------------------+
//     |15        ..          8|7         ..          0|
//     +-----------------------+-----------------------+
< ASCII >
//
```
## Visual type:
- #memory-layout


== ./chromium/chromium_39.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/base/win/com_init_check_hook.cc#L29-L66

```c
// Hotpatchable Microsoft x86 32-bit functions take one of two forms:
// Newer format:
< ASCII >
// RelAddr  Binary     Instruction                 Remarks
//      -5  cc         int 3
//      -4  cc         int 3
//      -3  cc         int 3
//      -2  cc         int 3
//      -1  cc         int 3
//       0  8bff       mov edi,edi                 Actual entry point and no-op.
//       2  ...                                    Actual body.
//
// Older format:
// RelAddr  Binary     Instruction                 Remarks
//      -5  90         nop
//      -4  90         nop
//      -3  90         nop
//      -2  90         nop
//      -1  90         nop
//       0  8bff       mov edi,edi                 Actual entry point and no-op.
//       2  ...                                    Actual body.
< ASCII >
//
// The "int 3" or nop sled as well as entry point no-op are critical, as they
// are just enough to patch in a short backwards jump to -5 (2 bytes) then that
// can do a relative 32-bit jump about 2GB before or after the current address.
//
// To perform a hotpatch, we need to figure out where we want to go and where
// we are now as the final jump is relative. Let's say we want to jump to
// 0x12345678. Relative jumps are calculated from eip, which for our jump is the
// next instruction address. For the example above, that means we start at a 0
// base address.
//
< ASCII >
// Our patch will then look as follows:
// RelAddr  Binary     Instruction                 Remarks
//      -5  e978563412 jmp 0x12345678-(-0x5+0x5)   Note little-endian format.
//       0  ebf9       jmp -0x5-(0x0+0x2)          Goes to RelAddr -0x5.
//       2  ...                                    Actual body.
< ASCII >
// Note: The jmp instructions above are structured as
//       Address(Destination)-(Address(jmp Instruction)+sizeof(jmp Instruction))
```
## Visual type:
- #table


== ./chromium/chromium_390.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/protobuf/src/google/protobuf/compiler/csharp/csharp_helpers.cc#L190-L196

```c
< ASCII >
// Previous input character      Current character         Case
// Any                           Non-alphanumeric          Skipped
// None - first char of input    Alphanumeric              Upper
// Non-letter (e.g. _ or 1)      Alphanumeric              Upper
// Numeric                       Alphanumeric              Upper
// Lower letter                  Alphanumeric              Same as current
// Upper letter                  Alphanumeric              Lower
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_391.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/protobuf/third_party/utf8_range/naive.c#L3-L29

```c
/*
 * http://www.unicode.org/versions/Unicode6.0.0/ch03.pdf - page 94
 *
 * Table 3-7. Well-Formed UTF-8 Byte Sequences
 *
< ASCII >
 * +--------------------+------------+-------------+------------+-------------+
 * | Code Points        | First Byte | Second Byte | Third Byte | Fourth Byte |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+0000..U+007F     | 00..7F     |             |            |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+0080..U+07FF     | C2..DF     | 80..BF      |            |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+0800..U+0FFF     | E0         | A0..BF      | 80..BF     |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+1000..U+CFFF     | E1..EC     | 80..BF      | 80..BF     |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+D000..U+D7FF     | ED         | 80..9F      | 80..BF     |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+E000..U+FFFF     | EE..EF     | 80..BF      | 80..BF     |             |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+10000..U+3FFFF   | F0         | 90..BF      | 80..BF     | 80..BF      |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+40000..U+FFFFF   | F1..F3     | 80..BF      | 80..BF     | 80..BF      |
 * +--------------------+------------+-------------+------------+-------------+
 * | U+100000..U+10FFFF | F4         | 80..8F      | 80..BF     | 80..BF      |
 * +--------------------+------------+-------------+------------+-------------+
< ASCII >
 */
```
## Visual type:
- #table


== ./chromium/chromium_392.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/puffin/src/puffin_unittest.cc#L514-L533

```c
// The following is a sequence of bits starting from the top right and ends in
// bottom left. It represents the bits in |kGapDeflates|. Bits inside the
// brackets (including bits exactly under brackets) represent a deflate stream.
< ASCII >
//
//       }   {                  } {                  }{                  }
// 11000101 10000000 10001100 01010000 00010001 10001000 00000100 01100010
//   0xC5     0x80     0x8C     0x50     0x11     0x88     0x04     0x62
//
//      }         {                  } {                  }   {
// 10001011 11111100 00000100 01100010 00000001 00011000 10111000 00001000
//   0x8B     0xFC     0x04     0x62     0x01     0x18     0xB8     0x08
//
//      }   {                  }         {                  }{
// 10001011 00000001 00011000 10111111 11000000 01000110 00100000 00010001
//   0x8B     0x01     0x18     0xBF     0xC0     0x46     0x20     0x11
//
//       {                  }          {                  }  {
// 11111100 00000100 01100010 11111111 00000001 00011000 10110000 00010001
//   0xFC     0x04     0x62     0xFF     0x01     0x18     0xB0     0x11
< ASCII >
//
```
## Visual type:
- #memory-layout


== ./chromium/chromium_393.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/bindgen/v0_60/crate/src/codegen/bitfield_unit_tests.rs#L1-L22

```c
//! Tests for `__BindgenBitfieldUnit`.
//!
//! Note that bit-fields are allocated right to left (least to most significant
//! bits).
//!
//! From the x86 PS ABI:
//!
//! ```c
//! struct {
//!     int j : 5;
//!     int k : 6;
//!     int m : 7;
//! };
//! ```
//!
//! ```ignore
< ASCII >
//! +------------------------------------------------------------+
//! |                     |              |            |          |
//! |       padding       |       m      |     k      |    j     |
//! |31                 18|17          11|10         5|4        0|
//! +------------------------------------------------------------+
< ASCII >
//! ```
```
## Visual type:
- #memory-layout


== ./chromium/chromium_394.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/bindgen/v0_60/crate/src/ir/derive.rs#L81-L94

```c
/// Whether it is possible or not to automatically derive trait for an item.
///
/// ```ignore
< ASCII >
///         No
///          ^
///          |
///      Manually
///          ^
///          |
///         Yes
< ASCII >
/// ```
///
/// Initially we assume that we can derive trait for all types and then
/// update our understanding as we learn more about each type.
```
## Visual type:
- #flowchart


== ./chromium/chromium_395.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/bindgen/v0_60/crate/src/ir/template.rs#L36-L101

```c
/// Template declaration (and such declaration's template parameters) related
/// methods.
///
/// This trait's methods distinguish between `None` and `Some([])` for
/// declarations that are not templates and template declarations with zero
/// parameters, in general.
///
/// Consider this example:
///
/// ```c++
/// template <typename T, typename U>
/// class Foo {
///     T use_of_t;
///     U use_of_u;
///
///     template <typename V>
///     using Bar = V*;
///
///     class Inner {
///         T        x;
///         U        y;
///         Bar<int> z;
///     };
///
///     template <typename W>
///     class Lol {
///         // No use of W, but here's a use of T.
///         T t;
///     };
///
///     template <typename X>
///     class Wtf {
///         // X is not used because W is not used.
///         Lol<X> lololol;
///     };
/// };
///
/// class Qux {
///     int y;
/// };
/// ```
///
/// The following table depicts the results of each trait method when invoked on
/// each of the declarations above:
///
< ASCII >
/// +------+----------------------+--------------------------+------------------------+----
/// |Decl. | self_template_params | num_self_template_params | all_template_parameters| ...
/// +------+----------------------+--------------------------+------------------------+----
/// |Foo   | [T, U]               | 2                        | [T, U]                 | ...
/// |Bar   | [V]                  | 1                        | [T, U, V]              | ...
/// |Inner | []                   | 0                        | [T, U]                 | ...
/// |Lol   | [W]                  | 1                        | [T, U, W]              | ...
/// |Wtf   | [X]                  | 1                        | [T, U, X]              | ...
/// |Qux   | []                   | 0                        | []                     | ...
/// +------+----------------------+--------------------------+------------------------+----
///
/// ----+------+-----+----------------------+
/// ... |Decl. | ... | used_template_params |
/// ----+------+-----+----------------------+
/// ... |Foo   | ... | [T, U]               |
/// ... |Bar   | ... | [V]                  |
/// ... |Inner | ... | []                   |
/// ... |Lol   | ... | [T]                  |
/// ... |Wtf   | ... | [T]                  |
/// ... |Qux   | ... | []                   |
/// ----+------+-----+----------------------+
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_396.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/bindgen/v0_60/crate/src/ir/analysis/mod.rs#L1-L38

```c
//! Fix-point analyses on the IR using the "monotone framework".
//!
//! A lattice is a set with a partial ordering between elements, where there is
//! a single least upper bound and a single greatest least bound for every
//! subset. We are dealing with finite lattices, which means that it has a
//! finite number of elements, and it follows that there exists a single top and
//! a single bottom member of the lattice. For example, the power set of a
//! finite set forms a finite lattice where partial ordering is defined by set
//! inclusion, that is `a <= b` if `a` is a subset of `b`. Here is the finite
//! lattice constructed from the set {0,1,2}:
//!
< ASCII >
//! ```text
//!                    .----- Top = {0,1,2} -----.
//!                   /            |              \
//!                  /             |               \
//!                 /              |                \
//!              {0,1} -------.  {0,2}  .--------- {1,2}
//!                |           \ /   \ /             |
//!                |            /     \              |
//!                |           / \   / \             |
//!               {0} --------'   {1}   `---------- {2}
//!                 \              |                /
//!                  \             |               /
//!                   \            |              /
//!                    `------ Bottom = {} ------'
//! ```
//!
< ASCII >
//! A monotone function `f` is a function where if `x <= y`, then it holds that
//! `f(x) <= f(y)`. It should be clear that running a monotone function to a
//! fix-point on a finite lattice will always terminate: `f` can only "move"
//! along the lattice in a single direction, and therefore can only either find
//! a fix-point in the middle of the lattice or continue to the top or bottom
//! depending if it is ascending or descending the lattice respectively.
//!
//! For a deeper introduction to the general form of this kind of analysis, see
//! [Static Program Analysis by Anders Møller and Michael I. Schwartzbach][spa].
//!
//! [spa]: https://cs.au.dk/~amoeller/spa/spa.pdf
```
## Visual type:
- #custom


== ./chromium/chromium_397.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/bindgen/v0_60/crate/src/ir/analysis/sizedness.rs#L13-L26

```c
/// The result of the `Sizedness` analysis for an individual item.
///
/// This is a chain lattice of the form:
///
< ASCII >
/// ```ignore
///                   NonZeroSized
///                        |
///                DependsOnTypeParam
///                        |
///                     ZeroSized
/// ```
< ASCII >
///
/// We initially assume that all types are `ZeroSized` and then update our
/// understanding as we learn more about each type.
```
## Visual type:
- #flowchart


== ./chromium/chromium_398.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/clap/v3/crate/src/build/value_hint.rs#L3-L26

```c
/// Provide shell with hint on how to complete an argument.
///
/// See [Arg::value_hint][crate::Arg::value_hint] to set this on an argument.
///
/// See the `clap_complete` crate for completion script generation.
///
< ASCII >
/// Overview of which hints are supported by which shell:
///
/// | Hint                   | zsh | fish[^1]|
/// | ---------------------- | --- | ------- |
/// | `AnyPath`              | Yes | Yes     |
/// | `FilePath`             | Yes | Yes     |
/// | `DirPath`              | Yes | Yes     |
/// | `ExecutablePath`       | Yes | Partial |
/// | `CommandName`          | Yes | Yes     |
/// | `CommandString`        | Yes | Partial |
/// | `CommandWithArguments` | Yes |         |
/// | `Username`             | Yes | Yes     |
/// | `Hostname`             | Yes | Yes     |
/// | `Url`                  | Yes |         |
/// | `EmailAddress`         | Yes |         |
< ASCII >
///
/// [^1]: fish completions currently only support named arguments (e.g. -o or --opt), not
///       positional arguments.
```
## Visual type:
- #table


== ./chromium/chromium_399.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/clap/v4/crate/src/builder/value_hint.rs#L3-L26

```c
/// Provide shell with hint on how to complete an argument.
///
/// See [Arg::value_hint][crate::Arg::value_hint] to set this on an argument.
///
/// See the `clap_complete` crate for completion script generation.
///
< ASCII >
/// Overview of which hints are supported by which shell:
///
/// | Hint                   | zsh | fish[^1]|
/// | ---------------------- | --- | ------- |
/// | `AnyPath`              | Yes | Yes     |
/// | `FilePath`             | Yes | Yes     |
/// | `DirPath`              | Yes | Yes     |
/// | `ExecutablePath`       | Yes | Partial |
/// | `CommandName`          | Yes | Yes     |
/// | `CommandString`        | Yes | Partial |
/// | `CommandWithArguments` | Yes |         |
/// | `Username`             | Yes | Yes     |
/// | `Hostname`             | Yes | Yes     |
/// | `Url`                  | Yes |         |
/// | `EmailAddress`         | Yes |         |
< ASCII >
///
/// [^1]: fish completions currently only support named arguments (e.g. -o or --opt), not
///       positional arguments.
```
## Visual type:
- #table


== ./chromium/chromium_4.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/accessibility/ui/accessibility_focus_ring.h#L14-L71

```c
// An AccessibilityFocusRing is a special type of shape designed to
// outline the focused object on the screen for users with visual
// impairments. It's specifically designed to outline text ranges that
// span multiple lines (we'll call this a "paragraph" shape from here on,
// but it works for any text range), so it can outline a shape defined by a
// few words from the first line, the complete contents of more lines,
// followed by a few words from the last line. See the figure below.
// When highlighting any other object, it outlines a rectangular shape.
//
// The outline is outset from the object it's highlighting by a few pixels;
// this margin distance also determines its border radius for rounded
// corners.
//
// An AccessibilityFocusRing can be initialized with either a rectangle
// defining the bounds of an object, or a paragraph-shape with three
// rectangles defining a top line, a body, and a bottom line, which are
// assumed to be adjacent to one another.
//
// Initializing an AccessibilityFocusRing computes the following 36 points
// that completely define the shape's outline. This shape can be traced
// using Skia or any other drawing utility just by drawing alternating
// straight lines and quadratic curves (e.g. a line from 0 to 1, a curve
// from 1 to 3 with 2 as a control point, then a line from 3 to 4, and so on.
//
// The same path should be used even if the focus ring was initialized with
// a rectangle and not a paragraph shape - this makes it possible to
// smoothly animate between one object and the next simply by interpolating
// points.
//
// Noncontiguous shapes should be handled by drawing multiple focus rings.
//
< ASCII >
// The 36 points are defined as follows:
//
//          2 3------------------------------4 5
//           /                                |
//          1                                  6
//          |      First line of paragraph     |
//          0                                  7
//         /                                    |
// 32 33-34 35                                 8 9---------------10 11
//   /                                                             |
// 31      Middle line of paragraph..........................       12
// |                                                                |
// |                                                                |
// |       Middle line of paragraph..........................       |
// |                                                                |
// |                                                                |
// 30      Middle line of paragraph..........................       13
//   |                                                             |
// 29 28---------27 26                             17 16---------15 14
//                 |                                 |
//                  25                             18
//                  |    Last line of paragraph    |
//                  24                             19
//                    |                           |
//                  23 22-----------------------21 20
< ASCII >
//
// Exported for tests.
```
## Visual type:
- #custom


== ./chromium/chromium_40.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/base/index_rect.h#L14-L60

```c
// This class encapsulates the index boundaries for region on co-ordinate system
// (used for tiling). The delimiting boundaries |left_|, |right_|, |top_| and
// |bottom_| are basically leftmost, rightmost, topmost and bottommost indices
// of the region. These delimiters can span in any quadrants.
//
// If |left_| <= |right_| and |top_| <= |bottom_|, IndexRect is considered to
// hold valid indices and this can be checked using is_valid().
//
// If IndexRect is valid, it has a coverage of all the indices from |left_| to
// |right_| both inclusive and |top_| to |bottom_| both inclusive. So for
// |left_| == |right_|, num_indices_x() is 1, meaning |left_| and |right_| point
// to the same index.
//
// The following diagram shows how indices span in different quadrants and the
// positive quadrant. In the positive quadrant all indices are >= 0. The first
// index in this quadrant is (0, 0). The indices in positive quadrant represent
// the visible region and is_in_positive_quadrant() can be used to check whether
// all indices lie within this quadrant or not.
< ASCII >
//
//              │
//              │
//  -ve index_x │  +ve index_x
//  -ve index_y │  -ve index_y
//              │
//  ────────────┼────────────
//              │
//  -ve index_x │  +ve index_x
//  +ve index_y │  +ve index_y
//              │
//              │  (+ve Quadrant)
//
// In the following example, region has |left_| = 0, |right_| = 4, |top_| = 0
// and |bottom_| = 4. Here x indices are 0, 1, 2, 3, 4 and y indices are
// 0, 1, 2, 3, 4.
//
//    x 0   1   2   3   4
//  y ┌───┬───┬───┬───┬───┐
//  0 │   │   │   │   │   │
//    ├───┼───┼───┼───┼───┤
//  1 │   │   │   │   │   │
//    ├───┼───┼───┼───┼───┤
//  2 │   │   │   │   │   │
//    ├───┼───┼───┼───┼───┤
//  3 │   │   │   │   │   │
//    ├───┼───┼───┼───┼───┤
//  4 │   │   │   │   │   │
//    └───┴───┴───┴───┴───┘
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_400.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/codespan_reporting/v0_11/crate/src/term/renderer.rs#L65-L110

```c
/// A renderer of display list entries.
///
/// The following diagram gives an overview of each of the parts of the renderer's output:
///
/// ```text
< ASCII >
///                     ┌ outer gutter
///                     │ ┌ left border
///                     │ │ ┌ inner gutter
///                     │ │ │   ┌─────────────────────────── source ─────────────────────────────┐
///                     │ │ │   │                                                                │
///                  ┌────────────────────────────────────────────────────────────────────────────
///        header ── │ error[0001]: oh noes, a cupcake has occurred!
/// snippet start ── │    ┌─ test:9:0
/// snippet empty ── │    │
///  snippet line ── │  9 │   ╭ Cupcake ipsum dolor. Sit amet marshmallow topping cheesecake
///  snippet line ── │ 10 │   │ muffin. Halvah croissant candy canes bonbon candy. Apple pie jelly
///                  │    │ ╭─│─────────^
/// snippet break ── │    · │ │
///  snippet line ── │ 33 │ │ │ Muffin danish chocolate soufflé pastry icing bonbon oat cake.
///  snippet line ── │ 34 │ │ │ Powder cake jujubes oat cake. Lemon drops tootsie roll marshmallow
///                  │    │ │ ╰─────────────────────────────^ blah blah
/// snippet break ── │    · │
///  snippet line ── │ 38 │ │   Brownie lemon drops chocolate jelly-o candy canes. Danish marzipan
///  snippet line ── │ 39 │ │   jujubes soufflé carrot cake marshmallow tiramisu caramels candy canes.
///                  │    │ │           ^^^^^^^^^^^^^^^^^^^ -------------------- blah blah
///                  │    │ │           │
///                  │    │ │           blah blah
///                  │    │ │           note: this is a note
///  snippet line ── │ 40 │ │   Fruitcake jelly-o danish toffee. Tootsie roll pastry cheesecake
///  snippet line ── │ 41 │ │   soufflé marzipan. Chocolate bar oat cake jujubes lollipop pastry
///  snippet line ── │ 42 │ │   cupcake. Candy canes cupcake toffee gingerbread candy canes muffin
///                  │    │ │                                ^^^^^^^^^^^^^^^^^^ blah blah
///                  │    │ ╰──────────^ blah blah
/// snippet break ── │    ·
///  snippet line ── │ 82 │     gingerbread toffee chupa chups chupa chups jelly-o cotton candy.
///                  │    │                 ^^^^^^                         ------- blah blah
/// snippet empty ── │    │
///  snippet note ── │    = blah blah
///  snippet note ── │    = blah blah blah
///                  │      blah blah
///  snippet note ── │    = blah blah blah
///                  │      blah blah
///         empty ── │
< ASCII >
/// ```
///
/// Filler text from http://www.cupcakeipsum.com
```
## Visual type:
- #gui


== ./chromium/chromium_401.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/minimal_lexical/v0_2/crate/src/libm.rs#L999-L1064

```c
/* sqrt(x)
 * Return correctly rounded sqrt.
 *           ------------------------------------------
 *           |  Use the hardware sqrt if you have one |
 *           ------------------------------------------
 * Method:
 *   Bit by bit method using integer arithmetic. (Slow, but portable)
 *   1. Normalization
 *      Scale x to y in [1,4) with even powers of 2:
 *      find an integer k such that  1 <= (y=x*2^(2k)) < 4, then
 *              sqrt(x) = 2^k * sqrt(y)
 *   2. Bit by bit computation
 *      Let q  = sqrt(y) truncated to i bit after binary point (q = 1),
 *           i                                                   0
 *                                     i+1         2
 *          s  = 2*q , and      y  =  2   * ( y - q  ).         (1)
 *           i      i            i                 i
 *
 *      To compute q    from q , one checks whether
 *                  i+1       i
 *
 *                            -(i+1) 2
 *                      (q + 2      ) <= y.                     (2)
 *                        i
 *                                                            -(i+1)
 *      If (2) is false, then q   = q ; otherwise q   = q  + 2      .
 *                             i+1   i             i+1   i
 *
 *      With some algebraic manipulation, it is not difficult to see
 *      that (2) is equivalent to
 *                             -(i+1)
 *                      s  +  2       <= y                      (3)
 *                       i                i
 *
 *      The advantage of (3) is that s  and y  can be computed by
 *                                    i      i
 *      the following recurrence formula:
< ASCII >
 *          if (3) is false
 *
 *          s     =  s  ,       y    = y   ;                    (4)
 *           i+1      i          i+1    i
 *
 *          otherwise,
 *                         -i                     -(i+1)
 *          s     =  s  + 2  ,  y    = y  -  s  - 2             (5)
 *           i+1      i          i+1    i     i
 *
< ASCII >
 *      One may easily use induction to prove (4) and (5).
 *      Note. Since the left hand side of (3) contain only i+2 bits,
 *            it does not necessary to do a full (53-bit) comparison
 *            in (3).
 *   3. Final rounding
 *      After generating the 53 bits result, we compute one more bit.
 *      Together with the remainder, we can decide whether the
 *      result is exact, bigger than 1/2ulp, or less than 1/2ulp
 *      (it will never equal to 1/2ulp).
 *      The rounding mode can be detected by checking whether
 *      huge + tiny is equal to huge, and whether huge - tiny is
 *      equal to huge for some floating point number "huge" and "tiny".
 *
 * Special cases:
 *      sqrt(+-0) = +-0         ... exact
 *      sqrt(inf) = inf
 *      sqrt(-ve) = NaN         ... with invalid signal
 *      sqrt(NaN) = NaN         ... with invalid signal for signaling NaN
 */
```
## Visual type:
- #formula 


== ./chromium/chromium_402.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/third_party/rust/semver/v1/crate/src/identifier.rs#L1-L67

```c
// This module implements Identifier, a short-optimized string allowed to
// contain only the ASCII characters hyphen, dot, 0-9, A-Z, a-z.
//
< ASCII >
// As of mid-2021, the distribution of pre-release lengths on crates.io is:
//
//     length  count         length  count         length  count
//        0  355929            11      81            24       2
//        1     208            12      48            25       6
//        2     236            13      55            26      10
//        3    1909            14      25            27       4
//        4    1284            15      15            28       1
//        5    1742            16      35            30       1
//        6    3440            17       9            31       5
//        7    5624            18       6            32       1
//        8    1321            19      12            36       2
//        9     179            20       2            37     379
//       10      65            23      11
//
// and the distribution of build metadata lengths is:
//
//     length  count         length  count         length  count
//        0  364445             8    7725            18       1
//        1      72             9      16            19       1
//        2       7            10      85            20       1
//        3      28            11      17            22       4
//        4       9            12      10            26       1
//        5      68            13       9            27       1
//        6      73            14      10            40       5
//        7      53            15       6
< ASCII >
//
// Therefore it really behooves us to be able to use the entire 8 bytes of a
// pointer for inline storage. For both pre-release and build metadata there are
// vastly more strings with length exactly 8 bytes than the sum over all lengths
// longer than 8 bytes.
//
// To differentiate the inline representation from the heap allocated long
// representation, we'll allocate heap pointers with 2-byte alignment so that
// they are guaranteed to have an unset least significant bit. Then in the repr
// we store for pointers, we rotate a 1 into the most significant bit of the
// most significant byte, which is never set for an ASCII byte.
//
< ASCII >
// Inline repr:
//
//     0xxxxxxx 0xxxxxxx 0xxxxxxx 0xxxxxxx 0xxxxxxx 0xxxxxxx 0xxxxxxx 0xxxxxxx
//
// Heap allocated repr:
//
//     1ppppppp pppppppp pppppppp pppppppp pppppppp pppppppp pppppppp pppppppp 0
//     ^ most significant bit   least significant bit of orig ptr, rotated out ^
< ASCII >
//
// Since the most significant bit doubles as a sign bit for the similarly sized
// signed integer type, the CPU has an efficient instruction for inspecting it,
// meaning we can differentiate between an inline repr and a heap allocated repr
// in one instruction. Effectively an inline repr always looks like a positive
// i64 while a heap allocated repr always looks like a negative i64.
//
// For the inline repr, we store \0 padding on the end of the stored characters,
// and thus the string length is readily determined efficiently by a cttz (count
// trailing zeros) or bsf (bit scan forward) instruction.
//
// For the heap allocated repr, the length is encoded as a base-128 varint at
// the head of the allocation.
//
// Empty strings are stored as an all-1 bit pattern, corresponding to -1i64.
// Consequently the all-0 bit pattern is never a legal representation in any
// repr, leaving it available as a niche for downstream code. For example this
// allows size_of::<Version>() == size_of::<Option<Version>>().
```
## Visual type:
- #memory-layout


== ./chromium/chromium_403.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/aura/window_occlusion_tracker_unittest.cc#L152-L155

```c
< ASCII >
// Verify that non-overlapping windows have a VISIBLE occlusion state.
// _____  _____
// |    | |    |
// |____| |____|
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_404.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/display_layout.cc#L655-L684

```c
// Creates a display::DisplayPlacement value for |rectangle| relative to the
// |reference| rectangle.
// The layout consists of two values:
//   - position: Whether the rectangle is positioned left, right, over or under
//     the reference.
//   - offset: The rectangle's offset from the reference origin along the axis
//     opposite the position direction (if the rectangle is left or right along
//     y-axis, otherwise along x-axis).
// The rectangle's position is calculated by dividing the space in areas defined
// by the |reference|'s diagonals and finding the area |rectangle|'s center
// point belongs. If the |rectangle| in the calculated layout does not share a
// part of the bounds with the |reference|, the |rectangle| position in set to
// the more suitable neighboring position (e.g. if |rectangle| is completely
// over the |reference| top bound, it will be set to TOP) and the layout is
// recalculated with the new position. This is to handle the case where the
// rectangle shares an edge with the reference, but it's center is not in the
// same area as the reference's edge, e.g.
< ASCII >
//
// +---------------------+
// |                     |
// | REFERENCE           |
// |                     |
// |                     |
// +---------------------+
//                 +-------------------------------------------------+
//                 | RECTANGLE               x                       |
//                 +-------------------------------------------------+
< ASCII >
//
// The rectangle shares an edge with the reference's bottom edge, but its
// center point is in the left area.
```
## Visual type:
- #custom


== ./chromium/chromium_405.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/display_layout.h#L30-L42

```c
// DisplayPlacement specifies where the display (D) is placed relative to
// parent (P) display. In the following example, D given by |display_id| is
// placed at the left side of P given by |parent_display_id|, with a negative
// offset and a top-left offset reference.
< ASCII >
//
//        +      +--------+
// offset |      |        |
//        +      |   D    +--------+
//               |        |        |
//               +--------+   P    |
//                        |        |
//                        +--------+
< ASCII >
//
```
## Visual type:
- #custom


== ./chromium/chromium_406.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/win/scaling_util.h#L25-L105

```c
// Returns a DisplayPlacement for |current| relative to |parent|.
// Note that DisplayPlacement's are always in DIPs, so this also performs the
// required scaling.
//
// Examples (The offset is indicated by the arrow.):
// Scaled and Unscaled Coordinates
< ASCII >
// +--------------+    +          Since both DisplayInfos are of the same scale
// |              |    |          factor, relative positions remain the same.
// |    Parent    |    V
// |      1x      +----------+
// |              |          |
// +--------------+  Current |
//                |    1x    |
//                +----------+
//
// Unscaled Coordinates
// +--------------+               The 2x DisplayInfo is offset to maintain a
// |              |               similar neighboring relationship with the 1x
// |    Parent    |               parent. Current's position is based off of the
// |      1x      +----------+    percentage position along its parent. This
// |              |          |    percentage position is preserved in the scaled
// +--------------+  Current |    coordinates.
//                |    2x    |
//                +----------+
// Scaled Coordinates
// +--------------+  +
// |              |  |
// |    Parent    |  V
// |      1x      +-----+
// |              + C 2x|
// +--------------+-----+
//
//
// Unscaled Coordinates
// +--------------+               The parent DisplayInfo has a 2x scale factor.
// |              |               The offset is adjusted to maintain the
// |              |               relative positioning of the 1x DisplayInfo in
// |    Parent    +----------+    the scaled coordinate space. Current's
// |      2x      |          |    position is based off of the percentage
// |              |  Current |    position along its parent. This percentage
// |              |    1x    |    position is preserved in the scaled
// +--------------+          |    coordinates.
//                |          |
//                +----------+
// Scaled Coordinates
// +-------+    +
// |       |    V
// | Parent+----------+
// |   2x  |          |
// +-------+  Current |
//         |    1x    |
//         |          |
//         |          |
//         +----------+
//
// Unscaled Coordinates
//         +----------+           In this case, parent lies between the top and
//         |          |           bottom of parent. The roles are reversed when
// +-------+          |           this occurs, and current is placed to maintain
// |       |  Current |           parent's relative position along current.
// | Parent|    1x    |
// |   2x  |          |
// +-------+          |
//         +----------+
// Scaled Coordinates
//  ^      +----------+
//  |      |          |
//  + +----+          |
//    |Prnt|  Current |
//    | 2x |    1x    |
//    +----+          |
//         |          |
//         +----------+
//
// Scaled and Unscaled Coordinates
// +--------+                     If the two DisplayInfos are bottom aligned or
// |        |                     right aligned, the DisplayPlacement will
// |        +--------+            have an offset of 0 relative to the
// |        |        |            bottom-right of the DisplayInfo.
// |        |        |
// +--------+--------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_407.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/win/scaling_util.h#L110-L142

```c
// Returns the squared distance between two rects.
// The distance between two rects is the length of the shortest segment that can
// be drawn between two rectangles. This segment generally connects two opposing
// corners between rectangles like this...
//
< ASCII >
// +----------+
// |          |
// +----------+
//             \  <--- Shortest Segment
//              \
//               +---+
//               |   |
//               |   |
//               +---+
//
// For rectangles that share coordinates within the same axis, that generally
// means the segment is parallel to the axis and perpendicular to the edges.
//
//                 One of many shortest segments
//  +----------+  /                            \    +--------+
//  |          |  |                             \   |        |
//  |          |  V  +---+                       \  +--------+
//  |          |-----|   |                        \-->|
//  +----------+     |   |                            +----+
//                   |   |                            |    |
//                   +---+                            +----+
< ASCII >
//
// For rectangles that intersect each other, the distance is the negative value
// of the overlapping area, so callers can distinguish different amounts of
// overlap.
//
// The squared distance is used to avoid taking the square root as the common
// usage is to compare distances greater than 1 unit.
```
## Visual type:
- #custom


== ./chromium/chromium_408.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/win/screen_win_unittest.cc#L1440-L1453

```c
< ASCII >
// Five 1x displays laid out as follows (not to scale):
// +---------+----------------+
// |         |                |
// |    0    |                |
// |         |       1        |
// +---------+                |
// |    2    |                |
// |         +----------------+-----+
// +---------+                |     |
//                            |  3  |
//                            |     |
//                            +--+--+
//                               |4 |
//                               +--+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_409.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/display/win/screen_win_unittest.cc#L1876-L1889

```c
< ASCII >
// Five 2x displays laid out as follows (not to scale):
// +---------+----------------+
// |         |                |
// |    0    |                |
// |         |       1        |
// +---------+                |
// |    2    |                |
// |         +----------------+-----+
// +---------+                |     |
//                            |  3  |
//                            |     |
//                            +--+--+
//                               |4 |
//                               +--+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_41.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/base/spiral_iterator.h#L13-L28

```c
// The spiral iterator which iterates based on directions around the center
// rect in the given region. If the center rect is at index (2, 2), spiral
// iterator gives following sequence on iterating.
< ASCII >
//
//    x 0   1   2   3   4
//  y ┌───┬───┬───┬───┬───┐
//  0 │ 16│ 15│ 14│ 13│ 12│
//    ├───┼───┼───┼───┼───┤
//  1 │ 17│  4│  3│  2│ 11│
//    ├───┼───┼───┼───┼───┤
//  2 │ 18│  5│  *│  1│ 10│
//    ├───┼───┼───┼───┼───┤
//  3 │ 19│  6│  7│  8│  9│
//    ├───┼───┼───┼───┼───┤
//  4 │ 20│ 21│ 22│ 23│ 24│
//    └───┴───┴───┴───┴───┘
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_410.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/gfx/text_constants.h#L97-L107

```c
// Text baseline offset types.
< ASCII >
// Figure of font metrics:
//   +--------+--------+------------------------+-------------+
//   |        |        | internal leading       | SUPERSCRIPT |
//   |        |        +------------+-----------|             |
//   |        | ascent |            | SUPERIOR  |-------------+
//   | height |        | cap height |-----------|
//   |        |        |            | INFERIOR  |-------------+
//   |        |--------+------------+-----------|             |
//   |        | descent                         | SUBSCRIPT   |
//   +--------+---------------------------------+-------------+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_411.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/gfx/text_utils.h#L74-L100

```c
// Returns insets that can be used to draw a highlight or border that appears to
// be distance |desired_visual_padding| from the body of a string of text
// rendered using |font_list|. The insets are adjusted based on the box used to
// render capital letters (or the bodies of most letters in non-capital fonts
// like Hebrew and Devanagari), in order to give the best visual appearance.
//
// That is, any portion of |desired_visual_padding| overlapping the font's
// leading space or descender area are truncated, to a minimum of zero.
//
// In this example, the text is rendered in a highlight that stretches above and
// below the height of the H as well as to the left and right of the text
// (|desired_visual_padding| = {2, 2, 2, 2}). Note that the descender of the 'y'
// overlaps with the padding, as it is outside the capital letter box.
//
// The resulting padding is {1, 2, 1, 2}.
< ASCII >
//
//  . . . . . . . . . .                               | actual top
//  .                 .  |              | leading space
//  .  |  |  _        .  | font    | capital
//  .  |--| /_\ \  /  .  | height  | height
//  .  |  | \_   \/   .  |         |
//  .            /    .  |              | descender
//  . . . . . . . . . .                               | actual bottom
//  ___             ___
//  actual        actual
//  left           right
< ASCII >
//
AdjustVisualBorderForFont(const FontList& font_list,
                          const Insets& desired_visual_padding);
```
## Visual type:
- #custom


== ./chromium/chromium_412.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/gfx/geometry/matrix44.h#L17-L35

```c
// This is the underlying data structure of Transform. Don't use this type
// directly.
//
// Throughout this class, we will be speaking in column vector convention.
// i.e. Applying a transform T to vector V is T * V.
< ASCII >
// The components of the matrix and the vector look like:
//    \  col
// r   \     0        1        2        3
// o  0 | scale_x  skew_xy  skew_xz  trans_x |   | x |
// w  1 | skew_yx  scale_y  skew_yz  trans_y | * | y |
//    2 | skew_zx  skew_zy  scale_z  trans_z |   | z |
//    3 | persp_x  persp_y  persp_z  persp_w |   | w |
< ASCII >
//
// Note that the names are just for remembering and don't have the exact
// meanings when other components exist.
//
// The components correspond to the DOMMatrix mij (i,j = 1..4) components:
//   i = col + 1
//   j = row + 1
```
## Visual type:
- #matrix 


== ./chromium/chromium_413.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/native_theme/native_theme_win.cc#L90-L100

```c
< ASCII >
//    <-a->
// [  *****             ]
//  ____ |              |
//  <-a-> <------b----->
< ASCII >
// a: object_width
// b: frame_width
// *: animating object
//
// - the animation goes from "[" to "]" repeatedly.
// - the animation offset is at first "|"
//
```
## Visual type:
- #custom


== ./chromium/chromium_414.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/ozone/platform/drm/gpu/hardware_display_controller.h#L40-L92

```c
// The HDC will handle modesetting and scanout operations for hardware devices.
//
// In the DRM world there are 3 components that need to be paired up to be able
// to display an image to the monitor: CRTC (cathode ray tube controller),
// encoder and connector. The CRTC determines which framebuffer to read, when
// to scanout and where to scanout. Encoders converts the stream from the CRTC
// to the appropriate format for the connector. The connector is the physical
// connection that monitors connect to.
//
// There is no 1:1:1 pairing for these components. It is possible for an encoder
// to be compatible to multiple CRTCs and each connector can be used with
// multiple encoders. In addition, it is possible to use one CRTC with multiple
// connectors such that we can display the same image on multiple monitors.
//
< ASCII >
// For example, the following configuration shows 2 different screens being
// initialized separately.
// -------------      -------------
// | Connector |      | Connector |
// |   HDMI    |      |    VGA    |
// -------------      -------------
//       ^                  ^
//       |                  |
// -------------      -------------
// |  Encoder1  |     |  Encoder2 |
// -------------      -------------
//       ^                  ^
//       |                  |
// -------------      -------------
// |   CRTC1   |      |   CRTC2   |
// -------------      -------------
//
// In the following configuration 2 different screens are associated with the
// same CRTC, so on scanout the same framebuffer will be displayed on both
// monitors.
// -------------      -------------
// | Connector |      | Connector |
// |   HDMI    |      |    VGA    |
// -------------      -------------
//       ^                  ^
//       |                  |
// -------------      -------------
// |  Encoder1  |     |  Encoder2 |
// -------------      -------------
//       ^                  ^
//       |                  |
//      ----------------------
//      |        CRTC1       |
//      ----------------------
< ASCII >
//
// Note that it is possible to have more connectors than CRTCs which means that
// only a subset of connectors can be active independently, showing different
// framebuffers. Though, in this case, it would be possible to have all
// connectors active if some use the same CRTC to mirror the display.
```
## Visual type:
- #flowchart


== ./chromium/chromium_415.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/paint_info_unittest.cc#L34-L42

```c
< ASCII >
//  ___________
// |     1     |
// |___________|
// | 3 | 4 | 5 | <-- 2 (encapsulates 3, 4 and 5)
// |___|___|___|
// |   7   | 8 | <-- 6 (encapsulates 7 and 8)
// |_______|___|
//
// |r_0| encapsulates 1, 2 and 6.
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_416.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/view_unittest.cc#L3212-L3219

```c
// Verifies that the ViewHierarchyChanged() notification is sent correctly when
// a child view is added or removed to all the views in the hierarchy (up and
// down).
< ASCII >
// The tree looks like this:
// v1
// +-- v2(view2)
//     +-- v3
//     +-- v4 (starts here, then get reparented to v1)
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_417.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/view_unittest.cc#L3485-L3495

```c
// Verifies if the child views added under the root are all deleted when calling
// RemoveAllChildViews.
< ASCII >
// The tree looks like this:
// root
// +-- child1
//     +-- foo
//         +-- bar0
//         +-- bar1
//         +-- bar2
// +-- child2
// +-- child3
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_418.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/view_unittest.cc#L3627-L3633

```c
// Verifies that GetViewByID returns the correctly child view from the specified
// ID.
< ASCII >
// The tree looks like this:
// v1
// +-- v2
//     +-- v3
//     +-- v4
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_419.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/view_unittest_aura.cc#L60-L74

```c
// Test that wm::RecreateLayers() recreates the layers for all child windows and
// all child views and that the z-order of the recreated layers matches that of
// the original layers.
< ASCII >
// Test hierarchy:
// w1
// +-- v1
// +-- v2 (no layer)
//     +-- v3 (no layer)
//     +-- v4
// +-- w2
//     +-- v5
//         +-- v6
// +-- v7
//     +-- v8
//     +-- v9
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_42.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/paint/discardable_image_map_unittest.cc#L400-L413

```c
// Test if SaveLayer and Restore work together.
// 1. Move cursor to (25, 25) draw a black rect of size 25x25.
// 2. save layer, move the cursor by (100, 100) or to point (125, 125), draw a
// red rect of size 25x25.
// 3. Restore layer, so the cursor moved back to (25, 25), move cursor by (100,
// 0) or at the point (125, 25), draw a yellow rect of size 25x25.
< ASCII >
//  (25, 25)
//  +---+
//  |   |
//  +---+
//  (25, 125) (125, 125)
//  +---+     +---+
//  |   |     |   |
//  +---+     +---+
< ASCII >
```
## Visual type:
- #custom


== ./chromium/chromium_420.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L616-L617

```c
< ASCII >
// Opacity        -->|
// RoundedCorners ----->|
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_421.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L647-L648

```c
< ASCII >
// Opacity        ----->|
// RoundedCorners    ----->|
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_422.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L682-L683

```c
< ASCII >
// Opacity        ----->|---|-->|
// RoundedCorners     ----->|-->|
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_423.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L752-L752

```c
< ASCII >
// Opacity [-->|-->|   ]
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_424.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L785-L786

```c
< ASCII >
// Opacity        [-->|-->]
// RoundedCorners [-->|-->]
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_425.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L821-L822

```c
< ASCII >
// Opacity        -->|-->|
// RoundedCorners   -->|-->|
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_426.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L867-L868

```c
< ASCII >
// Opacity        -->|-->|
// RoundedCorners   -->|
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_427.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/animation/animation_builder_unittest.cc#L906-L907

```c
< ASCII >
// Opacity        [-->|-->  ]
// RoundedCorners [-->|---->]
< ASCII >
```
## Visual type:
- #custom 


== ./chromium/chromium_428.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ui/views/window/non_client_view.h#L120-L150

```c
////////////////////////////////////////////////////////////////////////////////
// NonClientView
//
//  The NonClientView is the logical root of all Views contained within a
//  Window, except for the RootView which is its parent and of which it is the
//  sole child. The NonClientView has one child, the NonClientFrameView which
//  is responsible for painting and responding to events from the non-client
//  portions of the window, and for forwarding events to its child, the
//  ClientView, which is responsible for the same for the client area of the
//  window:
< ASCII >
//
//  +- views::Widget ------------------------------------+
//  | +- views::RootView ------------------------------+ |
//  | | +- views::NonClientView ---------------------+ | |
//  | | | +- views::NonClientFrameView subclass ---+ | | |
//  | | | |                                        | | | |
//  | | | | << all painting and event receiving >> | | | |
//  | | | | << of the non-client areas of a     >> | | | |
//  | | | | << views::Widget.                   >> | | | |
//  | | | |                                        | | | |
//  | | | | +- views::ClientView or subclass ----+ | | | |
//  | | | | |                                    | | | | |
//  | | | | | << all painting and event       >> | | | | |
//  | | | | | << receiving of the client      >> | | | | |
//  | | | | | << areas of a views::Widget.    >> | | | | |
//  | | | | +------------------------------------+ | | | |
//  | | | +----------------------------------------+ | | |
//  | | +--------------------------------------------+ | |
//  | +------------------------------------------------+ |
//  +----------------------------------------------------+
//
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_429.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/url/url_canon_host.cc#L14-L39

```c
// For reference, here's what IE supports:
// Key: 0 (disallowed: failure if present in the input)
//      + (allowed either escaped or unescaped, and unmodified)
//      U (allowed escaped or unescaped but always unescaped if present in
//         escaped form)
//      E (allowed escaped or unescaped but always escaped if present in
//         unescaped form)
//      % (only allowed escaped in the input, will be unmodified).
//      I left blank alpha numeric characters.
< ASCII >
//
//    00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
//    -----------------------------------------------
// 0   0  E  E  E  E  E  E  E  E  E  E  E  E  E  E  E
// 1   E  E  E  E  E  E  E  E  E  E  E  E  E  E  E  E
// 2   E  +  E  E  +  E  +  +  +  +  +  +  +  U  U  0
// 3                                 %  %  E  +  E  0  <-- Those are  : ; < = > ?
// 4   %
// 5                                    U  0  U  U  U  <-- Those are  [ \ ] ^ _
// 6   E                                               <-- That's  `
// 7                                    E  E  E  U  E  <-- Those are { | } ~ (UNPRINTABLE)
< ASCII >
//
// NOTE: I didn't actually test all the control characters. Some may be
// disallowed in the input, but they are all accepted escaped except for 0.
// I also didn't test if characters affecting HTML parsing are allowed
// unescaped, e.g. (") or (#), which would indicate the beginning of the path.
// Surprisingly, space is accepted in the input and always escaped.
```
## Visual type:
- #table


== ./chromium/chromium_43.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/raster/task.h#L20-L40

```c
// This class provides states to manage life cycle of a task and given below is
// how it is used by TaskGraphWorkQueue to process life cycle of a task.
// Task is in NEW state when it is created. When task is added to
// |ready_to_run_tasks| then its state is changed to SCHEDULED. Task can be
// canceled from NEW state (not yet scheduled to run) or from SCHEDULED state,
// when new ScheduleTasks() is triggered and its state is changed to CANCELED.
// When task is about to run it is added |running_tasks| and its state is
// changed to RUNNING. Once task finishes running, its state is changed to
// FINISHED. Both CANCELED and FINISHED tasks are added to |completed_tasks|.
< ASCII >
//                ╔═════╗
//         +------║ NEW ║------+
//         |      ╚═════╝      |
//         v                   v
//   ┌───────────┐        ╔══════════╗
//   │ SCHEDULED │------> ║ CANCELED ║
//   └───────────┘        ╚══════════╝
//         |
//         v
//    ┌─────────┐         ╔══════════╗
//    │ RUNNING │-------> ║ FINISHED ║
//    └─────────┘         ╚══════════╝
< ASCII >
```
## Visual type:
- #state-machine 


== ./chromium/chromium_44.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/tiles/gpu_image_decode_cache.cc#L106-L113

```c
< ASCII >
// lock_count │ used  │ result state
// ═══════════╪═══════╪══════════════════
//  1         │ false │ WASTED_ONCE
//  1         │ true  │ USED_ONCE
//  >1        │ false │ WASTED_RELOCKED
//  >1        │ true  │ USED_RELOCKED
< ASCII >
// Note that it's important not to reorder the following enum, since the
// numerical values are used in the histogram code.
```
## Visual type:
- #table


== ./chromium/chromium_45.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/trees/layer_tree_host_impl_unittest.cc#L2044-L2063

```c
// Tests the following tricky case:
// - Scrolling Layer A with scrolling children:
//    - Ordinary Layer B with NonFastScrollableRegion
//    - Ordinary Layer C
//
< ASCII >
//                   +---------+
//         +---------|         |+
//         | Layer A |         ||
//         |   +-----+-----+   ||
//         |   |  Layer C  |   ||
//         |   +-----+-----+   ||
//         |         | Layer B ||
//         +---------|         |+
//                   +---------+
< ASCII >
//
//
// Both B and C scroll with A but overlap each other and C appears above B. If
// we try scrolling over C, we need to check if we intersect the NFSR on B
// because C may not be fully opaque to hit testing (e.g. the layer may be for
// |pointer-events:none| or be a squashing layer with "holes").
```
## Visual type:
- #custom


== ./chromium/chromium_46.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/trees/layer_tree_host_impl_unittest.cc#L2160-L2190

```c
// Similar to the above test but this time layer B does not scroll with layer
// A. This is an edge case where the CSS painting algorithm allows a sibling of
// an overflow scroller to appear on top of the scroller itself but below some
// of the scroller's children. e.g. https://output.jsbin.com/tejulip/quiet.
//
// <div id="scroller" style="position:relative">
//   <div id="child" style="position:relative; z-index:2"></div>
// </div>
// <div id="sibling" style="position:absolute; z-index: 1"></div>
//
// So we setup:
//
// - Scrolling Layer A with scrolling child:
//    - Ordinary Layer C
// - Ordinary layer B with a NonFastScrollableRegion
< ASCII >
//
//                   +---------+
//         +---------|         |+
//         | Layer A |         ||
//         |   +-----+-----+   ||
//         |   |  Layer C  |   ||
//         |   +-----+-----+   ||
//         |         | Layer B ||
//         +---------|         |+
//                   +---------+
< ASCII >
//
//
// Only C scrolls with A but C appears above B. If we try scrolling over C, we
// need to check if we intersect the NFSR on B because C may not be fully
// opaque to hit testing (e.g. the layer may be for |pointer-events:none| or be
// a squashing layer with "holes").
```
## Visual type:
- #custom


== ./chromium/chromium_47.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/trees/layer_tree_host_impl_unittest.cc#L2291-L2309

```c
// - Scrolling Layer A with scrolling child:
//    - Ordinary Layer B with NonFastScrollableRegion
// - Fixed (scrolls with inner viewport) ordinary Layer C.
< ASCII >
//
//         +---------+---------++
//         | Layer A |         ||
//         |   +-----+-----+   ||
//         |   |  Layer C  |   ||
//         |   +-----+-----+   ||
//         |         | Layer B ||
//         +---------+---------++
< ASCII >
//
//
// B scrolls with A but C, which is fixed, appears above B. If we try scrolling
// over C, we need to check if we intersect the NFSR on B because C may not be
// fully opaque to hit testing (e.g. the layer may be for |pointer-events:none|
// or be a squashing layer with "holes"). This is similar to the cases above
// but uses a fixed Layer C to exercise the case where we hit the viewport via
// the inner viewport.
```
## Visual type:
- #custom


== ./chromium/chromium_48.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/cc/trees/layer_tree_host_impl_unittest.cc#L2407-L2423

```c
// - Scrolling Layer A with scrolling child:
//    - Ordinary Layer B
// - Fixed (scrolls with inner viewport) ordinary Layer C.
< ASCII >
//
//         +---------+---------++
//         | Layer A |         ||
//         |   +-----+-----+   ||
//         |   |  Layer C  |   ||
//         |   +-----+-----+   ||
//         |         | Layer B ||
//         +---------+---------++
< ASCII >
//
//  This test simply ensures that a scroll over the region where layer C and
//  layer B overlap can be handled on the compositor thread. Both of these
//  layers have the viewport as the first scrolling ancestor but C has the
//  inner viewport while B has the outer viewport as an ancestor. Ensure we
//  treat these as equivalent.
```
## Visual type:
- #custom


== ./chromium/chromium_49.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/android/java/src/org/chromium/chrome/browser/autofill/prefeditor/EditorLabelField.java#L19-L35

```c
/**
< ASCII >
 * Helper class for creating a view with three labels and an icon.
 *
 * +--------------+------------+
 * | TOP LABEL    |            |
 * | MID LABEL    |       ICON |
 * | BOTTOM LABEL |            |
 * +--------------+------------+
 *
 * Used for showing the uneditable parts of server cards. For example:
 *
 * +--------------+------------+
 * | Visa***1234  |            |
 * | First Last   |       VISA |
 * | Exp: 12/2020 |            |
 * +--------------+------------+
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_5.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/accessibility/ui/accessibility_focus_ring_group.cc#L229-L263

```c
// Given a vector of rects that all overlap, already sorted from top to bottom
// and left to right, split them into three shapes covering the top, middle,
// and bottom of a "paragraph shape".
//
< ASCII >
// Input:
//
//                       +---+---+
//                       | 1 | 2 |
// +---------------------+---+---+
// |             3               |
// +--------+---------------+----+
// |    4   |         5     |
// +--------+---------------+--+
// |             6             |
// +---------+-----------------+
// |    7    |
// +---------+
//
// Output:
//
//                       +-------+
//                       |  Top  |
// +---------------------+-------+
// |                             |
// |                             |
// |           Middle            |
// |                             |
// |                             |
// +---------+-------------------+
// | Bottom  |
// +---------+
< ASCII >
//
// When there's no clear "top" or "bottom" segment, split the overall rect
// evenly so that some of the area still fits into the "top" and "bottom"
// segments.
```
## Visual type:
- #custom


== ./chromium/chromium_50.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/android/java/src/org/chromium/chrome/browser/autofill/prefeditor/HintedDropDownAdapter.java#L19-L35

```c
/**
 * Subclass of DropdownFieldAdapter used to display a hinted dropdown. The hint will be used when no
 * element is selected.
 *
 * @param <T> The type of element to be inserted into the adapter.
 *
< ASCII >
 *
 * collapsed view:       --------          Expanded view:   ------------
 *                                                         . hint       .
 *                                                         ..............
 * (no item selected)   | hint   |                         | option 1   |
 *                       --------                          |------------|
 *                                                         | option 2   |
 * collapsed view:       ----------                        |------------|
 * (with selected item) | option X |                       .    ...     .
 *                       ----------                        .------------.
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_51.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/android/java/src/org/chromium/chrome/browser/autofill/prefeditor/HintedDropDownAdapterWithPlusIcon.java#L22-L39

```c
/**
 * Subclass of HintedDropDownAdapter used to display a hinted dropdown . The last
 * displayed element on the list will have a "+" icon on its left and a blue tint to indicate that
 * the option is for adding an element.
 *
 * @param <T> The type of element to be inserted into the adapter.
 *
 *
< ASCII >
 * collapsed view:       --------          Expanded view:   ------------
 *                                                         . hint       .
 *                                                         ..............
 * (no item selected)   | hint   |                         | option 1   |
 *                       --------                          |------------|
 *                                                         | option 2   |
 * collapsed view:       ----------                        |------------|
 * (with selected item) | option X |                       | + option N | -> stylized and "+" icon
 *                       ----------                        .------------.
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_52.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/certificate_manager_model.cc#L54-L74

```c
// CertificateManagerModel is created on the UI thread. It needs a
// NSSCertDatabase handle (and on ChromeOS it needs to get the TPM status) which
// needs to be done on the IO thread.
//
< ASCII >
// The initialization flow is roughly:
//
//               UI thread                              IO Thread
//
//   CertificateManagerModel::Create
//                  \--------------------------------------v
//                                CertificateManagerModel::GetCertDBOnIOThread
//                                                         |
//                                               NssCertDatabaseGetter
//                                                         |
//                               CertificateManagerModel::DidGetCertDBOnIOThread
//                  v--------------------------------------/
// CertificateManagerModel::DidGetCertDBOnUIThread
//                  |
//     new CertificateManagerModel
//                  |
//               callback
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_53.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/android/examples/partner_browser_customizations_provider/src/org/chromium/example/partnercustomizations/PartnerBookmarksProviderExample.java#L33-L55

```c
/**
 * Default partner bookmarks provider implementation of {@link PartnerBookmarksContract} API.
 * It reads the flat list of bookmarks and the name of the root partner
 * bookmarks folder using getResources() API.
 *
< ASCII >
 * Sample resources structure:
 *     res/
 *         values/
 *             strings.xml
 *                  string name="bookmarks_folder_name"
 *                  string-array name="bookmarks"
 *                      item TITLE1
 *                      item URL1
 *                      item TITLE2
 *                      item URL2...
 *             bookmarks_icons.xml
 *                  array name="bookmark_preloads"
 *                      item @raw/favicon1
 *                      item @raw/touchicon1
 *                      item @raw/favicon2
 *                      item @raw/touchicon2
 *                      ...
< ASCII >
 */
```
## Visual type:
- #tree


== ./chromium/chromium_54.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/app_list/app_list_notifier_impl.h#L28-L127

```c
// This AppListNotifier subclass is only used for the productivity launcher.
//
// Chrome implementation of the AppListNotifier. This is mainly responsible for
// translating notifications about launcher UI events - eg. launcher opened,
// results changed, etc. - into impression, launch, and abandon notifications
// for observers. See the header comment in app_list_notifier.h for definitions
// of these.
//
// This handles results on each search result view, which are are just called
// _views_ in this comment.
//
// State machine
// =============
//
// This class implements N state machines, one for each view, that are mostly
// independent. Each state machine can be in one of three primary states:
//
//  - kNone: the view is not displayed.
//  - kShown: the view is displayed.
//  - kSeen: the same results on the view have been displayed for a certain
//           amount of time.
//
// There are two extra 'transitional' states:
//
//  - kLaunched: a user has launched a result in this surface.
//  - kIgnored: a user has launched a result in another visible surface.
//
// These states exist only to simplify implementation, and the state machine
// doesn't stay in them for any length of time. Immediately after a transition
// to kLaunch or kIgnore, the launcher closes and the state is set to kNone.
//
// Various user and background events cause _transitions_ between states. The
// notifier performs _actions_ on a transition based on the (from, to) pair of
// states.
//
// Each state machine is associated with an _impression timer_ that begins on a
// transition to kShown. Once the timer finishes, it causes a transition to
// kSeen.
//
// Transitions
// ===========
//
// The events that cause a transition to a state are as follows.
//
//  - kNone: closing the launcher or moving to a different view.
//  - kShown: opening the relevant view, or changing the search query if the
//            view is the app tiles or results list.
//  - kLaunched: launching a result.
//  - kSeen: the impression timer finishing for the view.
//
// Actions
// =======
//
< ASCII >
// These actions are triggered on a state transition.
//
//  From -> To         | Actions
//  -------------------|--------------------------------------------------------
//  kNone -> kNone     | No action.
//                     |
//  kNone -> kShown    | Start impression timer, as view just displayed.
//                     |
//  kShown -> kNone    | Cancel impression timer, as view changed.
//                     |
//  kShown -> kSeen    | Notify of an impression, as impression timer finished.
//                     |
//  kShown -> kShown   | Restart impression timer. Should only be triggered for
//                     | the list view, when the displayed results change.
//                     |
//                     |
//  kSeen -> kLaunch   | Notify of a launch and immediately set state to kNone,
//                     | as user launched a result.
//                     |
//  kShown -> kLaunch  | Notify of a launch and impression then set state to
//                     | kNone and cancel impression timer, as user launched
//                     | result so must have seen the results.
//                     |
//                     |
//  kShown -> kIgnored | Notify of an ignore and impression, then set state to
//                     | kNone and cancel timer, as user launched a result in
//                     | a different view to must have seen the results.
//                     |
//  kSeen -> kIgnored  | Notify of an ignore, then set state to kNone, as user
//                     | launched a result in a different view.
//                     |
//                     |
//  kSeen -> kNone     | Notify of an abandon, as user closed the launcher.
//                     |
//  kSeen -> kShown    | Notify of an abandon and restart timer, as user saw
//                     | results but changed view or updated the search query.
< ASCII >
//
// The transitions we consider impossible are kNone -> {kSeen, kLaunch},
// kSeen -> kSeen, and {kLaunch, kIgnored } -> anything, because kLaunch and
// kIgnored are temporary states.
//
// Discussion
// ==========
//
// Warning: NotifyResultsUpdated cannot be used as a signal of user actions or
// UI state. Results can be updated at any time for any UI view, regardless
// of the state of the launcher or what the user is doing.
```
## Visual type:
- #table 


== ./chromium/chromium_55.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/arc/enterprise/cert_store/cert_store_service.cc#L86-L149

```c
// The following series of functions related to ListCerts make use of the
// NSSCertDatabase to fulfill its goal of listing certificates. The cert
// database is accessed through a raw pointer with limited lifetime guarantees
// and is not thread safe. Namely, the cert database is guaranteed valid for the
// single IO thread task where it was received.
//
// Furthermore, creating an NssCertDatabaseGetter requires a BrowserContext,
// which can only be accessed on the UI thread.
//
// ListCerts and related functions are implemented to make sure the above
// requirements are respected. Here's a diagram of the interaction between UI
// and IO threads.
< ASCII >
//
//                    UI Thread                     IO Thread
//
//                    ListCerts
//                        |
//       NssService::CreateNSSCertDatabaseGetterForIOThread
//                        |
//                        \----------------------------v
//                                          ListCertsWithDbGetterOnIO
//                                                     |
//                                           database_getter.Run()
//                                                     |
//                                               ListCertsOnIO
//                                                     |
//                                              ListCertsInSlot
//                                                     |
//                                   PostListedCertsBackToOriginalTaskRunner
//                                                     |
//                        v----------------------------/
//  Process certs / Repeat ListCerts for system slot
< ASCII >
//
// ARC requires certs from both the 'user' and 'system' chaps slots to be
// processed. Because ListCertsInSlot is asynchronous, it's not possible to
// guarantee that both ListCertsInSlot calls happen in the same task execution,
// so this entire process is performed twice: first for the user slot, then for
// the system slot. The ordering of the calls is not important, other than the
// implementation lists the 'user' slot first, and uses the 'system' slot to
// signal the process is complete.
//
// The current user may not have access to the system slot, but that is only
// discoverable on the IO thread. In that case, the sequence for the system slot
// becomes:
< ASCII >
//
//                    UI Thread                     IO Thread
//
//                    ListCerts
//                        |
//       NssService::CreateNSSCertDatabaseGetterForIOThread
//                        |
//                        \----------------------------v
//                                          ListCertsWithDbGetterOnIO
//                                                     |
//                                           database_getter.Run()
//                                                     |
//                                                ListCertsOnIO
//                                                     |
//                                   (Determine system slot isn't allowed)
//                                                     |
//                                   PostListedCertsBackToOriginalTaskRunner
//                                                     |
//                        v----------------------------/
//             Process list of certs...
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_56.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/arc/enterprise/cert_store/cert_store_service_browsertest.cc#L190-L219

```c
// The following series of functions related to IsSystemSlotAvailable use the
// NSSCertDatabase. The cert database is accessed through a raw pointer with
// limited lifetime guarantees and is not thread safe. Namely, the cert database
// is guaranteed valid for the single IO thread task where it was received.
//
// Furthermore, creating an NssCertDatabaseGetter requires a BrowserContext,
// which can only be accessed on the UI thread.
//
// ListCerts and related functions are implemented to make sure the above
// requirements are respected. Here's a diagram of the interaction between UI
// and IO threads.
< ASCII >
//
//             UI Thread                        IO Thread
//
//       IsSystemSlotAvailable
//                 |
//       run_loop.QuitClosure
//                 |
//   NssService::CreateNSSCertDatabaseGetterForIOThread
//                 |
//                 \--------------------------------v
//                                 IsSystemSlotAvailableWithDbGetterOnIO
//                                                  |
//                                         database_getter.Run
//                                                  |
//                                       IsSystemSlotAvailableOnIO
//                                                  |
//                                            GetSystemSlot
//                                                  |
//                                           quit_closure.Run
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_57.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/arc/input_overlay/ui/edit_finish_view.h#L15-L28

```c
// View displaying the 3 possible options to finish edit mode.
//
// These actions refer to what the user can do wrt customized key-bindings, they
// can either reset to a set of default key-bindings or just accept/cancel the
// ongoing changes.
//
< ASCII >
// View looks like this:
// +----------------------+
// |   Reset to defaults  |
// |                      |
// |         Save         |
// |                      |
// |        Cancel        |
// +----------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_58.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/arc/input_overlay/ui/input_menu_view.h#L22-L34

```c
// A view that shows display options for input overlay, this is the entry
// point for customizing key bindings and turning the feature on/off.
//
< ASCII >
// The class reports back to DisplayOverlayController, who owns this.
//   +---------------------------------+
//   | Game Controls |Alpha| [ o]  [x] |
//   |                                 |
//   | Key mapping             [Edit]  |
//   |                                 |
//   | Show key mapping          [ o]  |
//   |                                 |
//   | Send feedback                   |
//   +---------------------------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_59.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/arc/input_overlay/ui/touch_point.cc#L54-L63

```c
// Draw the cross shape path with round corner. It starts from bottom to up on
// line #0 and draws clock-wisely.
// |overall_length| is the total length of one side excluding the stroke
// thickness. |mid_length| is the length of the middle part which is close to
// the one third of |overall_length|.
< ASCII >
//      __
//   _0^  |__
//  |__    __|
//     |__|
< ASCII >
//
```
## Visual type:
- #custom


== ./chromium/chromium_6.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/ambient/ui/ambient_animation_attribution_transformer.cc#L35-L77

```c
// This translates between 2 coordinate systems. The first coordinate system
// (the one translating from) is the views coordinate system where the origin
// is the top-left of the view. In practice, the typical case looks like this:
//
< ASCII >
// Animation
// +-----------------------------------------------+
// |                                               |
// |(0, 0) View                                    |
// +-----------------------------------------------|
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |-------------------------------------------+   |
// |                           Attribution Text|   |
// |-------------------------------------------+   |
// |                                               |
// +-----------------------------------------------+
// |                                               |
// |                                               |
// +-----------------------------------------------+
< ASCII >
//
// Note in the above, the animation's width matches the view's width, and its
// height exceeds that of the view (the upper and lower parts of the animation
// get cropped out). The origin is the top-left of the view, so the top-left of
// the animation has coordinates (0, <some negative number>). The attribution
// text box's bottom right corner has coordinates
// (view_width - 24, view_height - 24).
//
// The second set of coordinates (the one translating to) is the original
// animation's coordinate system. "Original" here refers to the
// coordinates baked into the Lottie file. Visually, it looks the same as the
// picture above, except:
// * The origin is the top-left of the animation.
// * The animation's width/height are those of the original animation (baked
//   into the Lottie file), as opposed to those of the "scaled" animation that
//   was scaled to reflect the view's bounds/dimensions.
//
// Note that although the typical case is illustrated above, the implementation
// was written generically to account for all cases.
```
## Visual type:
- #gui 


== ./chromium/chromium_60.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/game_mode/game_mode_controller.h#L43-L80

```c
// When a Borealis or ARC game app game enters full screen, game mode is
// enabled. Game Mode is actually enabled as a result of multiple sets of
// criteria being fulfilled, each checked in sequence.
//
// When one criteria set is met, a new criteria object is constructed which
// is responsible for checking the next criteria, and is owned by the prior
// criteria object. An owner destroys its direct and indirect owned (subsequent)
// criteria objects as soon as itself becomes invalid.
//
// The criteria objects are constructed in this order:
//
// Criteria object         Conditions checked      Causes invalidation (x)
// -----------------------------------------------------------------------------
// GameModeController      Window is focused
// WindowTracker *         Window is fullscreen    Window is destroyed
// ArcGameModeCriteria **  ARC app is game
// GameModeEnabler ***     None
//
// (x) Indicates the responsible criteria object makes itself inactive and
//     discards its child criteria, if any.
// *   WindowTracker is responsible for determining the type of window
// **  ArcGameModeCriteria is not constructed for Borealis windows. Instead, a
//     GameModeEnabler is constructed directly.
// *** GameModeEnabler starts game mode on construction, and stops game mode on
//     destruction.
//
< ASCII >
// More concretely, this is the logical flow:
//
//          +"GameMode off"+<---------------------------------------------+
//          |              ^ focus                                  focus ^
//          |              | lost                                   lost  |
//          V   focused    |                                   Y          |
// "Watch focus"------->"Watch state"--------->"Game window?"---->"GameMode on"
//                          ^         full           |                    |
//                          |       screen'd         | N       fullscreen |
//                          |                        V               lost V
//                     "GameMode off"<------------------------------------+
< ASCII >
//
```
## Visual type:
- #state-machine


== ./chromium/chromium_61.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/login/ui/login_display_host.h#L46-L64

```c
// An interface that defines an out-of-box-experience (OOBE) or login screen
// host. It contains code specific to the login UI implementation.
//
< ASCII >
// The inheritance graph is as folllows:
//
//                               LoginDisplayHost
//                                   /       |
//                LoginDisplayHostCommon   MockLoginDisplayHost
//                      /      |
//   LoginDisplayHostMojo    LoginDisplayHostWebUI
< ASCII >
//
//
// - LoginDisplayHost defines the generic interface.
// - LoginDisplayHostCommon is UI-agnostic code shared between the views and
//   webui hosts.
// - MockLoginDisplayHost is for tests.
// - LoginDisplayHostMojo is for the login screen which is implemented in Ash.
//   TODO(estade): rename LoginDisplayHostMojo since it no longer uses Mojo.
// - LoginDisplayHostWebUI is for OOBE, which is written in HTML/JS/CSS.
```
## Visual type:
- #class-diagram


== ./chromium/chromium_62.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ash/login/ui/oobe_ui_dialog_delegate.h#L46-L52

```c
// This class manages the behavior of the Oobe UI dialog.
// And its lifecycle is managed by the widget created in Show().
< ASCII >
//   WebDialogView<----delegate_----OobeUIDialogDelegate
//         |
//         |
//         V
//   clientView---->Widget's view hierarchy
< ASCII >
```
## Visual type:
- #class-diagram


== ./chromium/chromium_63.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/cart/fetch_discount_worker.h#L48-L61

```c
// This is used to fetch discounts for active Carts in cart_db. It starts
// to work after calling Start() and continue to work util Chrome is finished.
< ASCII >
// The flow looks as follow:
//
//   UI Thread              | backend_task_runner_
//  ===========================================
// 1) Start                 |
// 2) PrepareToFetch (delay)|
// 3) ReadyToFetch          |
// 4)                       | FetchInBackground
// 5)                       | DoneFetchingInBackground
// 6) AfterDiscountFetched  |
// 7) OnUpdatingDiscounts   |
// 8) Start                 |
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_64.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/devtools/serialize_host_descriptions_unittest.cc#L95-L99

```c
< ASCII >
// Test serializing a small forest, of this structure:
// 5 -- 2 -- 4
// 0 -- 6
//   \ 1
//   \ 3
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_65.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/downgrade/snapshot_manager_unittest.cc#L55-L61

```c
< ASCII >
// Structure containing a folders and files structure for tests.
// root
// |_ SomeFile
// |_ Some Folder
//    |_ Some File
//    |_ Some Sub Folder
//       |_ Some Sub File
< ASCII >
```
## Visual type:
- #tree


== ./chromium/chromium_66.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/extensions/api/omnibox/omnibox_unittest.cc#L43-L46

```c
< ASCII >
//   0123456789
//    mmmm
// +       ddd
// = nmmmmndddn
< ASCII >
```
## Visual type:
- #formula


== ./chromium/chromium_67.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/extensions/api/omnibox/omnibox_unittest.cc#L120-L126

```c
< ASCII >
//   0123456789
//   uuuuu
// +          dd
// +          mm
// + mmmm
// +  dd
// = 3773unnnn66
< ASCII >
```
## Visual type:
- #formula


== ./chromium/chromium_68.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/extensions/api/omnibox/omnibox_unittest.cc#L231-L237

```c
< ASCII >
//   0123456789
//   uuuuu
// + mmmmm
// + mmm
// +   ddd
// + ddd
// = 77777nnnnn
< ASCII >
```
## Visual type:
- #formula 


== ./chromium/chromium_69.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/extensions/api/omnibox/omnibox_unittest.cc#L290-L296

```c
< ASCII >
//   0123456789
//   uuuuu
// + mmmmm
// + mmm
// +   ddd
// + ddd
// = 77777nnnnn
< ASCII >
```
## Visual type:
- #formula 


== ./chromium/chromium_7.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/ambient/ui/ambient_animation_attribution_transformer.h#L16-L51

```c
// "Attribution" refers to the text credits that may optionally accompany each
// photo that's assigned to a dynamic asset in an animation. The Lottie files
// for ambient mode have a placeholder for each dynamic asset where its
// attribution text should go.
//
// The attribution text box's coordinates must be baked into the Lottie file.
// However, UX requires that it is positioned such that the bottom-right of the
// text box has 24 pixels of padding from the bottom-right of the view.
// Additionally, the text box's width should extend from the left side of the
// view all the way to (width - 24) to account for long attributions.
// Visually, it looks like this:
//
< ASCII >
// View:
// +-----------------------------------------------+
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |                                               |
// |-------------------------------------------+   |
// |                           Attribution Text|   |
// |-------------------------------------------+   |
// |                                               |
// +-----------------------------------------------+
< ASCII >
//
// The animation already right-aligns the text within the box, but since the
// view's boundaries can vary from device to device, it is impossible to
// specify text box coordinates in the lottie file that work for all devices.
//
// To accomplish this, AmbientAnimationAttributionTransformer uses Skottie's
// text/transform property observer API to intercept and modify the text box's
// coordinates.
```
## Visual type:
- #gui


== ./chromium/chromium_70.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/lacros/account_manager/profile_account_manager.h#L18-L34

```c
// This is a profile-scoped implementation of `AccountManagerFacade`, intended
// to be used by the identity manager. Account updates generally follow the
// path:
< ASCII >
//
//                       AccountManagerFacadeImpl
//                                  |
//                                  V
//                         AccountProfileMapper
//                                  |
//                                  V
//                         ProfileAccountManager
//                                  |
//                                  V
//                            IdentityManager
< ASCII >
//
// The `ProfileAccountManager` is not intended to have much logic and mostly
// forwards calls to the `AccountProfileMapper`.
```
## Visual type:
- #flowchart


== ./chromium/chromium_71.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/metrics/power/coalition_resource_usage_provider_mac.h#L24-L44

```c
// Provides resource usage rate for the current process' coalition over "short"
// and "long" intervals.
//
// Init() must be invoked before any other method. It starts a "long" interval.
// After that, StartShortInterval() and EndInterval() should be invoked in
// alternance to start a "short" interval, end both intervals and start a new
// "long" interval:
< ASCII >
//
//  |         Long          |         Long          |         Long          |
//                  | Short |               | Short |               | Short |
//  Init            SSI     EI              SSI     EI              SSI     EI
< ASCII >
//
//      SSI = StartShortInterval
//      EI  = EndIntervals
//
// See //components/power_metrics/resource_coalition_mac.h for more details
// about resource coalitions.
//
// NOTE: Chrome could belong to a non-empty coalition if it's started from a
// terminal, in which case the data will be hard to interpret. This class
// reports that the coalition data isn't available when it's the case.
```
## Visual type:
- #custom


== ./chromium/chromium_72.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/net/nss_service_chromeos.cc#L43-L77

```c
// The following four functions are responsible for initializing NSS for each
// profile on ChromeOS Ash, which has a separate NSS database and TPM slot
// per-profile.
//
// Initialization basically follows these steps:
// 1) Get some info from user_manager::UserManager about the User for this
// profile.
// 2) Tell nss_util to initialize the software slot for this profile.
// 3) Wait for the TPM module to be loaded by nss_util if it isn't already.
// 4) Ask CryptohomePkcs11Client which TPM slot id corresponds to this profile.
// 5) Tell nss_util to use that slot id on the TPM module.
//
// Some of these steps must happen on the UI thread, others must happen on the
// IO thread:
< ASCII >
//               UI thread                              IO Thread
//
//  NssService::NssService
//                   |
//  ProfileHelper::Get()->GetUserByProfile()
//                   \---------------------------------------v
//                                                 StartNSSInitOnIOThread
//                                                           |
//                                          crypto::InitializeNSSForChromeOSUser
//                                                           |
//                                                crypto::IsTPMTokenEnabled
//                                                           |
//                                          StartTPMSlotInitializationOnIOThread
//                   v---------------------------------------/
//     GetTPMInfoForUserOnUIThread
//                   |
//   ash::TPMTokenInfoGetter::Start
//                   |
//     DidGetTPMInfoForUserOnUIThread
//                   \---------------------------------------v
//                                          crypto::InitializeTPMForChromeOSUser
< ASCII >
```
## Visual type:
- #sequence-diagram


== ./chromium/chromium_73.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/privacy_budget/mesa_distribution.h#L13-L68

```c
// Generates a set of integers drawn from a mesa shaped probability distribution
// with replacement.
//
< ASCII >
// The PDF is:
//
//            ⎧    0                               ... if x < 0
//            ⎪
//     P(x) = ⎨    λ                               ... if 0 <= x < T
//            ⎪
//            ⎩    (1 - τ) * γ * (1 - γ)^{X - T}   ... otherwise
< ASCII >
//
// where
//
//   T = Value at which the PDF switches from a uniform to a geometric
//       distribution. Referred to in code as the `pivot_point`.
//
//   τ = Ratio of probability between linear region of the PDF. I.e. if τ = 0.9,
//       then 90% of the probability space is in the linear region. The ratio is
//       referred to in code as `dist_ratio`.
//
//   γ = Parameter of the geometric distribution.
//
//        τ
//   λ = ───
//        T
//
// In otherwords, the PDF is uniform up to T with a probability of λ, and then
// switches to a geometric distribution with parameter γ that extends to
// infinity.
//
// It looks like this in the form of a graph which should make a little bit more
// sense.
< ASCII >
//
//          P(x)   ▲
//                 │
//   probability  λ│┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┬,
//   density       │    uniform     ┊ L        geometric
//                 │  distribution  ┊  "._    distribution
//                 │                ┊     `--..______
//                 └────────────────┴──────────────────▶ x
//                 0                T
< ASCII >
//
// Why this odd combination of disjoint probability distributions?
//
// Such a distribution is useful when you want to select some set of elements
// uniformly up to a threshold, but want to allow for a tail distribution that
// extends arbitrarily past that range.
//
// The τ parameter establishes the balance between the linear region and the
// geometric region, while T establishes the scale. Typically we set τ to
// something close to 0.9 or so such that 0.1 of the probability space is
// reserved for the long tail.
//
// Parameters:
//   pivot_point: T as described above. Any value bigger than 0.
//   dist_ratio : τ as described above. Must be in (0,1).
```
## Visual type:
- #formula


== ./chromium/chromium_74.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/resources/net_internals/view.js#L168-L184

```c
/**
 * View that positions two views vertically. The top view should be
 * fixed-height, and the bottom view will fill the remainder of the space.
 *
< ASCII >
 *  +-----------------------------------+
 *  |            topView                |
 *  +-----------------------------------+
 *  |                                   |
 *  |                                   |
 *  |                                   |
 *  |          bottomView               |
 *  |                                   |
 *  |                                   |
 *  |                                   |
 *  |                                   |
 *  +-----------------------------------+
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_75.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/resources/net_internals/view.js#L216-L233

```c
/**
< ASCII >
 * View that positions two views horizontally. The left view should be
 * fixed-width, and the right view will fill the remainder of the space.
 *
 *  +----------+--------------------------+
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  | leftView |       rightView          |
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  |          |                          |
 *  +----------+--------------------------+
< ASCII >
 */
```
## Visual type:
- #gui


== ./chromium/chromium_76.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/resources/settings/site_settings/category_default_setting.ts#L5-L36

```c
/**
 * @fileoverview
 * 'category-default-setting' is the polymer element for showing a certain
 * category under Site Settings.
 *
< ASCII >
 * |optionLabel_| toggle is enabled:
 * +-------------------------------------------------+
 * | Category                                        |<-- Not defined here
 * |                                                 |
 * |  optionLabel_                     ( O)          |
 * |  optionDescription_                             |
 * |                                                 |
 * |  subOptionLabel                   ( O)          |<-- SubOptionMode.PREF
 * |  subOptionDescription                           |    (optional)
 * |                                                 |
 * +-------------------------------------------------+
 *
 * |optionLabel_| toggle is disabled:
 * +-------------------------------------------------+
 * | Category                                        |<-- Not defined here
 * |                                                 |
 * |  optionLabel_                     (O )          |
 * |  optionDescription_                             |
 * |                                                 |
 * |  subOptionLabel                   (O )          |<-- Toggle is off and
 * |  subOptionDescription                           |    disabled; or hidden
 * |                                                 |
 * +-------------------------------------------------+
 *
< ASCII >
 * TODO(crbug.com/1113642): Remove this element when content settings redesign
 * is launched.
 */
```
## Visual type:
- #gui


== ./chromium/chromium_77.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/site_isolation/site_per_process_interactive_browsertest.cc#L251-L262

```c
// Ensure that sequential focus navigation (advancing focused elements with
// <tab> and <shift-tab>) works across cross-process subframes.
// The test sets up six inputs fields in a page with two cross-process
// subframes:
< ASCII >
//                 child1            child2
//             /------------\    /------------\.
//             | 2. <input> |    | 4. <input> |
//  1. <input> | 3. <input> |    | 5. <input> |  6. <input>
//             \------------/    \------------/.
< ASCII >
//
// The test then presses <tab> six times to cycle through focused elements 1-6.
// The test then repeats this with <shift-tab> to cycle in reverse order.
```
## Visual type:
- #gui


== ./chromium/chromium_78.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/site_isolation/site_per_process_interactive_browsertest.cc#L344-L356

```c
// Similar to the test above, but check that sequential focus navigation works
// with <object> tags that contain OOPIFs.
//
< ASCII >
// The test sets up four inputs fields in a page with a <object> that contains
// an OOPIF:
//                <object>
//             /------------\.
//             | 2. <input> |
//  1. <input> | 3. <input> |  4. <input>
//             \------------/.
< ASCII >
//
// The test then presses <tab> 4 times to cycle through focused elements 1-4.
// The test then repeats this with <shift-tab> to cycle in reverse order.
```
## Visual type:
- #gui


== ./chromium/chromium_79.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/site_isolation/site_per_process_interactive_browsertest.cc#L966-L988

```c
// Check that fullscreen works on a more complex page hierarchy with multiple
// local and remote ancestors.  The test uses this frame tree:
< ASCII >
//
//             A (a_top)
//             |
//             A (a_bottom)
//            / \   .
// (b_first) B   B (b_second)
//               |
//               C (c_top)
//               |
//               C (c_middle) <- fullscreen target
//               |
//               C (c_bottom)
< ASCII >
//
// The c_middle frame will trigger fullscreen for its <div> element.  The test
// verifies that its ancestor chain is properly updated for fullscreen, and
// that the b_first node that's not on the chain is not affected.
//
// The test also exits fullscreen by simulating pressing ESC rather than using
// document.webkitExitFullscreen(), which tests the browser-initiated
// fullscreen exit path.
// TODO(crbug.com/756338): flaky on all platforms.
```
## Visual type:
- #tree


== ./chromium/chromium_8.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/capture_mode/capture_mode_bar_view.h#L24-L45

```c
// A view that acts as the content view of the capture mode bar widget.
// It has a set of buttons to toggle between image and video capture, and
// another set of buttons to toggle between fullscreen, region, and window
// capture sources. It also contains a settings button. The structure looks like
// this:
< ASCII >
//
//   +---------------------------------------------------------------+
//   |  +----------------+  |                       |                |
//   |  |  +---+  +---+  |  |  +---+  +---+  +---+  |  +---+  +---+  |
//   |  |  |   |  |   |  |  |  |   |  |   |  |   |  |  |   |  |   |  |
//   |  |  +---+  +---+  |  |  +---+  +---+  +---+  |  +---+  +---+  |
//   |  +----------------+  |  ^                 ^  |  ^      ^      |
//   +--^----------------------|-----------------|-----|------|------+
//   ^  |                      +-----------------+     |      |
//   |  |                      |                       |      IconButton
//   |  |                      |                       |
//   |  |                      |                       IconButton
//   |  |                      CaptureModeSourceView
//   |  CaptureModeTypeView
//   |
//   CaptureModeBarView
//
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_80.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/android/omnibox/java/src/org/chromium/chrome/browser/omnibox/suggestions/tail/AlignmentManager.java#L14-L31

```c
/**
 * Coordinates horizontal alignment of the tail suggestions.
 * Tail suggestions are aligned to
 * - the user input in the Omnibox, when possible,
 * - to each other (left edge) when longest tail suggestion makes it impossible to align it to
 *   user input.
 *
< ASCII >
 * Examples:
 * 1. Aligned to User input:
 *    ( User Query In Omni             )
 *    [           ... Omnibox          ]
 *    [           ... Omnibox Android  ]
 *
 * 2. Aligned to longest suggestion:
 *    ( Longer User Query In The Omni  )
 *    [             ... Omnibox        ]
 *    [             ... Omnibox Android]
< ASCII >
 */
```
## Visual type:
- #gui 


== ./chromium/chromium_81.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/android/signin/java/src/org/chromium/chrome/browser/ui/signin/ConfirmSyncDataStateMachine.java#L20-L48

```c
/**
 * This class takes care of the various dialogs that must be shown when the user changes the
 * account they are syncing to (either directly, or by signing in to a new account). Most of the
 * complexity is due to many of the decisions getting answered through callbacks.
 *
< ASCII >
 * This class progresses along the following state machine:
 *
 *       E-----\  G--\
 *       ^     |  ^  |
 *       |     v  |  v
 * A->B->C->D->+->F->H
 *    |        ^
 *    v        |
 *    \--------/
< ASCII >
 *
 * Where:
 * A - Start
 * B - Decision: progress to C if the user signed in previously to a different account, F otherwise.
 * C - Decision: progress to E if we are switching from a managed account, D otherwise.
 * D - Action: show Import Data Dialog.
 * E - Action: show Switching from Managed Account Dialog.
 * F - Decision: progress to G if we are switching to a managed account, H otherwise.
 * G - Action: show Switching to Managed Account Dialog.
 * H - End: perform {@link ConfirmImportSyncDataDialogCoordinator.Listener#onConfirm} with the
 * result of the Import Data Dialog, if displayed or true if switching from a managed account.
 *
 * At any dialog, the user can cancel the dialog and end the whole process (resulting in
 * {@link ConfirmImportSyncDataDialogCoordinator.Listener#onCancel}).
 */
```
## Visual type:
- #state-machine


== ./chromium/chromium_82.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/ash/projector/projector_navigation_throttle_browsertest.cc#L48-L53

```c
< ASCII >
// Summary of expected behavior on ChromeOS:
// _____________________________|_SWA_launches_|_URL_redirection
// screencast.apps.chrome       | Yes          | Yes
// projector.apps.chrome        | Yes          | Yes (online only)
// chrome://projector/app/      | Yes          | No
// chrome-untrusted://projector | No           | No
< ASCII >
```
## Visual type:
- #table


== ./chromium/chromium_83.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/autofill/autofill_context_menu_manager.h#L52-L100

```c
// `AutofillContextMenuManager` is responsible for adding/executing Autofill
// related context menu items. `RenderViewContextMenu` is intended to own and
// control the lifetime of `AutofillContextMenuManager`.
// Here is an example of the structure of the autofill related context menu
// items:
// ...
// ...
// ...
< ASCII >
// Fill Address Info > Alex Park, 345 Spear Street... > Alex Park
//                                                      ___________________
//                                                      345 Spear Street
//                                                      San Francisco
//                                                      94105
//                                                      ___________________
//                                                      +1 858 230 4000
//                                                      alexpark@gmail.com
//                                                      ___________________
//                                                      Other             > Alex
//                                                                          Park
//                                                                          ___________
//                                                                          345
//                                                                          Spear
//                                                                          Street
//                     ______________________________
//                     Manage Addresses
// Fill Payment      > Mastercard **** 0952           > Alex Park
//                                                      **** **** **** 0952
//                                                      ___________________
//                                                      04
//                                                      2025
//                     ______________________________
//                     Manage payment methods
//
< ASCII >
// Provide Autofill feedback
// ....
// ....
// ....
//
// From the example, there are 3 layers:
// 1. Outermost layer that distinguishes between address or payment method
// filling. Refer to `AppendItems` and `AddAddressOrCreditCardItemsToMenu` for
// more info. It also includes an option to submit user feedback on Autofill.
// 2. Profile description layer to identify which profile to use for filling.
// Refer to `AddAddressOrCreditCardItemsToMenu` for more info.
// 3. Profile data layer that would be used for filling a single field. See
// `AddAddressOrCreditCardItemToMenu` and `AddProfileDataToMenu` for details on
// how this is done.
// 4. The Other Section, that suggests more granular data for filling. See
// `AddAddressOrCreditCardItemToMenu` and `AddProfileDataToMenu` for details.
```
## Visual type:
- #gui 


== ./chromium/chromium_84.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/confirm_bubble_views.h#L20-L28

```c
// A dialog (with the standard Title/[OK]/[Cancel] UI elements), as well as
// a message Label and help (?) button. The dialog ultimately appears like this:
< ASCII >
//   +------------------------+
//   | Title                  |
//   | Label                  |
//   | (?)      [OK] [Cancel] |
//   +------------------------+
< ASCII >
//
// TODO(msw): Remove this class or merge it with DialogDelegateView.
```
## Visual type:
- #gui


== ./chromium/chromium_85.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/intent_picker_bubble_view.h#L35-L55

```cs
// A bubble that displays a list of applications (icons and names), after the
// list the UI displays a checkbox to allow the user remember the selection and
// after that a couple of buttons for either using the selected app or just
// staying in Chrome. The top right close button and clicking somewhere else
// outside of the bubble allows the user to dismiss the bubble (and stay in
// Chrome) without remembering any decision.
//
// This class communicates the user's selection with a callback supplied by
// AppsNavigationThrottle.
< ASCII >
//   +--------------------------------+
//   | Open with                  [x] |
//   |                                |
//   | Icon1  Name1                   |
//   | Icon2  Name2                   |
//   |  ...                           |
//   | Icon(N) Name(N)                |
//   |                                |
//   | [_] Remember my choice         |
//   |                                |
//   |     [Use app] [Stay in Chrome] |
//   +--------------------------------+
< ASCII >
class IntentPickerBubbleView : public LocationBarBubbleDelegateView {
    ...
}
```
## Visual type:
- #gui


== ./chromium/chromium_86.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/fullscreen_control/fullscreen_control_host.cc#L30-L45

```c
< ASCII >
// +----------------------------+
// |         +-------+          |
// |         |Control|          |
// |         +-------+          |
// |                            | <-- Control.bottom * kExitHeightScaleFactor
// |          Screen            |       Buffer for mouse moves or pointer events
// |                            |       before closing the fullscreen exit
// |                            |       control.
// +----------------------------+
//
< ASCII >
// The same value is also used for timeout cooldown.
// This is a common scenario where people play video or present slides and they
// just want to keep their cursor on the top. In this case we timeout the exit
// control so that it doesn't show permanently. The user will then need to move
// the cursor out of the cooldown area and move it back to the top to re-trigger
// the exit UI.
```
## Visual type:
- #gui 


== ./chromium/chromium_87.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/fullscreen_control/fullscreen_control_host.cc#L48-L56

```c
< ASCII >
// +----------------------------+
// |                            |
// |                            |
// |                            | <-- kShowFullscreenExitControlHeight
// |          Screen            |       If a mouse move or pointer event is
// |                            |       above this line, show the fullscreen
// |                            |       exit control.
// |                            |
// +----------------------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_88.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/page_info/page_info_permission_content_view.h#L21-L32

```c
// The view that is used as a content view of the permissions subpages in page
// info. It contains information about the permission (icon, title, state label)
// and controls to change the permission state (toggle, checkbox and manage
// button).
< ASCII >
// *---------------------------------------------------------------*
// | Icon | Title                                         | Toggle |
// |      | State label                                   |        |
// |      |                                               |        |
// |      | "Remember this setting" checkbox              |        |
// |---------------------------------------------------------------|
// | Manage button                                                 |
// *---------------------------------------------------------------*
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_89.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/page_info/page_info_row_view.h#L24-L29

```c
< ASCII >
// A view that contains basic layout for rows in page info.
// *-------------------------------------------------------------------------*
// | Icon | Title                                | Controls (buttons, icons) |
// |-------------------------------------------------------------------------|
// |      | Secondary label                      |                           |
// *-------------------------------------------------------------------------*
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_9.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/ash/capture_mode/recording_overlay_controller.cc#L27-L57

```c
// When recording a non-root window (i.e. kWindow recording source), the overlay
// is added as a direct child of the window being recorded, and stacked on top
// of all children. This is so that the overlay contents show up in the
// recording above everything else.
< ASCII >
//
//   + window_being_recorded
//       |
//       + (Some other child windows hosting contents of the window)
//       |
//       + Recording overlay widget
< ASCII >
//
// (Note that bottom-most child are the top-most child in terms of z-order).
//
// However, when recording the root window (i.e. kFullscreen or kRegion
// recording sources), the overlay is added as a child of the menu container.
// The menu container is high enough in terms of z-order, making the overlay on
// top of most things. However, it's also the same container used by the
// projector bar (which we want to be on top of the overlay, since it has the
// button to toggle the overlay off, and we don't want the overlay to block
// events going to that button). Therefore, the overlay is stacked at the bottom
// of the menu container's children. See UpdateWidgetStacking() below.
//
< ASCII >
//   + Menu container
//     |
//     + Recording overlay widget
//     |
//     + Projector bar widget
< ASCII >
//
// TODO(https://crbug.com/1253011): Revise this parenting and z-ordering once
// the deprecated Projector toolbar is removed and replaced by the shelf-pod
// based new tools.
```
## Visual type:
- #tree


== ./chromium/chromium_90.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/payment_sheet_view_controller.cc#L474-L477

```c
< ASCII >
// Adds the product logo to the footer.
// +---------------------------------------------------------+
// | (•) chrome                               | PAY | CANCEL |
// +---------------------------------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_91.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/payment_sheet_view_controller.cc#L493-L501

```c
// Creates the Order Summary row, which contains an "Order Summary" label,
// an inline list of display items, a Total Amount label, and a Chevron. Returns
// nullptr if WeakPtr<PaymentRequestSpec> has become null.
< ASCII >
// +----------------------------------------------+
// | Order Summary   Item 1            $ 1.34     |
// |                 Item 2            $ 2.00   > |
// |                 2 more items...              |
// |                 Total         USD $12.34     |
// +----------------------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_92.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/payment_sheet_view_controller.cc#L606-L613

```c
// Creates the Shipping row, which contains a "Shipping address" label, the
// user's selected shipping address, and a chevron. Returns null if the
// WeakPtr<PaymentRequestSpec> has become null.
< ASCII >
// +----------------------------------------------+
// | Shipping Address   Barack Obama              |
// |                    1600 Pennsylvania Ave.  > |
// |                    1800MYPOTUS               |
// +----------------------------------------------+
< ASCII >
```
## Visual type:
- #gui


== ./chromium/chromium_93.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/payment_sheet_view_controller.cc#L661-L668

```c
// Creates the Payment Method row, which contains a "Payment" label, the
// selected Payment Method's name and details, the Payment Method's icon, and a
// chevron.
< ASCII >
// +----------------------------------------------+
// | Payment         BobBucks                     |
// |                 bobbucks.dev      | ICON | > |
// |                                              |
// +----------------------------------------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_94.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/secure_payment_confirmation_dialog_view.cc#L290-L300

```c
< ASCII >
// Creates the body including the title, the set of merchant, instrument, and
// total rows.
// +------------------------------------------+
// | Title                                    |
// |                                          |
// | merchant label      value                |
// +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
// | instrument label    [icon] value         |
// +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
// | total label         value                |
// +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_95.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/secure_payment_confirmation_dialog_view.cc#L333-L336

```c
< ASCII >
// Creates a row of data with |label|, |value|, and optionally |icon|.
// +------------------------------------------+
// | label      [icon] value                  |
// +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+ <-- border
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_96.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/secure_payment_confirmation_no_creds_dialog_view.cc#L143-L149

```c
< ASCII >
// Creates the body.
// +------------------------------------------+
// |              [header image]              |
// |                                          |
// | No matching credentials text             |
// |                                     [OK] |
// +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_97.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/payments/secure_payment_confirmation_views_util.h#L60-L66

```c
< ASCII >
// Creates the header view, which contains the icon and a progress bar. The icon
// covers the whole header view with the progress bar at the top of the header.
// +------------------------------------------+
// |===============progress bar===============|
// |                                          |
// |                   icon                   |
// +------------------------------------------+
< ASCII >
```
## Visual type:
- #gui 


== ./chromium/chromium_98.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/sharing_hub/preview_view.h#L22-L31

```c
// A PreviewView shows some information about a pending share so the user can
// tell what they're about to share. In particular, it looks like this:
< ASCII >
//   +-----------------------------+
//   |+------+  Title              |
//   || Icon |                     |
//   |+------+  URL                |
//   +-----------------------------+
< ASCII >
// The title and URL are fixed at construction time, but the icon may change -
// which image is used depends on the state of the DesktopSharePreview field
// trial.
```
## Visual type:
- #gui 


== ./chromium/chromium_99.md
# https://github.com/chromium/chromium/blob/3a651660d1d01daac3c0ff0faacd098fd50e9c84/chrome/browser/ui/views/try_chrome_dialog_win/arrow_border.h#L24-L32

```cs
< ASCII >
// A Border that paints a border around a view with an arrow pointing outward to
// some location. For example:
// ___________________________
// |      View contents      |
// |_________    ____________|
//           \  /
//            \/
< ASCII >
// The owner must provide the bounding rectangle of of the arrow in screen
// coordinates (pixels) via set_arrow_bounds().
```
## Visual type:
- #gui 
