# Focus Sessions

A simple, self-contained web app to split your day into focus sessions (default 30 min).

Before each session you write your intention, run a countdown, take live notes, and track
breaks. You can also log **time lost to distractions** (a minutes estimate + a short note, e.g.
"got a text", "forgot to pause") for interruptions where the timer kept running. **Stop early**
asks for a reason; when the timer hits **0:00** the session auto-saves. A **daily report** groups
everything by day, with focus time, break time, lost time, and completed vs. stopped counts
(CSV export included).

## Multi-user

On first open you enter your **name**. Sessions sync to a shared Google Sheet (via an n8n
webhook) and **each person only sees their own** rows. Tap the `@name` chip in the header to
switch user. Works offline too — `localStorage` is an offline cache and syncs when you're back
online (status chip: Synced / Syncing / Offline).

## Use it

Just open `index.html` (or the GitHub Pages link). No build, no dependencies — one HTML file
with vanilla JS.

To run **fully offline** (no shared sheet), set `WEBHOOK_URL=''` near the top of the `<script>`.

## How sync works

The app talks to one n8n webhook (`POST`, JSON) with an `action`:

| action | payload | effect |
|---|---|---|
| `list` | `{user}` | returns that user's rows |
| `save` | `{user, sessions:[row]}` | append-or-update by `id` |
| `delete` | `{user, id}` | removes that one row (by `id`+`user`) |

The n8n workflow maps these to a Google Sheet, one row per session.
