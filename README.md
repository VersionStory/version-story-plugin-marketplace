# Version Story: document comparison for Claude, ChatGPT, and Grok

Version Story is a deterministic document comparison engine for lawyers. Give it two versions
of a Word or PDF document and it returns a true redline: every insertion, deletion, and move,
as a Word file with tracked changes or a PDF. It also merges parallel edits into one
tracked-changes draft and builds version histories that attribute every change.

This repository holds the plugins that put that engine inside an AI assistant. With one
installed, asking the assistant to compare two documents returns the redline itself, produced
by Version Story, not the assistant's own summary of what it thinks changed.

```text
Compare these two versions and give me a Word file with tracked changes.
```

## What an assistant can do with it

| Task | What you get back |
| --- | --- |
| Compare two versions of a document | A redline as a PDF, a Word file with tracked changes, or a PDF of only the changed pages |
| Merge edits several people made to their own copies | One Word document with each person's changes tracked under their name |
| Combine drafts written independently | One Word draft with each author's contribution marked |
| Trace a series of drafts | One document showing which version introduced each change |

Supported inputs are Word (`.docx`, `.doc`) and PDF, including scanned PDFs. Footnotes, tables,
numbering, and formatting are preserved. The comparison is produced by a rules-based engine, so
the same two documents always give the same redline.

## Install

| Assistant | How | Guide |
| --- | --- | --- |
| Claude desktop, Cowork, Claude Code | Plugin from this repository (below) | [Plugin installation guide](https://www.versionstory.com/developers/plugin/claude-installation-guide) |
| Claude in a browser | Hosted connector | [Connector installation guide](https://www.versionstory.com/developers/mcp/claude-installation-guide) |
| ChatGPT | Plugin from ChatGPT's plugin directory | [Version Story in the plugin directory](https://chatgpt.com/plugins/plugin_asdk_app_6ab400006af081919da14c4f6b0048c0), [ChatGPT installation guide](https://www.versionstory.com/developers/mcp/chatgpt-installation-guide) |
| Grok Build | Plugin in [`grok/`](grok/) | [`grok/README.md`](grok/README.md) |
| Any other MCP client | The hosted MCP server | [MCP server overview](https://www.versionstory.com/developers/mcp/overview) |

A Version Story account is required, and is free to start at
[app.versionstory.com](https://app.versionstory.com/register?trial).

### Claude

1. Open **Customize → Plugins**.
2. Under **Personal plugins**, select **+ → Add marketplace**.
3. Choose **Add from a repository** and enter `VersionStory/version-story-plugin-marketplace`.
4. Install **Version Story** from the Version Story marketplace.
5. Start a new conversation and ask Claude to compare two documents.

In Claude Code:

```text
/plugin marketplace add VersionStory/version-story-plugin-marketplace
/plugin install version-story@version-story
```

Organization owners can instead download the
[latest production plugin](https://assets.versionstory.com/plugins/version-story-mcp-plugin.plugin)
and upload it under **Organization settings → Plugins → Add plugins → Upload a file**. The
plugin checks for a newer release when it starts.

### ChatGPT

Open **Plugins** in the ChatGPT sidebar, search for **Version Story**, click **Install
plugin**, and authorize your Version Story account. There is nothing else to configure.

### Grok Build

Run `/plugins` inside Grok Build and install **version-story**. See [`grok/README.md`](grok/README.md).

### Any MCP client

Every plugin here connects to one hosted MCP server:

```text
https://mcp-compare.versionstory.com/mcp
```

It uses streamable HTTP and OAuth 2.1 with dynamic client registration, so a compliant client
needs only the URL.

## How to use it

- [Compare documents in Claude](https://www.versionstory.com/help/compare-documents-in-claude)
- [Compare documents in ChatGPT](https://www.versionstory.com/help/compare-documents-in-chatgpt)
- [Get a Word file with tracked changes](https://www.versionstory.com/help/get-tracked-changes-word-file)
- [Compare a Word document to a PDF](https://www.versionstory.com/help/compare-word-to-pdf)

Building your own agent or application? Use the
[REST API](https://www.versionstory.com/product/api) or the
[MCP tools](https://www.versionstory.com/developers/mcp/compare) directly.

## Why give an assistant a comparison tool

Without one, an assistant asked to compare two documents writes its own comparison program on
the spot. We measured that on 25 real contract revisions:

- Claude with no comparison tool averaged 5.7 minutes and 858,000 tokens per comparison. The
  Version Story API took a median of 2.8 seconds and no model tokens.
  [Benchmark](https://www.versionstory.com/resources/benchmarks/ai-agent-document-comparison-cost)
- An agent reads a redline delivered as Markdown with 44% fewer tokens than a Word
  tracked-changes file and 69% fewer than a PDF.
  [Benchmark](https://www.versionstory.com/resources/benchmarks/markdown-vs-word-vs-pdf-redlines)

## What is in this repository

| Directory | Contents |
| --- | --- |
| [`claude/`](claude/) | The Claude plugin: a bundled local MCP server that uploads and downloads documents itself, plus the `compare`, `merge`, `combine`, and `version-history` skills |
| [`grok/`](grok/) | The Grok Build plugin: a pointer to the hosted MCP server plus the same four skills |
| [`.claude-plugin/`](.claude-plugin/) | The marketplace manifest Claude reads when this repository is added as a marketplace |

`claude/` and `grok/` are generated from the Version Story backend by its plugin build and are
not edited by hand. The ChatGPT plugin and the Claude connector have no files here; their
listings point at the hosted server.

## Releases

Every directory is published together from one backend release and carries the same version.
Tags mark each publish (`v0.28.0`).

## Support

https://www.versionstory.com, or kevin@versionstory.com.
