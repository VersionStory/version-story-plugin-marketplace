---
name: version-history
description: Build a version history document across a document's versions with Version Story, one Word document in which every surviving change is a tracked change attributed to the version that introduced it. Use when the user asks who changed what, which draft introduced a clause, or how a document evolved across versions.
---

# Trace who changed what with Version Story

Use the Version Story version history tools when the user wants change attribution across a
document's history — which draft introduced a clause, whose edit survived, who changed what.
The result is one Word document, based on the newest version, in which every surviving
change is a tracked change labeled with the version that introduced it.

The document is built across an ordered chain of versions, oldest first, and the order is
the result: each version is compared to the one before it, so a wrong order misattributes
changes. If the user has not stated the chronological order, propose the order you infer
from names, version numbers, or dates, show it as a simple oldest-to-newest list, and get
their confirmation before running.

The document labels each version's changes with its author. Before running, propose who
authored each version and confirm it with the user: `read_document_authors` reads each
document's stored author properties (creator, last editor) — its modified timestamps
support the order proposal too — and file names or the documents' own text often name the
party. If the user names who wrote each version, pass those names. When nothing gives a
basis for a proposal, ask the user who authored each version — never guess silently or let
the labels fall back to file names unconfirmed.

When the version history document is ready, put it in front of the user: pass the path in
saved_files to this environment's file-presentation tool (present_files, or SendUserFile in
Cowork remote) so the document appears in the conversation. Writing a path into a sentence is not delivery.
Then stop: do not summarize, describe, or list what changed. If the user asks you to analyze, summarize, or explain it, call
read_text with the version_story_id and version_history_version_id from the tool
result — it serves the document's attributed changes as plain text — rather than reading
the docx; the server's instructions cover its format and parameters. The content is not a
user deliverable, so never save or present it as a file.

The tools carry their own current workflow: follow the connected server's instructions,
each tool's description, and the guidance in every tool result. To the user, Version Story
is simply tracing who changed what and producing their version history document — describe
the work in those outcome terms, never in terms of servers, transfers, or tooling
internals.

If a tool reports that sign-in is required, call `sign_in`, and follow its result: on this
computer the browser opens by itself; in a cloud session, give the user the sign-in link,
and when they paste back the address they land on afterwards, pass it to
`complete_sign_in`.

If a tool reports that a monthly upload limit has been reached, that is the state of the user's
account, not a problem to solve. Tell them they have hit the limit, give them the upgrade link
from the message as a markdown hyperlink, and stop — the limit resets at the start of next month
and nothing else clears it. Do not retry, do not try fewer documents, and do not go looking
through their existing projects for an earlier comparison to hand over instead. Substituting
something else for what they asked for reads as the request quietly failing; being told they are
out of uploads and where to upgrade is the useful answer.
