# arsenal

A fast, offline offensive-security command reference. Type a few characters, get the command, copy it, move on. Built as a single static page with instant client-side search — no build step, no backend, no dependencies.

**Everything is lab-only.** No client data, real engagements, internal hostnames, IPs, or credentials belong in this repo. See the OPSEC note below.

## Why this exists

Mid-engagement you don't want to scroll a 4,000-line cheatsheet. You want to type `kerberoast` and have the exact command on your clipboard in two seconds. That's the whole design goal: **retrieval speed**.

- Instant fuzzy filtering across command, tool, task, and tags
- One-key copy (matched search terms highlighted as you type)
- Full keyboard navigation — never touch the mouse
- Category filters for browsing
- Works completely offline once loaded

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `/` | Focus the search box from anywhere |
| `↑` `↓` | Move between results |
| `Enter` | Copy the highlighted command |
| `Space` | Expand details for the highlighted row |
| `Esc` | Clear the search |

Comment lines (starting with `#`) are shown for context but stripped automatically when you copy, so only runnable commands land on your clipboard.

## Running locally

The page fetches `data/commands.json`, so it needs to be served over HTTP — opening `index.html` directly with `file://` will be blocked by the browser. Any static server works:

```bash
python3 -m http.server
# then open http://localhost:8000
```

## Deploying to GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages → Build and deployment**, set **Source** to **GitHub Actions**.
3. Push to `main`. The included workflow (`.github/workflows/deploy.yml`) publishes automatically.

Your site goes live at `https://<username>.github.io/<repo>/`.

## Adding your own commands

You never touch the HTML. Open `data/commands.json` and add an object:

```json
{
  "category": "privesc-linux",
  "tool": "pspy",
  "title": "Watch processes without root",
  "command": "./pspy64 -pf -i 1000",
  "description": "Snoops on cron jobs and short-lived processes as an unprivileged user.",
  "tags": ["privilege escalation", "linux", "cron", "monitoring"],
  "output": "Live process and filesystem events — catch root cron jobs firing."
}
```

### Fields

| Field | Purpose |
|-------|---------|
| `category` | Which section it lives under (see list below) |
| `tool` | The binary or technique name, shown in amber |
| `title` | Short description of what this specific invocation does |
| `command` | The command itself. Use `\n` for multi-line; lines starting with `#` render as dim comments and are stripped on copy |
| `description` | One or two sentences of context |
| `tags` | Searchable keywords — add the words *you* would actually type to find it |
| `output` | What to expect back, so you know if it worked |

### Categories

`recon` · `enumeration` · `exploitation` · `privesc-linux` · `privesc-windows` · `active-directory` · `cloud-azure` · `pivoting` · `reporting`

To add a new category, add one line to the `CATEGORY_LABELS` object near the top of the `<script>` in `index.html`. Everything else — the filter chip, the section header, the ordering — is generated automatically.

## OPSEC

This is a public reference built from public techniques and lab practice. Keep it that way:

- No client names, internal hostnames, IP ranges, or credentials — not even anonymized. Internal identifiers are more re-identifiable than they look.
- Every command here targets `<target>` placeholders and lab environments (HTB, THM, PG, your own VMs).
- If you fork this for engagement notes, make that fork **private** and keep it separate.

## License

MIT — use it, fork it, make it yours.
