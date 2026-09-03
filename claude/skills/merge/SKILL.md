---
name: merge
description: Merge separately edited versions of one document into a single Word draft whose tracked changes are labeled by the revision they came from, with Version Story. Use when the user wants to merge, consolidate, or reconcile edits from several reviewers or parties into one draft, from files on the user's computer or attached to the conversation.
---

# Merge document versions with Version Story

Use the Version Story merge tools when the user has two or more separately edited copies of
one document and wants them combined into a single draft. The result is one Word document
carrying each revision's edits as tracked changes labeled by the version they came from,
ready to accept or reject in Word.

Merging needs one base document — the draft every revision started from — plus at least two
revisions, each edited from that same base. The base is the document every revision is
diffed against, so it must be right. When the user's words or the file names make the base
clear (a "v1" among "v2" revisions, an "original" or "template"), proceed with it and say
which document you used as the base; ask only when it is genuinely unclear, in the user's
terms: "Which draft did everyone start from?" — never in terms of tools or parameters. If
the versions were edited in sequence rather than each from a common original, that history
calls for a version history document, not a merge — confirm which situation the user has
when it is unclear. The order of the revisions does not affect the result — where revisions
conflict, every edit appears as a tracked change attributed to its revision, for the user to
resolve in Word — so never ask the user about merge order.

The merged document labels each revision's tracked changes with its author. Before merging,
propose who authored each revision and confirm it with the user: `read_document_authors`
reads each document's stored author properties (creator, last editor), and file names or
the documents' own text often name the party. When nothing gives a basis for a proposal,
ask the user who authored each revision — never guess silently.

When the merged document is ready, put it in front of the user: pass the path in
saved_files to this environment's file-presentation tool (present_files, or SendUserFile in
Cowork remote) so the document appears in the conversation. Writing a path into a sentence is not delivery.
Then stop: do not summarize, describe, or list what changed. If the user asks you to analyze, summarize, or explain it, call
read_text with the version_story_id and merge_version_id from the tool result —
it serves the merged document's tracked changes as plain text — rather than reading the
docx; the server's instructions cover its format and parameters. The content is not a user
deliverable, so never save or present it as a file.

The tools carry their own current workflow: follow the connected server's instructions,
each tool's description, and the guidance in every tool result. To the user, Version Story
is simply combining their drafts and producing their merged document — describe the work in
those outcome terms, never in terms of servers, transfers, or tooling internals.

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
