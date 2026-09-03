# Version Story for Grok Build

Git for lawyers: diff, merge, and blame for Word and PDF documents, exposed as MCP tools.

Lawyers do not use git. They use redlines, and they get them from desktop comparison tools
that have no API. Version Story is that comparison engine as a service. `compare` is diff: two
versions of a document in, a true redline out as Word tracked changes, a PDF, or a text
rendering the agent can read. `merge` is a three-way merge against the shared base, with each
party's edits attributed. `version-history` is blame across a chain of drafts. The engine is
deterministic. It produces the exact set of changes between two versions and preserves
footnotes, tables, numbering, and formatting, which is what makes the output something a law
firm will sign off on.

## What the plugin is

A pointer to Version Story's hosted MCP server plus four skills. There is no local process and
no code to run. Grok Build connects over streamable HTTP and completes an OAuth sign-in in the
browser on first use.

| | |
| --- | --- |
| Endpoint | `https://mcp-compare.versionstory.com/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 with PKCE and dynamic client registration |
| Account | Version Story account required (free tier available at https://app.versionstory.com/register) |
| Inputs | `.docx`, `.doc`, `.pdf` (native and scanned), `.xlsx` for comparisons |
| Outputs | PDF redline, Word tracked-changes redline, changed-pages-only PDF, text rendering with change markup |

## Tools

| Tool | Purpose |
| --- | --- |
| `get_instructions` | Full workflow guide for the agent. Call first. |
| `create_comparison` | Stage one or more base-versus-revision comparisons from local files. Returns an upload manifest. |
| `create_comparison_from_links` | Compare documents already reachable at HTTPS URLs. No upload step. |
| `get_redlines` | Wait for generation and return a download manifest of redline files. |
| `read_text` | Serve a finished redline, merge, combine, or version history as markdown with `<ins>`, `<del>`, and `<moved>` markup for analysis. |
| `create_merge` / `get_merged_document` | Merge two or more revisions of one base document into a single Word draft, each revision's edits as tracked changes attributed to its author. |
| `create_combine` / `get_combined_document` | Fold independent drafts with no shared base into one Word draft with attributed tracked changes. |
| `create_version_history` / `get_version_history_document` | Across an ordered chain of versions, produce one Word document where every surviving change is attributed to the version that introduced it. |
| `list_projects` / `list_project_comparisons` | Locate existing projects and redlines in the user's account. |
| `check_connection` | Report the authorization state and expiry. |
| `report_issue` | Send a client-side transfer failure to Version Story engineering. |

Tool input schemas come from `tools/list` and are always current with the deployed server.
Response shapes are documented at https://www.versionstory.com/developers/mcp/overview.

## How a comparison flows

1. The agent calls `create_comparison` with the local paths and display names of the base and
   revised documents. The response is `awaiting_uploads` with a signed upload manifest.
2. The agent fetches the manifest and performs one HTTP `PUT` per source file with any HTTP
   client (curl works), sending the manifest entry's `authorization` and `content_type`.
3. The agent calls `get_redlines`. It blocks server-side for up to about a minute and returns
   `ready` or `processing`; the agent repeats on `processing`.
4. The ready response carries a signed download manifest. The agent fetches the redline files
   and hands them to the user, along with the interactive redline link.

Merge, combine, and version history follow the same four steps with their own create and wait
tools. Manifest URLs and upload authorizations expire after one hour.

## Skills

| Skill | When it applies |
| --- | --- |
| `compare` | Any request to compare, diff, or redline documents. |
| `merge` | Two or more separately edited copies of one document need to become one draft. |
| `combine` | Independent drafts with no shared original need to become one draft. |
| `version-history` | The user wants to know which draft introduced a change, or who changed what across versions. |

The skills stay short on purpose. The server's own instructions carry the current workflow and
are fetched at every session, so wording fixes ship without a plugin update.

## Network endpoints and data handling

- `https://mcp-compare.versionstory.com/mcp` for tool calls.
- `https://mcp-compare.versionstory.com/transfer/...` for the agent's source-document uploads.
- `https://documents.versionstory.com/...` for signed redline downloads.
- `https://app.versionstory.com` for the browser sign-in during OAuth.

Documents the agent uploads land in the user's own Version Story account and are stored,
processed, and retained under the same controls as documents uploaded through the Version
Story web application. Customer documents are never used to train models. Document content
does not pass through the language model as part of producing a redline; the agent sees file
paths, tool results, and status messages, and reads document text only when the user asks for
the changes to be explained and it calls `read_text`. Version Story is SOC 2 Type II certified;
the trust center is at https://app.versionstory.com/trust.

## Limits

Free accounts have a monthly document upload limit. When it is reached, tool results say so and
include an upgrade link. The limit resets at the start of the next month.

Excel files are supported for comparison only, not for merge, combine, or version history.

## Install

1. Start Grok Build and open the plugin browser:

   ```text
   /plugins
   ```

2. Find **version-story** and install it.
3. Ask Grok to compare two documents. The first tool call opens a browser window for the
   Version Story sign-in.

## Example prompts

```text
Redline v2 of the NDA against v1.
```

```text
Compare these two PDFs and give me the Word tracked-changes version.
```

```text
Merge the counterparty's and our finance team's edits to the SPA into one draft. Both started from the version I sent Monday.
```

```text
Across drafts 1 through 4 of the license agreement, which draft introduced the exclusivity clause?
```

## Support

kevin@versionstory.com, or https://versionstory.com.
