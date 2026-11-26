# BUDDY — Personal Multilingual AI Assistant

BUDDY is an Android-friendly, mobile-first personal AI assistant with a futuristic blue-and-black interface that chats and speaks naturally in English, Hindi, and authentic Desi Punjabi. All AI chat, speech-to-text, and voice playback capabilities run on real, free-tier services with honest error states and local on-device chat history.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> Your Phase 1 preferences are locked into this blueprint. Review the plan below and click **Proceed** to start building, or let me know if you want any adjustments first.

- **Confirmed Decision 1 — Spoken Replies**: BUDDY will speak replies out loud using real text-to-speech, paired with a prominent mute/unmute toggle and per-message play/stop controls.
- **Confirmed Decision 2 — Script Matching**: BUDDY will automatically match whichever language and script you use (English, Hinglish, Devanagari Hindi, Roman Desi Punjabi, or Gurmukhi Punjabi), with an optional manual language selector when you want to steer replies explicitly.
- **Confirmed Decision 3 — Local-Only Chat History**: Conversations are saved directly on your device in browser local storage (no sign-in, cloud database, or paid subscriptions required).
- **Recommended Default — Free-Tier Real Voice Pipeline**: Voice input will record live microphone audio and transcribe it via free-tier Gemini audio transcription (`gemini-3.5-transcribe`), and spoken replies will use free-tier Gemini TTS (`gemini-3.8-flash-lite-tts`). This avoids browser-specific Web Speech recognition failures on Hindi/Punjabi code-switching while keeping everything 100% real and free.

---

## 1. Overview & Core Concept

- **What It Does**: BUDDY is a responsive web-based personal assistant built for effortless thumb use on Android phones while scaling into a full two-column workspace on desktop screens. Users can type or tap the microphone button to speak in English, Hindi, or Desi Punjabi, receive streamed intelligent answers in real time, hear BUDDY speak back naturally, and revisit or manage saved chats stored locally on their device.
- **Target Audience / Persona**: Everyday Android and desktop users—including beginners—who want a warm, sharp, futuristic personal assistant that genuinely understands conversational Hindi, English, and Desi Punjabi without setup friction or subscriptions.
- **Key Value**: Combines a sleek futuristic aesthetic with genuine multilingual personality (natural Desi Punjabi warmth rather than robotic translations), reliable voice input/output that never fakes functionality, and instant on-device history with zero account barriers.

---

## 2. User Experience & Visual Design

### Key User Flows

1. **First Launch (Welcoming Zero-State)**:
   - User lands directly inside a clean, distraction-free chat view featuring BUDDY's futuristic blue core emblem, a short friendly greeting, and 3 one-tap starter prompts showcasing English, Hindi, and Desi Punjabi (e.g., *"Plan my evening study schedule"*, *"Aaj dinner mein kya banaun? Kuch quick aur tasty batao"*, *"Sat Sri Akal Buddy! Ajj mood fresh karan layi kuch vadiya suna"*).
   - No onboarding walls, popups, or API-key forms.
2. **Typing & Streaming Conversation**:
   - User types into the bottom thumb-zone composer and taps **Send** (or presses Enter).
   - User's message appears immediately; BUDDY streams its response token-by-token in the matching language and script.
   - If Auto-Voice is unmuted, BUDDY automatically fetches and plays the spoken audio once the reply finishes streaming, displaying an active equalizer/stop affordance on the message.
3. **Voice Input (Microphone Flow)**:
   - User taps the large bottom-bar **Microphone** button (48×48px touch target).
   - App requests real microphone access, opens an inline recording bar with a live audio-level waveform and recording timer, plus **Cancel** and **Done** controls.
   - Stopping recording sends the audio to the transcription endpoint; the transcribed text appears in the chat and automatically triggers BUDDY's reply. If microphone permission is denied or unsupported, a clear inline notice explains why without pretending to listen.
4. **Language Mode & Voice Controls**:
   - A compact top bar provides a **Chats** drawer button (mobile), the **BUDDY** brand mark, and a one-tap **Voice On / Muted** toggle.
   - Inside the sidebar/drawer (and a sleek compact bar on zero-state), users can keep language on **Auto-Match** (default) or explicitly pick **English**, **Hindi**, or **Punjabi**—always preserving their script preference.
5. **Managing On-Device History**:
   - Users can start a **New Chat**, switch between saved local conversations, or delete any thread with immediate persistence to device storage.

### Visual Identity & Theme

- **Aesthetic Direction**: Refined Futuristic Obsidian & Cobalt — calm, modern, high-contrast, and approachable for beginners. Inspired by sleek sci-fi personal assistants without noisy cyberpunk clutter, decorative pulsing dots, or fake telemetry banners.
- **Color Palette & Mood (60-30-10 Distribution)**:
  - *60% Dominant Canvas*: Deep Obsidian Black (`--background: #060A12`) with subtle cool radial atmosphere behind the zero-state emblem.
  - *30% Structural Surfaces*: Midnight Slate panels (`--surface: #0D1526`, `--surface-elevated: #131E35`) with crisp hairline blue-tinted borders (`rgba(59, 130, 246, 0.16)`) and high-legibility cool-white typography (`--foreground: #F1F5F9`, `--muted: #94A3B8`).
  - *10% Accent Budget*: Electric Cobalt Blue (`--primary: #2563EB`) and Luminous Sky Blue (`--accent: #38BDF8`) reserved strictly for primary buttons, active microphone state, focus rings, and user chat bubbles. Recording/stop states pair color with explicit icons and labels (`Recording`, `Stop`) for accessibility.
