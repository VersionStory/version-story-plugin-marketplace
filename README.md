# Version Story Plugin Marketplace

This repository distributes the official Version Story Compare plugin for Claude.

## Install in Claude

1. Open **Customize → Plugins**.
2. Under **Personal plugins**, select **+ → Add marketplace**.
3. Choose **Add from a repository** and enter `VersionStory/version-story-plugin-marketplace`.
4. Install **Version Story Compare** from the Version Story marketplace.
5. Start a new conversation and ask Claude to compare or redline documents.

## Install in Claude Code

```text
/plugin marketplace add VersionStory/version-story-plugin-marketplace
/plugin install version-story@version-story
```

## Updating

Releases are published by updating the plugin version in
`plugins/version-story/.claude-plugin/plugin.json`. Refresh the marketplace or enable automatic
updates to receive new versions.

## Support

Visit [Version Story](https://versionstory.com/) for product information and support.
