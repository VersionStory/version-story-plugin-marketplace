---
name: version-history
description: Build a version history document across an ordered chain of document versions with Version Story, one Word document in which every surviving change is a tracked change attributed to the version that introduced it. Use when the user asks who changed what, which draft introduced a clause, or how a document evolved across versions.
---

# Trace who changed what with Version Story

Use the Version Story version history tools when the user wants change attribution across a
document's history: which draft introduced a clause, whose edit survived, who changed what. The
result is one Word document, based on the newest version, in which every surviving change is a
tracked change labeled with the version that introduced it and its author.

Call `get_instructions` once per session before the first version history and follow its
workflow.

The document is built across an ordered chain of versions, oldest first, and the order is the
result: each version is compared to the one before it, so a wrong order misattributes changes.
If the user has not stated the chronological order, propose the order you infer from names,
version numbers, or dates, show it as a simple oldest-to-newest list, and get confirmation
before running.

Propose who authored each version from file names, document properties, or the documents' own
text, confirm it with the user, and pass the names as `version_authors`. Left unset, each change
is labeled with the name of the version that introduced it. Ask when nothing gives a basis for
a proposal rather than guessing.

The shape of the work: `create_version_history` with the version paths oldest first, transfer
the uploads from the manifest, then `get_version_history_document` with the `version_story_id`
and `version_history_version_id`, repeating on `processing`. When ready, fetch the download
manifest, save the Word document, and give the user the file and the interactive link.

When the document is delivered, stop. Do not summarize what changed. If the user asks for
analysis, call `read_text` with the `version_story_id` and `version_history_version_id`.

If a manifest fetch, upload, or download fails, call `report_issue` with the verbatim error,
then tell the user plainly what failed.

If a tool reports that the account's monthly upload limit has been reached, tell the user, give
them the upgrade link from the message, and stop.
