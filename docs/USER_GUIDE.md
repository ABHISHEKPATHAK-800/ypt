# DuoTrack user guide

This guide covers the current browser version of DuoTrack. The interface is designed for two study partners who share live progress through Firebase.

## Sign in

1. Open the [live app](https://abhishekpathak-800.github.io/ypt/) or a locally served copy.
2. Enter an authorized login ID and password.
3. Wait for the top bar to show that Firebase is connected before starting a session.

Do not share credentials publicly or reuse an important password. The current login system is prototype-level and runs in frontend code.

## Run a study session

1. Choose **Physics**, **Chemistry**, or **Maths**.
2. Choose **Lecture** or **Practice** mode.
3. Leave the timer in **Stopwatch** mode, or use the arrow control to switch to **Countdown**.
4. In countdown mode, select a quick duration or enter a custom duration, then choose an alarm sound.
5. Select **Start focus**. Use the same control to pause the session.

While the timer is running, DuoTrack keeps the live duration on screen and periodically checkpoints the session to Firebase. Study totals are grouped by the `Asia/Kolkata` calendar day.

The dashboard shows:

- total study time today and yesterday;
- current subject and current timer status;
- Lecture and Practice totals;
- questions solved today;
- subject-wise totals and XP in the partner view.

## Record questions solved

Use the **Qs done** button in the top bar, enter today's solved-question count, and save. The count appears in the dashboard, partner comparison, history, and generated report.

## Plan the day

### Quick-add stages

Use the `L-0` to `L-3` shortcuts to add common revision, lecture, practice, and test tasks.

### Custom tasks

1. Enter a task in the to-do field.
2. Optionally select a tag: urgent, low priority, deadline/stress, or important.
3. Add the task.
4. Check it off when complete, drag it to reorder, or remove it.

The circular indicator shows completed tasks versus total tasks.

### D-Day

Enter a name and date in the D-Day card. DuoTrack displays the number of days remaining and changes the visual stage as the date approaches.

## Use the study-partner view

The **Study Partner** card displays the other user's live status. Open it for a comparison of study duration, modes, subjects, questions, and recent history.

The current implementation assumes a two-person group. If more accounts are registered, it selects the first other account it finds rather than building a complete group leaderboard.

## Study Chat

Open **Chat** from the lower-right corner. The current chat supports:

- realtime messages and typing status;
- replies and emoji reactions;
- image attachments;
- editing your message;
- deletion for yourself or, where allowed, for everyone;
- unread counts, sounds, and browser notifications;
- a larger chat-panel mode.

Images are compressed into data URLs before they are stored. Large or frequent image messages can increase Realtime Database usage quickly.

## Study AI

An authorized user must first save a Groq API key in **Settings → AI Assistant (Groq)**.

You can then:

- open the AI button in the lower-left corner and ask a question;
- type `@grok` followed by a question in Study Chat for a normal AI reply;
- type `@a9` followed by a question for the alternate roast-style reply.

The assistant supports KaTeX-formatted maths. It also attempts a best-effort lookup through DuckDuckGo and Wikipedia for current or factual questions. AI output can still be wrong, so verify important formulas, dates, and exam information.

The current prototype sends recent relevant chat context with AI requests. Do not place secrets or highly private information in chat when AI integration is enabled.

## Study Music

An administrator must first open **Settings → YouTube Music**, enable the feature, enter a YouTube Data API v3 key, and save. This setting and key are shared by both accounts.

To play music:

1. Enter a song, artist, or study-mix query in the **Study Music** panel.
2. Choose **Search** to load up to ten music-category video results.
3. Use the circular button beside a result to start or pause it.
4. Use the cassette card to resume or pause playback, or select the card to open the full embedded YouTube controls.
5. Choose **Stop** in the player dialog to end playback and clear the now-playing state.

While a track is playing, the listener's avatar changes to the video's thumbnail with a music badge. DuoTrack synchronizes the video ID, title, thumbnail, and play/pause state through Firebase so both study partners can see it. The media itself is served directly by YouTube and is not sent through Firebase.

Search depends on YouTube Data API quota and key restrictions. Playback also depends on the selected video being available and allowing embedded playback.

## Voice calls, video calls, and screen sharing

Use the phone or camera button in the Study Chat header.

- A voice call asks for microphone permission.
- A video call asks for microphone and camera permission.
- During a video call, the screen button can replace the outgoing camera track with a shared screen.
- Use the call controls to mute, disable the camera, return from screen sharing, or end the call.

Calls are peer-to-peer through WebRTC, while Firebase carries the connection offer, answer, and ICE candidates. Because there is no TURN relay, calls may fail on restrictive school, office, carrier, or VPN networks.

## Settings

Settings include:

- reset both users' timers for today;
- download a comparison PDF;
- toggle chat sound;
- select a shared theme;
- set a custom background on the current device;
- export or import appearance settings;
- configure the welcome popup;
- configure hydration and study check-in reminders;
- add a custom eight-hour milestone sound;
- configure the shared Groq key;
- enable YouTube Music and configure its shared YouTube Data API key;
- download or administer chat history;
- download or restore a full backup;
- register another prototype account.

Some settings are shared through Firebase and others are personal to the current browser. A full restore replaces shared group data and cannot be undone, so download a fresh backup first.

## Reports and backups

| Tool | Output | Scope |
| --- | --- | --- |
| Comparison report | PDF | Current study comparison and recent history |
| Chat history | PDF | Shared chat messages available to the app |
| Appearance export | JSON | Theme/background-related settings |
| Full backup | JSON | Shared group state, accounts, chat, appearance, and history |

Treat exported files as private: they may contain names, study history, chat content, images, account records, or configuration values.

## Troubleshooting

### The app stays on “Connecting Firebase”

- Confirm that the device is online.
- Reload once.
- Check whether the Firebase project/database is available.
- If using a fork, confirm `firebaseConfig`, `databaseURL`, `GROUP_ID`, and database rules.

### The app does not work when double-clicking `index.html`

Serve it over HTTP instead:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

### Notifications do not appear

- Allow notifications in the browser's site settings.
- Check the notification toggle in Study Chat.
- Some browsers suppress notifications while the page is active.

### A call does not connect

- Allow microphone/camera access on both devices.
- Try without a VPN or restrictive network.
- Confirm both users are online and only one call is active.
- A production deployment should add a TURN relay.

### Study AI fails

- Confirm a valid Groq key is saved.
- Check the Groq account's limits and model availability.
- Confirm the browser can reach the Groq API.
- Remember that search context is best-effort and may be blocked independently.

### YouTube Music search or playback fails

- Confirm the feature is enabled and a valid YouTube Data API v3 key is saved in Settings.
- Check that the key allows the YouTube Data API v3 and the current localhost or GitHub Pages HTTP referrer.
- Check whether the project's YouTube API quota has been exhausted.
- Try another result if the selected video is unavailable or does not allow embedded playback.
- Serve the app through localhost or GitHub Pages instead of opening `index.html` directly.

### Data looks out of date

- Wait for Firebase connectivity to recover, then reload.
- Do not run a second conflicting timer session for the same account on multiple devices.
- Before manual recovery, download a full backup if the settings page is still accessible.
