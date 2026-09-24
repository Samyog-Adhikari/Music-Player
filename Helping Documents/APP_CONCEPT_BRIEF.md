# Music Player App — Complete Project & Architecture Brief

> **Purpose of this document:**  
> This file contains the complete project specification, functional requirements, user flows, and architecture details for our music player app (temporary name: **VibeTune**). It is intentionally free of hardcoded colors or fixed styling so you can provide it directly to **Google Stitch** or any AI UI design tool alongside your prompt to generate cutting-edge, world-class UI mockups.

---

## 1. Executive Summary & Vision

- **App Concept:** An ad-free, high-performance music streaming application sourced from YouTube Music's global catalog.
- **Key Proposition:** Unlimited streaming, zero paywalls, instant guest mode (no forced registration), and an ultra-modern dual-layout interface that feels like a native app on mobile and an elevated Spotify/YouTube Music on desktop web.
- **Target Platforms:**
  1. **Web Browser:** Single-Page App (SPA) with responsive Desktop & Mobile shells.
  2. **Mobile (Android & iOS):** Packaged via Capacitor with native background playback & lock screen widgets.
  3. **Desktop (Windows/Mac/Linux):** Electron wrapper with native media keys.

---

## 2. Core Architectural Principles

1. **One Unified Codebase (React + TypeScript + Vite):**
   - Single application codebase serving Web, Mobile, and Desktop shells.
   - Platform abstraction layers (`AudioEngine`, `LocalStore`, `Platform`) decouple UI from native/browser differences.

2. **Direct-to-Music Experience (No Landing Page):**
   - The web app opens directly into the player interface / home dashboard.
   - No landing page or sign-up wall blocking the user.

3. **100% Functional Guest Mode:**
   - Browsing, searching, streaming, local queuing, playlist creation, equalizer adjustments, and sleep timer all work **without logging in**.
   - Login (Supabase Auth) is purely optional for cloud playlist backup and cross-device synchronization.

4. **Dynamic Adaptive Layout System (`useLayoutMode`):**
   - **Desktop Layout (≥ 768px):** Left sidebar navigation + top search bar + multi-column grid layouts + bottom persistent player bar.
   - **Mobile Layout (< 768px):** Compact top header + scrollable content + floating mini-player + bottom 4-tab bar + slide-out navigation drawer.
   - Live real-time responsive switching on window resize with a manual layout toggle in Settings.

5. **Centralized App Branding:**
   - App name, logo icon, taglines, and metadata are stored in a single configuration file (`appConfig.ts`). Changing the name updates all 27 views automatically.

---

## 3. Complete Feature & Screen Inventory (27 Views & Modules)

### A. Navigation & Shell Elements
1. **Desktop Sidebar Navigation:**
   - Brand logo with tagline.
   - Main links: Home, Search, Explore, Library, Playlists, Favorites, Folders, Settings.
   - "My Playlists" quick section with "+" new playlist button.
   - Quick utility links: Equalizer, Sleep Timer, Lockscreen Widget Preview.
   - User account status badge / login trigger at the bottom.
2. **Mobile Header & Slide-Out Drawer Menu (Screen 26):**
   - Header with menu toggle button, app logo, and user avatar.
   - Drawer containing user stats (playlists count, liked songs, listening hours), navigation links, playlist shortcuts, audio tools, and logout/login.
3. **Mobile Bottom Navigation Bar:**
   - 4 primary tabs: Home, Explore, Library, Search with active indicators.
4. **Desktop Persistent Player Bar:**
   - Left: Album thumbnail, track title, artist name, favorite heart toggle.
   - Center: Shuffle, Previous, Play/Pause, Next, Repeat (Off/All/One), and interactive time scrubber with elapsed/total duration.
   - Right: Synced Lyrics toggle, Queue drawer toggle, Equalizer modal toggle, Sleep Timer toggle, Volume slider with mute button.
5. **Mobile Floating Mini-Player:**
   - Floating rounded card above the bottom navigation tabs with live progress strip, artwork, title, artist, like button, play/pause, and next track button.
   - Tap anywhere to expand into the Full-Screen Now Playing view.

---

### B. Core Screens & Views

6. **Home Dashboard (Screen 4 & Secondary Idea):**
   - Time-sensitive greeting (e.g., *"Good Morning, Samyog"* or *"Welcome, Music Lover"*).
   - Hero banner with quick "Play Daily Mix" CTA and "Explore Genres".
   - Quick access cards: "Liked Songs" (with track count) and "Recently Played".
   - "Made For You" carousel/grid with personalized smart mixes.
   - "Trending Tracks" song list with rank, artwork, play button, and like status.
   - "Featured Artists" with circular avatars and follower stats.
   - "Popular Albums" grid.

7. **Search & Discovery Engine (Screen 13):**
   - Prominent search input with auto-focus, clear button, and real-time query filtering.
   - Filter chips: `All`, `Songs`, `Albums`, `Artists`, `Playlists`, `Genres`.
   - "Recent Searches" history tags with one-tap search.
   - Multi-category results sections (Top Songs with inline playback, Artist circle cards, Album cards).

8. **Library Hub (Screen 5):**
   - Multi-tab navigation: `Playlists`, `Songs`, `Albums`, `Artists`, `Folders`.
   - Featured interactive cards for "Liked Songs" and "Recently Played".
   - Full categorized display of user's saved music items.

