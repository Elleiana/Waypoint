# Waypoint // site migration toolkit

A self-contained tool for planning, mapping, and validating URL redirects during a site migration.
No installs, no logins, no spreadsheets.
Open it, do the work, export, done.

---

## The idea
Site migrations have a habit of quietly destroying organic traffic. Old URLs disappear, links break, search engines get confused, and nobody notices until rankings have already tanked. Waypoint exists to prevent that - it takes your old URLs and your new URLs, figures out where everything should go, and gives you a clean redirect map you can hand to a developer.
It runs entirely in your browser. Nothing is sent anywhere (with one exception covered below). You can use it offline, share sessions with colleagues, and export in whatever format the developer needs.

---

## The four tabs
### 🗺️ Mapper

This is where the work happens. Paste in your old site's URLs on the left, your new site's URLs on the right, hit the arrow, and Waypoint will cross-reference the two lists and suggest the best match for every old URL that doesn't have an exact equivalent on the new site.

From there you go through the results and confirm, adjust, or manually set each redirect. Unchanged URLs (ones that exist identically on both sites) are grouped separately so you're only dealing with what actually needs attention.

**A few things worth knowing:**

- You can paste raw URLs, drop in a sitemap XML, or upload a CSV. Waypoint will figure out what it's looking at.
- The **Workbench** (collapsible, below the URL inputs) has pattern rules for bulk-confirming redirects that follow a consistent structure, and a bulk import tool if you've already got redirects defined somewhere.
- The **confidence threshold** in the Workbench controls how certain Waypoint needs to be before it shows a suggestion. At 50% (the default), anything with a weaker match just shows the manual input option instead. Slide it down if you want to see lower-confidence suggestions; up if you only want the solid ones.
- The filter pills at the top of the results let you focus on just the unconfirmed ones so you can work through them without scrolling past everything you've already dealt with.
- **↑ prev / ↓ next unconfirmed** jumps between rows that still need attention.
- Keyboard shortcuts: `Y` confirms the suggestion on a focused row, `E` opens the edit input, `Backspace` clears a confirmed destination, arrow keys move between rows, `X` toggles row selection for bulk actions.
- Sessions are named - give yours a project name and it'll show up clearly in history and in exported filenames.

---

### 🧪 Analysis

Four tools that inspect the logic of your redirect map without touching the live web. Run them at any point - they work purely on what's confirmed so far.

**Tracker** - breaks down your redirect coverage by directory (`/blog/`, `/services/`, etc.) with a progress bar per section. A quick sanity check for "how much of this have I actually done?"

**Looper** - finds redirect loops. If `/a` redirects to `/b` and `/b` redirects back to `/a`, that's a loop, and it'll be caught here as well as flagged inline on the Mapper.

**Hopper** - finds redirect chains. If `/a` → `/b` → `/c`, a user hitting `/a` takes two hops to get where they're going. Hopper flags these and suggests flattened versions you can apply with one click.

**Merger** - finds destination URLs that multiple old URLs are pointing to. This might be intentional (you're consolidating pages), but it's worth being aware of. Merger surfaces them so you can confirm it's deliberate rather than a mistake.

**Run all checks** (top right of the page) runs all four at once.

---

### 🕷️ Spider

Point it at a website and it'll crawl all the internal URLs it can find - useful for pulling a URL list from a live site when you don't have a sitemap handy.

**Proxy** - browsers have security restrictions that prevent fetching pages from other sites directly. Waypoint uses a proxy to get around this. The **automatic** option uses a built-in proxy that works immediately with no setup. If you'd rather use your own, switch to **manual** and point it at your own Cloudflare Worker.

The **Workbench** (collapsible) has options for what to exclude before crawling - pagination, static assets, dev URLs, query strings, and custom patterns. Worth checking before a crawl on a large site so you're not wading through thousands of `.css` and `.png` references.

Once a crawl finishes, the results show discovered / ok / errors / blocked. If anything was excluded by the Workbench filters, a summary accordion appears showing the breakdown - handy for understanding what's on the site even if you didn't crawl it.

Hit **→ use as existing site URLs** or **→ use as new site URLs** to send the results straight to the Mapper.

---

### 📊 Validator

The final check before you export. Runs through your confirmed redirects and flags anything that could cause problems: loops, chains, mergers, missing destinations.

**Live Check** (optional) - if you provide base URLs for the old and new sites, Waypoint will actually fetch each URL and check the HTTP status. This tells you whether old URLs are still resolving correctly and whether your redirect destinations actually exist on the new site. Leave the base URLs blank to skip the live checks and just run the logic checks.

**Report** - generates a standalone HTML summary of your redirect map, useful for sending to a client or keeping as a record. The PDF button opens the same report in a new tab where you can print or save it yourself.

**Export** - downloads your confirmed redirects in your format of choice: CSV, Apache `.htaccess`, or Nginx config. The filename includes the project name and today's date.

---

## Sessions

Waypoint automatically saves your recent sessions (up to 5) in your browser. The **home screen** (click the Waypoint logo) shows them as cards with key stats - total URLs, how many are confirmed, overall completion percentage. Click any card to pick up where you left off.

Sessions are saved whenever you run an analysis. You can also manually save a session as a JSON file (↓ Save session in the Mapper) and load it back later or on a different device.

**Share links** - the **⇪ Share** button generates a short link to your current session. Anyone who opens it gets the full session loaded into their own Waypoint, ready to view and continue. Shared sessions are stored and don't expire.

---

## A note on data

Everything in Waypoint runs in your browser. Your URLs, redirect mappings, and session data never leave your device - with two exceptions:

- The **Spider's proxy** routes page fetches through a Cloudflare Worker so the browser can reach external sites. Only the URLs being fetched are passed through.
- **Share links** store session data in a Cloudflare Worker so they can be retrieved via a short link. If you share a session, that data is stored there.

If you're not using the Spider or the share feature, Waypoint is completely local.

---

## Keyboard shortcuts (Mapper)

| Key | Action |
|-----|--------|
| `↑` / `↓` or `k` / `j` | Move between rows |
| `Y` | Confirm the suggestion on the focused row |
| `E` | Open the edit input |
| `Backspace` / `Delete` | Clear the confirmed destination |
| `X` | Toggle row selection (for bulk actions) |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Escape` | Close any open modal |

---

*🐯 // boring stuff* - [waypoint legal & privacy](#)
