# Metrik Runs

A small, dependency-free tracker for the task runs of an AI agent on Hup Social. It answers one question: which tasks run clean, and which need pushing, editing or fixing.

Agent chat history can't be exported, and logging runs by hand doesn't last. So you paste the agent's reply, the app reads it, and you confirm with a tap.

## How it works

1. **Log.** Paste the agent's reply. The app guesses the task, the outcome (drafted, skipped, error) and whether the run stalled, and shows the phrase it matched.
2. **Confirm.** Fix anything wrong, set what you did with the draft (approved, edited, discarded) and tap Save.
3. **Health.** Per-task clean rate, pushes, wrong skips, discards and edits, with a short pointer when the counts justify one.
4. **History.** Every run, with its result. Drafts left unresolved can be settled later.
5. **Overdue alert.** Set each task's schedule in Setup. A task with no log for 1.5 times its interval shows a banner, so a forgotten run leaves a visible gap.

The classifier is keyword rules, not a model. When you correct a guess, the app stores that reply and matches similar ones next time. There is no API key and no cost.

## Data and sync

- **Browser storage.** Everything is saved in the browser on each device.
- **GitHub sync (optional).** Connect a **private** repository and the app keeps a single file, `metrik-runs-data.json`, in it. This gives you:
  - the same log on every device
  - a copy that survives cleared site data
  - version history from the commits
- **Merging.** Runs merge by ID, the newest edit wins, and deletions are tracked so they don't come back. Task names and schedules are last-write-wins.
- **Offline.** Changes stay in the browser and sync when the connection returns.

### Connecting a repo

1. Create a **private** repository with at least one file, for example a README.
2. On GitHub, go to Settings, Developer settings, Fine-grained tokens and generate a token.
3. Under Repository access, choose Only select repositories and pick that repo.
4. Under Repository permissions, set **Contents** to **Read and write**. Leave everything else off.
5. In the app, open Setup, then Sync. Enter `owner/repo` and the token, then tap Connect and sync.

The token is stored in the browser only. It is never written to the repo file or to exports. Fine-grained tokens expire; when it lapses the status turns red and logging keeps working locally until you paste a new one.

Do not point the app at a public repository. The log holds run notes.

### Backup

Setup has Export file, Copy as text and Import file. The app reminds you if you haven't exported in 7 days. Import merges into the existing log and doesn't overwrite it.

## Limits

- **Classifier.** It knows the phrasings seen so far. A new phrasing may get a wrong or blank guess until you correct it once.
- **File size.** GitHub's contents API returns a file inline up to 1 MB, which is roughly 2,500 runs. Past that, sync will fail until the app reads the file differently or splits the log by month.
- **What it can't see.** Approved, edited and discarded are your taps. The paste only shows what the agent said.
- **One writer at a time for tasks.** Editing task names or schedules on two devices at once keeps only the later save.

## Running it

One HTML file with inline CSS and JavaScript. No build step, no dependencies.

- Open the file in a browser, or serve it from GitHub Pages.
- After updating the file on Pages, refresh. If the old version persists, close and reopen the tab.

## Privacy

Everything stays in your browser and, if you connect one, your own repository. The app makes requests only to `api.github.com`, and only when sync is on.