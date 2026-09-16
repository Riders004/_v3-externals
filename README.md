# _v3-externals

External plugins and command packages for the Anya v3 Discord bot framework. This repository serves as a centralized plugin store and distribution system for gameplay commands, interactive features, and utility modules.

### Stack
- **Language(s):** JavaScript (primary), Python (helper scripts)
- **Framework / runtime:** Node.js (Anya v3 bot framework)
- **Notable libraries:** cheerio (HTML parsing for scrapers), custom plugin loader

## How it's organized

```
_v3-externals/
  arcade/           Gamified commands with points & multipliers (tower climbing game)
  fun/              Social check commands (personality & character traits generators)
  downloader/       Content fetching modules (TikTok scraper)
  _Helper/          Shared utility modules & pre-installed dependencies
    downloader/     Helper scripts for downloadable content (Python/JS)
  _test/            Test modules demonstrating plugin installation & imports
  storage/          Asset files (icons, images, game graphics)
  pluginStore.json  Plugin registry & metadata (categories, descriptions, URLs)
  bannedUsers.json  User blocklist for plugin access restrictions
  latest_version.txt Version reference (external drive link)
```

**How it fits together:** 
Plugins are registered in `pluginStore.json` with metadata (category, URL, type). Each category folder contains command definitions that use the Anya v3 framework's `cmd()` helper. Commands can declare external dependencies in an `install` object, which pulls helper files from `_Helper/` and installs them locally. At runtime, the bot fetches raw plugin files from this repo, evaluates them as ES modules, and wires them into the command handler. Test plugins (`_test/`) demonstrate the dependency injection pattern. The `storage/` folder holds assets used by commands (game graphics, reaction icons).

## How to run it

This repository is consumed by the Anya v3 bot at runtime, not run standalone:

1. **Add a plugin:** Create a new command file in the appropriate category folder (e.g., `arcade/mygame.js`).
2. **Register it:** Add an entry to `pluginStore.json` with the raw GitHub URL, category, description, and type.
3. **Include dependencies:** If needed, place helper files in `_Helper/` and declare them in the `install` object of your command.
4. **Load in bot:** The bot's plugin loader reads `pluginStore.json` and dynamically imports commands.

Example plugin entry:
```json
{
  "url": "https://raw.githubusercontent.com/PikaBotz/_v3-externals/refs/heads/main/arcade/tower.js",
  "category": "arcade",
  "desc": "Climb the tower! Pick safe doors to multiply your money, but avoid the bombs.",
  "type": "command"
}
```

For local testing:
- Install Node.js
- Review test files in `_test/` to understand the plugin API
- Use `import` statements to load helpers from `_Helper/`

## Try asking

- **How do I add a new fun command to the fun check pack?** Look at `fun/checkgcfun.js` — it maps command metadata to handler logic. Each command shares the same structure: parse mentions/quotes, generate random scores, and reply with formatted results.

- **What does the `install` object do in plugins?** It declares remote dependencies. The `tower.js` and `tikporn.js` files both use it to fetch helper modules from `_Helper/` and save them locally before the command runs, enabling code reuse and decoupling.

- **Can I add custom assets (icons, images) for a new command?** Yes — store them in `storage/` (already contains game tiles, reaction icons, and stamps) and reference them by path in your command code.
