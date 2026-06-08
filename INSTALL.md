# Installing Incubae & ClientMate on Claude Code

These are the same steps for both plugins — just swap the name (`incubae` ↔ `clientmate`).
Verified against the Claude Code plugin docs (the `/plugin` marketplace system).

> **Heads-up on command names:** plugin skills are *namespaced* by the plugin name.
> After install your commands look like `/incubae:incposer` and `/clientmate:cmposer`
> — **not** bare `/incposer`. Type `/incubae:` (or `/clientmate:`) and let autocomplete list them.

---

## Step 0 — Prerequisites

1. **Claude Code, up to date.** Check, then update if `/plugin` isn't recognized later:
   ```bash
   claude --version
   npm install -g @anthropic-ai/claude-code@latest    # or: brew upgrade claude-code
   ```
2. **git** installed.
3. **A `STARTUP_PROFILE.md`** in whatever project directory you'll run the commands from.
   Every command reads it from the *current working directory*. Use the template at
   `assets/STARTUP_PROFILE_TEMPLATE.md`. The same profile works for VCupid, Incubae, and ClientMate.

---

## Step 1 — Get the plugins onto disk

**From the zip files:**
```bash
mkdir -p ~/dev
unzip ~/Downloads/incubae-plugin.zip    -d ~/dev/
unzip ~/Downloads/clientmate-plugin.zip -d ~/dev/
# → ~/dev/incubae-plugin  and  ~/dev/clientmate-plugin
```

**Or, once you've pushed them to GitHub:**
```bash
git clone https://github.com/<you>/incubae-plugin.git    ~/dev/incubae-plugin
git clone https://github.com/<you>/clientmate-plugin.git ~/dev/clientmate-plugin
```

---

## Step 2 — Install (pick ONE method)

### Method A — Marketplace flow (recommended, persistent)

Each plugin ships a `.claude-plugin/marketplace.json`, so you can add it as a local
marketplace and install from it. Inside Claude Code:

```
/plugin marketplace add ~/dev/incubae-plugin
/plugin install incubae@incubae

/plugin marketplace add ~/dev/clientmate-plugin
/plugin install clientmate@clientmate

/reload-plugins
```

The part after `@` is the **marketplace** name (here it matches the plugin name).
Choose **User scope** when prompted to make the commands available in every project.

> Pushed to GitHub instead? Use the repo shorthand — more robust for relative paths:
> ```
> /plugin marketplace add <you>/incubae-plugin
> /plugin install incubae@incubae
> ```

### Method B — Quick test, zero config (`--plugin-dir`)

Loads the plugin(s) for one session only — good for trying them before committing:
```bash
claude --plugin-dir ~/dev/incubae-plugin --plugin-dir ~/dev/clientmate-plugin
```

### Method C — Bundled script (what the plugins were built with)

Each plugin includes an idempotent `install.sh` that registers it directly in your
`~/.claude` config:
```bash
cd ~/dev/incubae-plugin    && bash install.sh
cd ~/dev/clientmate-plugin && bash install.sh
```
Then restart Claude Code (or run `/reload-plugins`).

---

## Step 3 — Verify

1. Open Claude Code in a folder that contains `STARTUP_PROFILE.md`.
2. Run `/plugin` → **Installed** tab — both plugins should be listed with no errors.
3. Type `/incubae:` and `/clientmate:` — the commands should autocomplete:
   - Incubae: `incstrat, inclist, incposer, incmatch, incperks, incapply, incprep, incdevil`
   - ClientMate: `cmstrat, cmlist, cmposer, cmmatch, cmvalue, cmreach, cmprep, cmdevil`
4. Smoke test:
   ```
   /incubae:incstrat
   /clientmate:cmstrat
   ```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `/plugin` not recognized | Update Claude Code (Step 0), restart your terminal. |
| Commands don't appear after install | Run `/reload-plugins`, or restart Claude Code. |
| Skills still missing | Clear the cache: `rm -rf ~/.claude/plugins/cache`, restart, reinstall. |
| "plugin not found in marketplace" | `/plugin marketplace update incubae` (or re-add it), then retry install. |
| A command says it can't find your profile | You're not in a directory with `STARTUP_PROFILE.md` — `cd` into your project. |
| Local relative-path issues | Push to GitHub and add via `<you>/incubae-plugin` — git-based marketplaces resolve relative sources most reliably. |

To remove later: `/plugin uninstall incubae@incubae` and `/plugin marketplace remove incubae`.
