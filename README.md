# agentBrain addon registry

The official addon "app store" for [agentBrain](https://github.com/frontmatters/agentbrain-registry).
A registry is a static `index.json` plus addon zips published as release assets.

## Using this registry

It is the built-in default — `addons.sh` already knows it. To list and install:

```bash
bash scripts/addons.sh search           # browse bundled + local + registries
bash scripts/addons.sh install <id>     # download, verify sha256, install
```

Add your own registry alongside it:

```bash
bash scripts/addons.sh registry add <name> <url-to-index.json>
```

## index.json contract

```json
{
  "registry": "agentbrain-official",
  "updated": "<ISO-8601>",
  "addons": [
    {
      "id": "voice",
      "name": "Voice (STT + TTS)",
      "version": "1.3.0",
      "url": "https://github.com/frontmatters/agentbrain-registry/releases/download/addon-voice-v1.3.0/addon-voice-v1.3.0.zip",
      "sha256": "…",
      "privacy": "local"
    }
  ]
}
```

Every install verifies the downloaded zip's `sha256`; a mismatch hard-fails.

## Publishing an addon

From an agentBrain checkout:

```bash
GITEA_URL=http://mikeminim4.local:3000 bash scripts/publish-addon.sh <id>
```

This packages the addon (privacy-scanned zip + sha256), creates a Gitea release
tagged `addon-<id>-v<version>` with the zip as an asset, regenerates `index.json`
here, and commits/pushes.

### GitHub mirror (manual)

The default `url` template in `index.json` points at the public GitHub mirror.
After reviewing on Gitea, mirror each release to GitHub:

1. Create/maintain the public `frontmatters/agentbrain-registry` repo on GitHub.
2. For each tag, `gh release create <tag> <zip>` (same asset name).
3. Push `index.json` to the GitHub `main`.

Until a release is mirrored to GitHub, its `url` resolves only after the mirror
step — identical to the Gitea-first review flow used by the framework itself.
