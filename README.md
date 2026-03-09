# Bluesky Auto Live Status for Streamers

Automatically set your Bluesky profile to **LIVE** when you start streaming, and clear it when you're done — directly inside OBS.

Works on **Windows and Linux**. No bat files. No extra bot software. All configuration is done through a settings panel inside OBS itself.

---

## What You'll Need

- [OBS](https://obsproject.com/) *(already installed if you're streaming)*
- [Python 3](https://www.python.org/downloads/) — pre-installed on Linux; Windows users will need to install it
- A [Bluesky](https://bsky.app) account

---

## Step 1 — Install Python *(Windows only)*

Linux users can skip this step — Python 3 is already on your machine.

1. Go to [python.org/downloads](https://www.python.org/downloads/) and download the latest version
2. Run the installer — on the first screen, check the box that says **"Add Python to PATH"** before clicking Install
3. Once installed, open **Command Prompt** and run `where python` to confirm it worked and get the full path (e.g. `C:\Users\YourName\AppData\Local\Programs\Python\Python312\python.exe`) — you'll need this in Step 3

---

## Step 2 — Get a Bluesky App Password

You'll use an **App Password** instead of your real Bluesky password. This keeps your account safe — if it's ever accidentally exposed, you just delete it and make a new one without changing your login.

1. Log into Bluesky
2. Go to **Settings → Privacy and Security → App Passwords**
3. Click **Add App Password**, give it a name like `stream-bot`
4. Copy the password it gives you — it looks like `xxxx-xxxx-xxxx-xxxx`

---

## Step 3 — Point OBS to Python

Before adding any scripts, OBS needs to know where Python is installed.

1. In OBS, go to **Tools → Scripts**
2. Click the **Python Settings** tab at the top of the Scripts window
3. Paste your Python path into the **Python Install Path** field:
   - **Windows:** the path you copied in Step 1, e.g. `C:\Users\YourName\AppData\Local\Programs\Python\Python312`  *(the folder, not the .exe)*
   - **Linux:** run `which python3` in a terminal and paste the result, e.g. `/usr/bin/python3`
4. Click **Close**

> If you don't see a Python Settings tab, your version of OBS has Python support built in and you can skip this step.

---

## Step 4 — Download the Scripts

Save these files somewhere stable on your computer:
- **Windows:** e.g. `C:\Users\YourName\streaming\bluesky\`
- **Linux:** e.g. `~/streaming/bluesky/`

Files:
- `bsky-live-status.py` — sets your LIVE badge when you go live, clears it when you stop
- `post-to-bluesky.py` *(optional)* — posts to your Bluesky feed when you go live

You don't need to open or edit them. All setup is done inside OBS.

---

## Step 5 — Add the Scripts to OBS

For each script you want to use:

1. In OBS, go to **Tools → Scripts**
2. Click the **+** button at the bottom left
3. Navigate to where you saved the `.py` file and select it
4. The script will appear in the list — click it to see its settings panel on the right

---

## Step 6 — Fill In Your Details

Click each script in the list to open its settings panel on the right side of the window.

**bsky-live-status.py:**

| Field | What to enter |
|---|---|
| Bluesky Handle | Your handle, e.g. `yourname.bsky.social` |
| App Password | The password from Step 2 *(shown as dots)* |
| Stream URL | Your stream link, e.g. `https://twitch.tv/yourchannel` |
| Link Card Title | Shown on the clickable card, e.g. `Live Now!` |
| Link Card Description | e.g. `Come hang!` |
| Duration (minutes) | Leave at `240` — this is the maximum Bluesky allows |

**post-to-bluesky.py** *(if using)*:

| Field | What to enter |
|---|---|
| Bluesky Handle | Same as above |
| App Password | Same as above |
| Post Text | Your announcement, e.g. `https://twitch.tv/yourchannel Come hang! #YourHashtag` |

> **Post Text tips:** URLs and `#hashtags` are made clickable automatically. Keep it under **300 characters** (Bluesky's limit). Update this field whenever your stream content changes.

Settings save automatically — there's no save button needed.

---

## Step 7 — Test It

1. In OBS, start a stream as normal
2. Check your Bluesky profile — the LIVE badge should appear within a few seconds *(you may need to refresh)*
3. End the stream — the badge clears automatically

To check for errors, click **Script Log** at the bottom of the Tools → Scripts window. Any issues will appear there.

---

## Also Included — Auto Post on Stream Start *(optional)*

`post-to-bluesky.py` posts to your Bluesky feed when you go live. Add it to OBS the same way as `bsky-live-status.py` — both scripts can be loaded at the same time and will fire together when you start streaming.

---

## Troubleshooting

**The badge never shows up**
Make sure the Duration field is set to `240`. Bluesky requires a duration value to display the LIVE badge — it won't appear without one.

**Auth failed error in the Script Log**
Double-check your Bluesky Handle and App Password in the settings panel. Make sure you're using an App Password, not your regular login password.

**OBS doesn't load the script / Python error**
Make sure the Python path is set correctly in the Python Settings tab (Step 3). The path should point to the Python folder, not the executable itself.

**Script loads but nothing fires**
These scripts respond to streaming events specifically — not recordings. Make sure you're starting an actual stream, not just a local recording.

**OBS installed via Flatpak (Linux) and Python isn't working**
Flatpak OBS runs in a sandbox that can interfere with Python support. The easiest fix is to install OBS via your distro's package manager instead (e.g. `sudo apt install obs-studio` on Ubuntu/Debian). Alternatively, see the [OBS Flatpak Python guide](https://obsproject.com/forum/resources/flatpak-obs-python-scripting.1571/) on the OBS forums.

---

## Notes

- Bluesky enforces a maximum status duration of **4 hours (240 minutes)** regardless of what you set. The badge clears automatically when you end your stream — the duration is just a safety fallback in case OBS closes unexpectedly.
- Each streamer needs their own OBS with the scripts loaded and their own credentials filled in via the settings panel.
- If your App Password is ever accidentally shared or exposed, revoke it immediately in Bluesky settings and generate a new one. Your main account password is never at risk.

---

*Built using the [AT Protocol](https://atproto.com/) and [Bluesky lexicons](https://github.com/bluesky-social/atproto/tree/main/lexicons/app/bsky/actor).*
