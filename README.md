# evanbarth.com

My personal site. One `index.html`, no build step, no framework, no tracking. I wanted something that looks simple at first glance but has a few things to find if you poke around.

## What's in it

- **One file.** All the HTML, CSS and JavaScript live in `index.html`, so I can drop it on any static host.
- **Dot field background.** A canvas grid that ripples on its own, reacts to the cursor, and sends a shockwave when you click.
- **My E₿ logo.** A hand-drawn SVG hexagon that draws itself on load and tilts toward the cursor. The B has the Bitcoin strokes, and they turn orange on hover. Click it and see what happens.
- **Boot sequence.** A quick startup log the first time someone visits in a session. Any key or click skips it.
- **A real(ish) terminal.** Press `/` or click **open terminal**. It has history, tab completion, and a few secrets.
- **Themes.** Sky is my default: dark navy-black with sky blue and hot pink. Mint, violet, amber, rose and ice are there too. Click the pulsing dot next to my location or run `theme <name>`. It remembers the pick.
- **Hidden email.** My address is stored encoded and only gets put together in the browser when someone clicks, so it's never sitting in the page source.
- **My local time.** The status line shows what time it is here in Springfield.
- **Accessible.** Keyboard friendly, visible focus states, labels for screen readers, and everything holds still for anyone with reduced motion turned on.

## Terminal

| Command | What it does |
| --- | --- |
| `help` | Lists the commands |
| `howdy` | Says hi |
| `whoami` | A little about me |
| `neofetch` | My "system info" card |
| `ls` | My links |
| `open <linkedin\|github\|email>` | Opens one |
| `email` | Reveals and copies my email |
| `theme [name]` | Lists or switches the accent color |
| `matrix` | Follow the white rabbit |
| `coffee` | Brews a cup |
| `date` | My local time |
| `echo <text>` | Prints whatever you type |
| `clear` | Clears the screen |
| `exit` | Closes the terminal |

**Shortcuts:** `/` or `` ` `` opens it, `Esc` closes it, `Tab` completes, `↑` / `↓` scroll back through history.

<details>
<summary>Spoilers (don't open if you'd rather find them yourself)</summary>

- Commands that aren't listed: `sudo`, `rm`, `hello`, `hi`, `ping`, `moon`
- Double-click (or double-tap on a phone) anywhere on the background for a quick matrix rain that spreads out from where you clicked
- The Konami code (`↑ ↑ ↓ ↓ ← → ← → B A`) or `matrix` runs the long version
- Clicking my logo throws ₿ coins and glitches my name. Click it five times fast (or type `moon` in the terminal) and it goes to the moon: the logo hyperspins, the Bitcoin logo flashes over it, coins fly up and rockets launch

</details>

## Notes to self: changing things

Everything personal is in two spots.

### The page itself (HTML)

**Who**, **What**, **Where**, my name, and the LinkedIn/GitHub URLs are plain HTML inside `<main>`. The terminal's `ls` and `open` read the links straight from the page, so I only have to change a URL in one place.

### `CONFIG` (top of the `<script>`)

| Key | What it drives |
| --- | --- |
| `emailEnc` | My encoded email (see below) |
| `host` | The domain shown in the terminal and boot screen |
| `role`, `region`, `stack`, `focus` | `neofetch` |
| `bio` | `whoami` |
| `tz` | Time zone for the clock and `date` (`America/Chicago`) |

### If I change my email

It's stored as reversed base64. To get the new value, run this in a browser console:

```js
btoa("new@evanbarth.com").split("").reverse().join("")
```

or with Node:

```bash
node -e 'console.log(Buffer.from("new@evanbarth.com").toString("base64").split("").reverse().join(""))'
```

Then paste the output into `CONFIG.emailEnc`.

> This stops bots that just scrape the raw HTML, which is most of them. It won't stop a headless browser or a person, which is why the site uses an alias instead of my real inbox. If it ever gets spammy, I kill the alias and make a new one. If I proxy through Cloudflare, their Email Obfuscation adds another layer.

### If I add a theme

Add it to `THEMES` in the main script **and** to the `T` object in the little `<head>` script. The head one applies the saved theme before the page paints. If I skip it, the page flashes the default before switching.

In `THEMES`: `a` is the accent, `b` is the second color, `grey` is the resting dot color, and `bg` is the browser bar color (all colors are RGB triplets except `bg`). A simple accent theme only needs those. A full look like Sky also gets its own `html[data-theme="name"]` block in the CSS for the background, glows and anything else that changes.

### If I want a different default

Change the `"sky"` fallback in the `<head>` script and in `themeName` in the main script.

### If the boot screen gets old

In the `<head>` script, delete the line that starts with `if (!sessionStorage.getItem("booted")`.

### If I add a terminal command

Add an entry to `COMMANDS`: `name: ["description", (args) => { ... }]`. Put `true` as the third item to keep it secret: it still works, but it stays out of `help` and tab completion.

## Deploying

It's a static file, so anything works. `index.html` just goes at the web root.

- **Cloudflare Pages:** connect the repo, leave the build command empty, set the output directory to `/`, then add the domain in the project settings.
- **GitHub Pages:** Settings → Pages → deploy from `main`, folder `/`. Add a `CNAME` file with `evanbarth.com` and point DNS at GitHub.
- **Anywhere else:** upload `index.html` as-is.

## Privacy

No analytics, no cookies, no third-party scripts. The only outside request is Google Fonts (Orbitron for my name, Space Grotesk for headings, JetBrains Mono for everything else). The site keeps two small things in the visitor's own browser:

- `localStorage.theme`: the color they picked
- `sessionStorage.booted`: so the boot screen only shows once per session

If I ever want to drop Google Fonts, I can self-host the three fonts and swap the `<link>` tags for `@font-face` rules. If the fonts don't load, it falls back to system fonts without breaking anything.

## Browser support

Current Chrome, Edge, Firefox and Safari, on desktop and mobile.

## License

© 2026 Evan Barth. All rights reserved.
