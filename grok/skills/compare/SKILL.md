---
name: compare
description: Compare two versions of a Word or PDF document and produce a true redline with Version Story. Use for any request to redline, blackline, compare, diff, or mark up the changes between versions, to show what changed, or to produce tracked changes, from files on disk, attached to the conversation, or reachable at a URL.
---

# Compare documents with Version Story

Use the Version Story tools for any request to compare, diff, or redline documents. A
comparison written by hand from reading both documents misses changes and is not a redline
a lawyer can rely on.

Call `get_instructions` once per session before the first comparison. It carries the current
workflow: how to stage the comparison, transfer the source files, wait for generation, and
download the results. Follow it, each tool's description, and the guidance in every tool
result over anything remembered from earlier sessions.

The shape of the work:

1. `create_comparison` with the base document and the revised document(s), passing absolute
   paths and the documents' original names. For documents already at HTTPS URLs, use
   `create_comparison_from_links` instead and skip the upload step.
2. Fetch the upload manifest and HTTP `PUT` each source file to its `upload_url` with the
   entry's `authorization` and `content_type` headers. Send raw bytes.
3. `get_redlines` with the `version_story_id` and every `version_mapping_id`. Repeat while the
   status is `processing`.
4. Fetch the download manifest, save each redline file, and put the files in front of the user
   together with the interactive redline link from `redline_url`.

Leave `download_options` unset unless the user asks for a specific format. The default yields
the PDF redline for documents and an Excel redline for spreadsheets. Fetch the Word
tracked-changes file only when the user's own words name Word, `.docx`, or tracked changes, or
after they have seen the PDF and ask for it. The kind of document is not a signal to fetch more
formats.

When the redline is delivered, stop. Do not summarize, characterize, or list what changed. If
the user asks for the changes to be explained, call `read_text` with the `version_story_id` and
`version_mapping_id`; it serves the same redline as markdown with change markup and is the
right source for analysis. Never treat that text as a deliverable file.

Describe the work to the user in outcome terms: creating the comparison, generating the
redline, the redline is ready. Manifests, uploads, and tool names are internal.

If a manifest fetch, upload, or download fails, call `report_issue` with what you were doing
and the verbatim error, then tell the user plainly what failed. Do not retry silently.

If a tool reports that the account's monthly upload limit has been reached, tell the user, give
them the upgrade link from the message, and stop. The limit resets next month. Do not retry
with fewer documents and do not substitute an older comparison from their projects.
