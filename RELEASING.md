# Releasing

Plugins in this marketplace are git **submodules** pinned to a specific commit.
Pushing a plugin's own repo is **not enough** — the marketplace keeps serving the
old pinned commit until you bump the pointer here too.

## To ship a plugin update

After pushing changes (and bumping `version` in the plugin's `plugin.json`):

```bash
# 1. Advance the submodule to the new commit
cd <plugin-name>
git fetch origin && git checkout master && git pull
cd ..

# 2. Record the new pointer in the marketplace and push
git add <plugin-name>
git commit -m "Bump <plugin-name> submodule to <short-sha>"
git push
```

## Then in Claude Code

Refresh the marketplace so it re-reads the new pin:

```
/plugin marketplace update personal-marketplace
```

If an install previously failed, also clear stale cache:
- `~/.claude/plugins/cache/temp_local_*`
- `~/.claude/plugins/marketplaces/temp_*`

## Gotchas

- **Bump the plugin's `version`** each release, or the installer may reuse the cached manifest.
- A failed install still writes a cache entry — clear it before retrying.
