# Version Story Plugin Marketplace

Version Story is a deterministic document comparison engine for lawyers. It turns two versions
of a Word or PDF document into a true redline, merges parallel edits into one tracked-changes
draft, and builds version histories that attribute every change. Agents reach it through one
hosted MCP server:

```text
https://mcp-compare.versionstory.com/mcp
```

This repository holds the plugin packages that let agent hosts install that server. Each host
gets its own directory. The files here are generated from the Version Story backend by its
plugin build and are not edited by hand.

## Channels

| Host | What it is | Where it is listed | Files here |
| --- | --- | --- | --- |
| Claude (desktop, Cowork, Code) | Local plugin: a bundled MCP server that uploads and downloads documents itself, plus skills | This repository, added as a Claude plugin marketplace | [`claude/`](claude/) |
| Claude (any surface) | Hosted connector | Anthropic connector directory, and Anthropic's [claude-for-legal](https://github.com/anthropics/claude-for-legal) reference plugins | None; the listing points at the server URL |
| Grok Build | Hosted connector plus skills | [xAI plugin marketplace](https://github.com/xai-org/plugin-marketplace) | [`grok/`](grok/) |
| ChatGPT Work | Hosted connector | OpenAI app submission | None; the listing points at the server URL |

## Install in Claude

1. Open **Customize → Plugins**.
2. Under **Personal plugins**, select **+ → Add marketplace**.
3. Choose **Add from a repository** and enter `VersionStory/version-story-plugin-marketplace`.
4. Install **Version Story** from the Version Story marketplace.
5. Start a new conversation and ask Claude to compare or redline documents.

In Claude Code:

```text
/plugin marketplace add VersionStory/version-story-plugin-marketplace
/plugin install version-story@version-story
```

Organization owners can instead download the
[latest production plugin](https://mcp-compare.versionstory.com/plugin-download) and upload it
under **Organization settings → Plugins → Add plugins → Upload a file**. The plugin checks for
a newer release when it starts.

Full setup, including the network allowlist for Claude Chat sessions, is at
https://www.versionstory.com/developers/plugin/claude-installation-guide.

## Install in Grok Build

Run `/plugins` inside Grok Build and install **version-story**. See [`grok/README.md`](grok/README.md).

## Releases

Every directory is published together from one backend release and carries the same version.
Tags mark each publish (`v0.28.0`). The xAI catalog pins the tagged commit and re-pins when the
version changes.

## Support

https://versionstory.com, or kevin@versionstory.com.
