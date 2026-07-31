# DuoTrack

> A real-time, two-person study accountability app inspired by the focused-study workflow of YPT (Yeolpumta).

[![Live demo](https://img.shields.io/badge/Live%20demo-GitHub%20Pages-246b45?style=for-the-badge&logo=github)](https://abhishekpathak-800.github.io/ypt/)
[![Built with](https://img.shields.io/badge/Built%20with-HTML%20%2B%20CSS%20%2B%20JavaScript-f4c542?style=for-the-badge)](./index.html)
[![Sync](https://img.shields.io/badge/Sync-Firebase-ffca28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/database)

DuoTrack combines a synchronized study timer, partner accountability, task planning, live chat, calls, reports, reminders, and a study-focused AI assistant in one responsive web page. It is a personalized prototype designed primarily for two trusted users.

**Live app:** [abhishekpathak-800.github.io/ypt](https://abhishekpathak-800.github.io/ypt/)

> [!IMPORTANT]
> This is a client-side prototype, not production authentication. Account records and administrative controls are implemented in browser code/Firebase rather than a secure server. Do not reuse an important password or deploy the current security model for untrusted users. Read [Security](./SECURITY.md) before forking or extending the app.

## Highlights

- **Flexible study timer:** stopwatch and countdown modes, quick durations, custom durations, pause/resume, and selectable alarm sounds.
- **Study classification:** track Physics, Chemistry, and Maths in Lecture or Practice mode.
- **Live accountability:** see both users' current status, today's total, yesterday's total, subject split, mode split, questions solved, and XP.
- **Daily planning:** sortable to-do list, quick-add study stages, priority tags, completion progress, and a D-Day countdown.
- **Realtime study chat:** typing indicator, replies, reactions, image attachments, editing, deletion, unread counts, sounds, and browser notifications.
- **Voice and video calls:** WebRTC calling with mute, camera toggle, and screen sharing during video calls.
- **Study AI:** Groq-powered assistant with KaTeX math rendering, optional search context, an in-app panel, and `@grok`/`@a9` chat commands.
- **Personalization:** shared themes, personal night mode, custom device background, profile image, mascot, and optional football styling.
- **Reminders and exports:** hydration/check-in reminders, an eight-hour milestone sound, comparison PDF, chat PDF, appearance import/export, and full JSON backup/restore.
- **JEE motivation:** an interactive presentation inside the app with study and exam-focused slides.

## Documentation

- [User guide](./docs/USER_GUIDE.md) — how to use timers, chat, calls, AI, settings, and backups.
- [Architecture](./docs/ARCHITECTURE.md) — code layout, data flow, persistence, integrations, and extension points.
- [Security](./SECURITY.md) — current prototype risks and the changes required before wider deployment.

## Quick start

DuoTrack has no build step and no package manager. It is a static site whose application code lives in `index.html`.

### 1. Clone the repository

```bash
git clone https://github.com/abhishekpathak-800/ypt.git
cd ypt
```

### 2. Start a local web server

Using Python:

```bash
python -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000).

Using a local server is recommended because ES modules, CORS rules, media permissions, and browser APIs may not work correctly when `index.html` is opened through a `file://` URL.

### 3. Configure your own deployment

The checked-in page is connected to the existing DuoTrack Firebase project and demo group. For a fork or independent deployment, update these values in `index.html`:

| Setting | Purpose |
| --- | --- |
| `firebaseConfig` | Connects the browser to your Firebase web app and Realtime Database. |
| `GROUP_ID` | Selects the shared group path used for users, chat, calls, and settings. |
| Account/authentication code | Must be replaced with Firebase Authentication or another real authentication system for public use. |
| Firebase Security Rules | Decide who may read or modify each database path. They are the actual security boundary. |
| Groq configuration | The current prototype accepts a shared key through Settings; a production version should call Groq through a protected backend. |

Firebase web configuration is visible by design in frontend apps. Never commit Firebase service-account credentials or other private server keys.

## Deploy with GitHub Pages

1. Keep `index.html` in the repository root.
2. Open **Repository Settings → Pages**.
3. Choose **Deploy from a branch**.
4. Select the `main` branch and `/ (root)` folder.
5. Save and wait for the deployment to finish.

For this repository, the resulting project site is:

```text
https://abhishekpathak-800.github.io/ypt/
```

## Tech stack

| Layer | Technology |
| --- | --- |
| UI | Semantic HTML, responsive CSS, vanilla JavaScript |
| Realtime state | Firebase Realtime Database 10.12.5 |
| Calls | WebRTC with Firebase-based signaling and public STUN servers |
| AI | Groq OpenAI-compatible chat-completions API |
| Math | KaTeX 0.16.9 |
| PDF export | jsPDF 2.5.1 |
| Search context | DuckDuckGo Instant Answer API and Wikipedia search API |
| Hosting | GitHub Pages |

Dependencies are loaded from CDNs at runtime, so the app needs internet access even when served locally.

## Project structure

```text
ypt/
├── index.html             # Complete app: markup, styling, state, and integrations
├── README.md              # Project overview, setup, and deployment
├── SECURITY.md            # Prototype security guidance
└── docs/
    ├── ARCHITECTURE.md    # System design and data flow
    └── USER_GUIDE.md      # End-user instructions and troubleshooting
```

## Browser permissions and external services

Some features require explicit browser permission:

- **Notifications** for chat and study check-ins.
- **Microphone** for voice/video calls.
- **Camera** for video calls.
- **Screen capture** for screen sharing.

Study state, shared appearance, chat, call signaling, registered prototype accounts, and shared AI configuration use Firebase. Personal browser preferences such as session memory, notification choices, night-mode override, selected timer settings, welcome popup, and reminders use browser storage. See [Architecture](./docs/ARCHITECTURE.md#persistence) for the full split.

## Known limitations

- The app is optimized for exactly two study partners; extra registered accounts do not receive a true multi-user partner view.
- Authentication and settings protection are client-side prototype mechanisms.
- The current app and styles are kept in one large HTML file, which makes testing and maintenance harder.
- Calls use STUN without a dedicated TURN relay, so some restrictive networks may prevent a connection.
- The AI key is shared through the group database and requests are made from the browser.
- There is no automated test suite, offline mode, or service worker.

## Suggested roadmap

- Move accounts to Firebase Authentication and lock down Realtime Database rules.
- Proxy AI requests through a serverless function so API keys never reach clients.
- Split `index.html` into reusable HTML/CSS/JavaScript modules.
- Add automated tests for timer rollovers, synchronization, backups, and chat actions.
- Add true multi-group and multi-user support.
- Add a TURN service for reliable calls across restrictive networks.
- Add a web app manifest, service worker, and offline-friendly timer state.

## Contributing

1. Fork the repository and create a focused branch.
2. Keep changes limited to one feature or fix.
3. Run the app through a local HTTP server.
4. Test at desktop and mobile widths.
5. Verify Firebase sync and any permission-dependent feature you changed.
6. Open a pull request explaining the behavior before and after the change.

Please do not include real passwords, private API keys, personal chat exports, or Firebase service-account files in commits.

## Project status and attribution

DuoTrack is an independent personal project by Abhishek. It is inspired by study-accountability workflows but is not affiliated with or endorsed by Yeolpumta/YPT.

This repository does not currently include a software license. Add an explicit license before inviting third-party reuse or distribution.
