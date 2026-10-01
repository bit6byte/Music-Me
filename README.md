🎵 Music Me
A fully working, offline-first music player for Android — built with Flutter + Material 3

✨ Overview
Music Me is a clean, single-screen music player that scans your device's audio library, groups songs by folder, and delivers a full playback experience — complete with background play, lock-screen controls, and a beautiful accent-tinted Material 3 interface.

No tabs. No clutter. Just your music. 🎧

📚 Features
🗂️ Library
🔍 Scans songs on the device via Android MediaStore
📁 Groups every track by folder (keyed by absolute path — no name collisions!)
🎬 Folder detail screen with cover header, play & shuffle actions
🔄 Pull-to-refresh to rescan your library
🎁 Bundled Demo Tracks folder of streaming songs — playable even with an empty device (toggleable in Settings)
❤️ Favourites, persisted between launches
▶️ Playback
⏯️ Play / pause, previous, next & seekable progress bar
🔀 Shuffle & 🔁 repeat (off → all → one)
📝 Queue editing: play next, add to queue, remove & drag to reorder
⏰ Sleep timer
🌙 Background playback with media notification, lock-screen controls & headset button support (audio_service + just_audio)
🎨 Design (Material 3)
Element	Detail
🌈 Palette	MaterialApp with useMaterial3: true + ColorScheme.fromSeed
💠 Accent Colour	8 swatches in Settings, saved between launches — one seed re-tints bars, buttons, scrubber, sheets & player controls instantly
🧱 Structure	Scaffold + SliverAppBar.large (collapsing titles), AppBar, RefreshIndicator
🧩 Components	ListTile, SwitchListTile, Card, IconButton.filled, Slider, CircularProgressIndicator
📋 Sheets	showModalBottomSheet for queue, song actions & sleep timer
💬 Feedback	AlertDialog confirmations + SnackBar toasts
📊 Mini Player	LinearProgressIndicator in the docked mini player
🌄 Now Playing	Custom slide-up NowPlayingRoute — background gradient sampled from cover art
🌗 Theming	Automatic light & dark mode via ThemeMode.system
📱 Responsive (flutter_screenutil)
📐 One design canvas (390 × 844) drives everything — AppTheme exposes .w, .h, .r and .sp tokens so spacing, type & artwork scale together
🔠 Clamped TextScaler (0.85–1.3) keeps large-accessibility text readable
🖥️ Content capped at 640dp and centred — tablets never stretch rows into unreadable lines
✅ Verified at: 320×568, 390×844, 844×390 (landscape), 1024×1366, 1400×1000
🚀 Getting Started
flutter pub get
flutter run        # on a connected device or emulator
🔐 The app asks for the audio permission on first launch. Granting it is optional — without it you can still enjoy the bundled demo tracks! 🎧

🏗️ Architecture
lib/
  main.dart                     🚪 App entry point, registers the media session
  data/demo_songs.dart          🎁 Bundled "Demo Tracks" folder
  models/                       📦 Song, Folder
  services/
    device_media_store.dart     🌉 Dart side of the MediaStore bridge
    library_service.dart        🔐 Permission + device library loading
    music_handler.dart          🎛️ just_audio ↔ audio_service adapter
    artwork_cache.dart          🖼️ In-memory cover art + gradient extraction
    preferences_service.dart    💾 Favourites, demo mode & accent persistence
  state/
    appearance_provider.dart    🎨 The accent the whole palette is generated from
    library_provider.dart       📁 Songs grouped into folders, favourites
    player_provider.dart        ⚡ Queue, transport, shuffle/repeat, sleep timer
  theme/app_theme.dart          🎯 Design tokens + the accent palette
  ui/
    folders_screen.dart         🏠 The single home screen
    folder_detail_screen.dart   📂 Tracks inside one folder
    now_playing_screen.dart     🎧 Full player + custom route
    queue_sheet.dart            📝 "Playing Next"
    settings_screen.dart        ⚙️ Grouped settings
    favourites_screen.dart      ❤️ Hearted tracks
    widgets/                    🧩 Mini player, song row, artwork, action sheet

android/.../MainActivity.kt     🤖 Native MediaStore query + permission handling
🧠 State Management
Built with provider (ChangeNotifier). PlayerProvider only notifies on meaningful changes and publishes the moving playhead through a separate ValueNotifier — so the 5 Hz progress bar never rebuilds the app. ⚡

🤔 Why a Platform Channel for the Library?
MediaStore access needs a ContentResolver query + runtime permission, so it's implemented directly in MainActivity.kt and reached over the music_me/media_store method channel. This keeps the dependency list lean (just_audio, audio_service, provider, shared_preferences) and avoids plugins that no longer build against current Android Gradle Plugin versions. 🛠️

📂 Folder names & paths are derived from each track's absolute file path — not the deprecated BUCKET column — so nested folders like /sdcard/Music/Rock stay separate from /sdcard/Music.

🧪 Tests
flutter analyze   # ✅ no issues
flutter test      # ✅ 32 tests passing
Coverage includes: 📦 the Song model's URI/MediaItem mapping · ⏱️ duration formatting · 📁 folder grouping & lookup · ❤️ favourites persistence · 🎁 demo-mode toggling · 🧪 widget smoke tests that render the folder list and open a folder.

🔑 Permissions
Permission	📌 Why
READ_MEDIA_AUDIO (Android 13+)	Read songs stored on the device
READ_EXTERNAL_STORAGE (Android 12-)	Same, on older releases
FOREGROUND_SERVICE · FOREGROUND_SERVICE_MEDIA_PLAYBACK · WAKE_LOCK	Keep music playing in the background
INTERNET	Stream the bundled demo tracks
📄 License
A new Flutter project. 🚀

🎶 Music Me — your folders, your colour, your music.