- **Typography & Hierarchy**:
  - *Display & Brand*: **Plus Jakarta Sans** (clean geometric clarity with friendly curves, balanced headings via `text-wrap: balance`).
  - *Chat Body & Multilingual Scripts*: **DM Sans** paired with **Noto Sans Devanagari** and **Noto Sans Gurmukhi** web fonts so English, Hindi, and Punjabi scripts render crisply at `15px–16px` (`1.6` line-height) on Android and desktop.
  - *Timers & Timestamps*: Tabular numerals (`tabular-nums`) rendered as quiet unboxed metadata (`10:42 AM · Punjabi`), adhering to zero-pill discipline.
- **Component Styling & Layout**:
  - *Mobile (375px–430px)*: Full `100dvh` flex column keeping sticky top chrome compact (54px) and all primary actions (input, 48×48px mic button, send button) inside the natural lower thumb zone with safe-area bottom padding. All touch targets are at least `44×44px`.
  - *Desktop (1024px–1440px+)*: Full-viewport two-column layout with a 280px collapsible history & preferences sidebar on the left and a spacious, centered reading column (`max-w-3xl`) on the right—never a narrow phone strip floating in empty space.
  - *Visual Asset*: One custom-generated futuristic blue-and-obsidian geometric core emblem for BUDDY (with a resilient SVG fallback so it never breaks).

### Interactive Feedback & Motion

- Smooth `150–200ms` transitions (`cubic-bezier(0.16, 1, 0.3, 1)`) animating only compositor properties (`transform`, `opacity`), respecting `prefers-reduced-motion`.
- Live Web Audio waveform bars respond to actual microphone amplitude while recording.
- Streaming typing indicator while BUDDY thinks, real audio loading/playing indicators on spoken replies, and clear inline error banners with a **Retry** action if any network or API call fails.

---

## 3. Key Product Decisions & Trade-Offs

- **Decision 1 — Real Server-Side Gemini Transcription (`gemini-3.5-transcribe`) for Microphone Input**
  - *Chosen Approach*: Capture microphone audio via `MediaRecorder` and transcribe it server-side with `gemini-3.5-transcribe` (free tier).
  - *Why*: Browser `webkitSpeechRecognition` is inconsistent across Android WebViews/iframes/Firefox and frequently breaks on Hinglish and Desi Punjabi code-switching. Real audio capture + Gemini transcription works uniformly across devices and accurately preserves spoken Hindi, Punjabi, and English.
  - *Alternatives Considered*: Browser-only `SpeechRecognition` API (unreliable for Punjabi and unsupported in several browsers).

- **Decision 2 — Real Spoken Replies (`gemini-3.8-flash-lite-tts`) with Graceful Mute & Replay**
  - *Chosen Approach*: Generate crisp WAV speech audio via a server-side endpoint calling `gemini-3.8-flash-lite-tts` (free tier), played via standard browser audio when Voice is unmuted or when a user taps **Listen** on any message.
  - *Why*: Standard browser `speechSynthesis` often lacks Punjabi/Hindi voices on many devices or sounds robotic. Gemini TTS delivers warm, natural multilingual speech with zero paid subscription required.
  - *Alternatives Considered*: Browser `window.speechSynthesis` only (inconsistent voice availability for Punjabi across platforms).

- **Decision 3 — Fast Multilingual Chat Streaming (`gemini-4-hydrogen`)**
  - *Chosen Approach*: Stream responses via Server-Sent Events (SSE) using `gemini-4-hydrogen` with a carefully crafted system persona for BUDDY that encodes friendly futuristic intelligence, script-matching rules, and authentic Desi Punjabi conversational style.
  - *Why*: Free-tier compatible, low latency for chat, and strong multilingual fluency across English, Hindi, and Punjabi (both Roman and native scripts).
  - *Alternatives Considered*: Non-streaming chat calls (slower perceived speed on mobile) or paid-only models (violates free-only requirement).

- **Decision 4 — Local Device Storage (`localStorage`) for Conversations**
  - *Chosen Approach*: Persist chat threads, active thread selection, language preference, and voice mute state in structured `localStorage` with safe JSON recovery.
  - *Why*: Matches user preference ("Save chats on this device only"), works instantly offline for reading past chats, and avoids any login friction or server database setup.

---

## 4. Technical Architecture & Data Strategy *(Technical Reference)*

### Architecture & Component Diagram

