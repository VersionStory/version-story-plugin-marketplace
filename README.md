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

Beginning with version 0.14.0, the plugin checks Version Story for a newer release when it starts.
When an update is available, Claude downloads the new `.plugin` file and gives the user the
organization-admin installation steps.

Organization owners can also download the [latest production plugin](https://mcp-compare.versionstory.com/plugin-download),
then open **Organization settings → Plugins → Add plugins → Upload a file**. Uploading a newer
Version Story plugin with the same name replaces the installed version.

## Support

Visit [Version Story](https://versionstory.com/) for product information and support.