9. **Playlists Manager & Detail Views (Screens 8 & 9):**
   - Playlists catalog with category badges (`For You`, `My Playlists`, `Auto Playlists`, `Smart Mixes`).
   - "Create Playlist" modal/action.
   - Playlist/Album detail page: High-res artwork header, metadata (creator/year, total tracks, duration), "Play All" and "Shuffle" buttons, full song table.

10. **Favorites & Liked Songs (Screen 10):**
    - Starred tracks hub with large animated heart header badge, total count, instant "Play All" and "Shuffle" controls, and tracklist.

11. **Explore & Browse Screen:**
    - Genre & mood tiles (Pop Hits, Synthwave, Hip Hop, Chill/Lofi, R&B, Rock/Indie).
    - Global New Releases and Top 10 Charts.

12. **Local Folders Explorer (Screen 14):**
    - Offline storage browser for local music directories (`Music`, `Download`, `Bollywood`, `Rock`, `Hip Hop`, `EDM`, etc.).
    - Song count and file size indicators (e.g., "42 songs • 2.6 GB").

---

### C. Immersive Audio Overlays & Modals

13. **Full-Screen Now Playing (Screen 11):**
    - Dynamic blurred ambient glow matching album artwork.
    - Header with collapse chevron, source label (*"Playing from YouTube / Playlist"*), and share button.
    - Large center artwork with vinyl/glow styling.
    - Track title, artist, like button, and add-to-playlist button.
    - Interactive waveform/slider progress bar with timestamps.
    - Transport controls: Shuffle, Skip Back, Large Play/Pause, Skip Forward, Repeat cycle.
    - Quick shortcut footer: Lyrics, Queue, Equalizer, Sleep Timer.

14. **Queue & Up Next Panel (Screen 12):**
    - Slide-over panel with `Up Next` and `History` tabs.
    - "Now Playing" highlight banner.
    - Reorderable and removable queue items.
    - "Add Songs" and "Clear Queue" actions.

15. **Synchronized Lyrics Visualizer (Screen 15):**
    - Synchronized line-by-line lyrics display with active karaoke line highlighting.
    - Ambient background glow matching artwork.
    - Floating playback control strip at bottom.

16. **Web Audio Equalizer (Screen 16):**
    - Acoustic preset chips: `Normal`, `Bass Boost`, `Rock`, `Pop`, `Jazz`, `Vocal`.
    - 3-band parametric frequency sliders: **Bass**, **Mid**, **Treble** (-10 dB to +10 dB).
    - 3D Surround sound and Bass Boost toggle options.
    - Reset and Apply action buttons.

17. **Sleep Timer (Screen 17):**
    - Preset radio options: `Off`, `15 minutes`, `30 minutes`, `45 minutes`, `60 minutes`, and `Custom` minutes input.
    - Active timer countdown banner displaying remaining minutes.

18. **Authentication & Profile Modal (Screens 3, 21, 22):**
    - Dual mode: Login / Sign Up with Email & Password.
    - Social login options: Google and Apple.
    - "Skip & Continue as Guest" bypass link.
    - Logged-in profile view: User avatar, name, email, listening stats (playlists, liked tracks, hours), cloud sync status, and Logout confirmation.

19. **Settings & Preferences (Screens 18, 19, 20):**
    - **Appearance:** Theme mode (`Dark`, `Light`, `System`), Accent color picker (Purple, Pink, Blue, Teal, Amber), "Force Desktop Layout" toggle.
    - **Audio & Playback:** Equalizer shortcut, Sleep Timer shortcut, Crossfade duration, Gapless playback, Audio normalization, Stream quality selector (`High 320kbps`).
    - **Storage & Cache:** Cache usage monitor with "Clear Cache" button.
    - **About & App Info:** Version info, open-source credits, and app renaming guide.

20. **Lock Screen Controls Preview (Screen 23):**
    - Interactive mobile lock screen widget preview with realistic wallpaper, digital clock, date, artwork, progress scrubber, and media controls.

21. **In-App Notification & Toast Banners (Screens 24 & 27):**
    - Non-intrusive floating toasts for events (Track liked/unliked, Song added to queue, Playlist created, Sleep timer set/ended).

---

## 4. UI Component Architecture (shadcn UI Design System)

The UI utilizes a modern component system built on top of accessible primitives:
- **Button:** Standard, Outline, Ghost, Gradient, Pill, Icon variants with subtle micro-interactions.
- **Card:** Glassmorphic translucent cards with subtle border highlight and hover lift.
- **Dialog & Sheet:** Modals for Now Playing, Lyrics, Equalizer, Sleep Timer, Auth, and Sidebar Drawer.
- **Tabs:** Accessible switching for Library sections, Queue/History, and Search filters.
- **Slider:** Precision scrubbers for audio progress, volume, and equalizer frequency bands.
- **Switch:** Toggle controls for Gapless audio, Crossfade, and Desktop layout override.
- **Avatar:** Circular user profile and artist portraits with status badges.
- **Badge:** Tag indicators for genres, releases, and smart mixes.
- **Dropdown Menu:** Context menus for track options (Add to playlist, Share, Go to artist).

---

## 5. Summary for AI Design Prompting (Google Stitch Ready)

> When generating designs from this brief:
> - Aim for a **sleek, dark-mode-first aesthetic** with rich depth, glassmorphism, elegant typography, and vibrant accents.
> - Ensure **desktop views feel spacious and productive** (sidebar + multi-column grid + bottom player bar).
> - Ensure **mobile views feel like a high-end native iOS/Android music app** (floating mini-player, bottom navigation, smooth sheets).