```
┌───────────────────────────────────────────────────────────────────────────┐
│                         CLIENT (React + Tailwind)                         │
│                                                                           │
│  ┌──────────────────────┐     ┌────────────────────────────────────────┐  │
│  │  Local Storage Store │◄───►│          App Workspace Shell           │  │
│  │  • Chat Threads      │     │  • Mobile Slide Drawer / Desktop Rail  │  │
│  │  • Voice Mute State  │     │  • Compact Top Bar (BUDDY + Mute)      │  │
│  │  • Language Mode     │     └──────────────────┬─────────────────────┘  │
│  └──────────────────────┘                        │                        │
│                                  ┌───────────────┴───────────────┐        │
│                                  ▼                               ▼        │
│                     ┌────────────────────────┐      ┌──────────────────┐  │
│                     │   Chat Message Feed    │      │  Thumb Composer  │  │
│                     │  • Zero-State Starters │      │  • Auto Textarea │  │
│                     │  • Streamed Bubbles    │      │  • Mic Recorder  │  │
│                     │  • Per-Msg Play / Stop │      │    (Web Audio)   │  │
│                     │  • Retry Error Banner  │      │  • Send Trigger  │  │
│                     └───────────┬────────────┘      └────────┬─────────┘  │
└─────────────────────────────────┼────────────────────────────┼────────────┘
                                  │                            │
                   POST /api/tts  │  POST /api/chat (SSE)      │ POST /api/transcribe
                                  ▼                            ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                     EXPRESS SERVER PROXY (Node / TS)                      │
│  • Shared @google/genai client (process.env.GEMINI_API_KEY)               │
│  • /api/chat       → gemini-4-hydrogen (SSE stream + BUDDY persona)       │
│  • /api/transcribe → gemini-3.5-transcribe (audio/webm|mp4 → text)        │
│  • /api/tts        → gemini-3.8-flash-lite-tts (text → audio/wav)         │
│  • Validates payloads, cleans markdown for TTS, propagates real errors    │
└───────────────────────────────────────────────────────────────────────────┘
```

### Data Model & State

- **Conversation Thread**: `{ id, title, createdAt, updatedAt, messages: Message[] }`
  - Title auto-generates from the first user message (trimmed cleanly to 1 line).
- **Message**: `{ id, role: 'user' | 'assistant', content, createdAt, inputMode: 'text' | 'voice', isStreaming?, error? }`
- **Assistant Preferences**:
  - `voiceEnabled: boolean` (default `true`; toggled anytime via the top bar Mute/Unmute button)
  - `languageMode: 'auto' | 'english' | 'hindi' | 'punjabi'` (default `'auto'` — matches user language & script)
- **Live Audio / Recording State**:
  - `recordingState: 'idle' | 'requesting' | 'recording' | 'transcribing'`
  - `audioLevels: number[]` (real-time microphone frequency samples from Web Audio `AnalyserNode`)
  - `speakingMessageId: string | null` and `ttsLoadingId: string | null` (tracks active audio playback so only one reply speaks at a time, with instant cancellation when muted or stopped).

### Interactive Component & State Mapping

- **Starter Prompt Cards (Zero-State)**: Tapping any multilingual starter immediately creates/uses the active thread and dispatches the message through the real `/api/chat` stream.
- **Thumb Composer (Text Input + Send)**: Validates non-empty trimmed input, supports Enter-to-send (Shift+Enter for newline), disables duplicate sends while streaming, and offers a **Stop Generating** button mid-stream via `AbortController`.
- **Microphone Recorder Button**: Requests real mic stream via `getUserMedia`, starts `MediaRecorder` with compatible Android/desktop MIME type (`audio/webm` / `audio/mp4`), visualizes live mic levels, and handles **Cancel** (discards audio cleanly and releases tracks) or **Stop & Send** (converts blob to base64 → `/api/transcribe` → dispatches transcribed text as a chat message).
- **Voice Mute Toggle & Per-Message Speaker Control**: Muting immediately pauses and clears any active `<audio>` element; unmuting enables automatic spoken playback for subsequent replies. Clicking **Listen** on any past BUDDY message fetches (and caches in-memory for the session) its WAV audio from `/api/tts` and plays it; clicking **Stop** halts playback immediately.
- **Sidebar / Mobile Drawer**: Full CRUD for local chats (`New Chat`, select thread, delete thread with instant fallback to another thread or fresh zero-state) plus segmented language-mode buttons (`Auto`, `English`, `Hindi`, `Punjabi`).

### Key Sequences & Integrations

1. **SDK & Server Setup**: Upgrade `@google/genai` to `>= 2.29.1` (required for `gemini-4-hydrogen` and `gemini-3.8-flash-lite-tts` `speechMetadata`), and run an Express server hosting both the `/api/*` Gemini proxy routes and the Vite frontend on port 3000.
2. **Desi Punjabi & Script-Matching System Persona**: System instructions enforce BUDDY's warm, sharp, futuristic-friend tone, explicit script mirroring (Roman input → Roman output; Devanagari → Devanagari; Gurmukhi → Gurmukhi), and authentic conversational Desi Punjabi expressions when speaking Punjabi.
3. **Error Propagation**: Any missing mic permission, network interruption, or Gemini API error surfaces directly to the user in plain language with a working **Retry** button—never replaced with fake/simulated replies.
