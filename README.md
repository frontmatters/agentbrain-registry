# agentBrain addon registry

The official addon "app store" for agentBrain — a Docker-style registry of
optional, agent-agnostic add-ons. A registry is a static `index.json` plus
add-on zips published as GitHub release assets.

## Using this registry

It is the built-in default — agentBrain's `addons.sh` already knows it.

```bash
bash scripts/addons.sh search            # browse bundled + local + registries
bash scripts/addons.sh install <id>      # download, verify sha256, install
bash scripts/addons.sh status --remote   # see which installed add-ons have updates
bash scripts/addons.sh update <id>       # pull a newer version
```

Add another registry alongside this one:

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
      "id": "git-email-guard",
      "name": "Git Email Guard (pre-commit hook)",
      "version": "0.1.0",
      "url": "https://github.com/frontmatters/agentbrain-registry/releases/download/addon-git-email-guard-v0.1.0/addon-git-email-guard-v0.1.0.zip",
      "sha256": "…",
      "privacy": "local"
    }
  ]
}
```

Every install verifies the downloaded zip's `sha256` against the index; a
mismatch hard-fails and unpacks nothing.

## Contributing an add-on

Open a pull request adding your add-on's release and `index.json` entry, or host
your own registry and point agentBrain at it with `addons.sh registry add`. See
the agentBrain add-ons documentation for the add-on structure contract and the
`package-addon.sh` / `registry-index.sh` tooling that build the zip + index.
