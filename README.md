# Stage Timer

A minimal, offline countdown timer for events with several speakers, inspired by [stagetimer.io](https://stagetimer.io).
Plain HTML and JavaScript, no build step and no dependencies. The speaker blocks live in a text file.

- Big countdown with speaker, description, progress bar and the next speaker
- Turns yellow at the warning time and red at zero, then counts into overtime (`−01:23`)
- Keyboard shortcuts for laptops, large buttons for phones and tablets
- Keeps the screen on
- Works offline and can be installed on a phone's home screen
- Remembers the agenda and the current position across reloads

## Agenda file

One block per line: `duration | speaker | description | warn (optional)`

```
# Lines starting with # are ignored.
warn = 2                                   # default warning time for all blocks

5     | Anna Berger | Welcome & agenda
15:30 | Julia Weiss | Customer story | 3   # this block warns at 3 minutes
10    | —           | Coffee break
```

Durations and warning times are written as `10` (10 min), `10:30` (10 min 30 s) or `1:15:00` (1 h 15 min).
See [agenda.txt](agenda.txt) for a full example.

Load the file with **Open agenda file…** or by dragging it onto the window. After editing it, load it again with
**O** or ☰ → **Open agenda file…**. The current block and time are kept.

## Controls

| Key | Button | Action |
|---|---|---|
| `Space` | ▶ / ⏸ | Start / pause |
| `→` `PgDn` | ⏭ | Next block |
| `←` `PgUp` | ⏮ | Previous block |
| `R` | | Restart the current block |
| `+` / `-` | | Add or remove 1 minute |
| `A` | ☰ | Agenda: tap a block to jump to it |
| `O` | ☰ → Open agenda file… | Load or reload the agenda file |
| `F` / double-click | ☰ → Fullscreen | Toggle fullscreen |
| `H` / `Esc` | | Show or hide help |

Presentation clickers usually send `PgUp`/`PgDn`, so they switch blocks.

## Keeping the screen on

The app uses the browser's [Screen Wake Lock API](https://developer.mozilla.org/en-US/docs/Web/API/Screen_Wake_Lock_API).
If that's refused, it falls back to a silent looping video that stays hidden behind the page.
The top bar shows which method is active:

| Top bar | Meaning |
|---|---|
| `awake: wake lock` | Everything's fine. |
| `awake: video (…)` | The fallback is in use. The reason is in brackets. |
| `⚠ screen may sleep — tap anywhere` | Neither method is running yet. Tap once. |

Chrome refuses the Wake Lock for pages opened as a local file (`file://`). **On phones, use the hosted version.**
Battery saver can also block both methods.

## Use it

**Hosted (recommended, especially on phones):** open the GitHub Pages URL once while online. On Android Chrome,
choose ⋮ → **Add to Home screen**. On an iPhone in Safari, choose Share → **Add to Home Screen**. From then on it
opens from the icon in fullscreen, even without internet. **Load example** on the start screen loads the sample
agenda.

**Local (laptop):** double-click `index.html`. Everything works except that Chrome and Edge don't grant the
Wake Lock to local files, so the video fallback is used.

## Deploy to GitHub Pages

1. Push this folder to a GitHub repository.
2. In the repository, go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**,
   select `main` and `/ (root)`, and save.
3. After a minute, the app is available at `https://<user>.github.io/<repo>/`.

**Updating:** the service worker serves the cached version first and fetches the new one in the background, so an
update appears on the second load. When you change files, also bump `CACHE` in [sw.js](sw.js) (for example to
`stagetimer-v2`) so the old cache is cleared.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `agenda.txt` | Example agenda |
| `sw.js` | Service worker for offline use |
| `manifest.webmanifest`, `icon-*.png` | Home-screen install |

## Credits

The keep-awake fallback video comes from [NoSleep.js](https://github.com/richtr/NoSleep.js) by Rich Tibbett (MIT license).
