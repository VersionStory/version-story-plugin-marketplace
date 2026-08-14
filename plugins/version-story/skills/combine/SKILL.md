---
name: combine
description: Combine two or more independent documents that share no common original into a single Word draft whose tracked changes are labeled by the document they came from, with Version Story — working from files on the user's computer or attached to the conversation.
---

# Combine independent documents with Version Story

Use the Version Story combine tools when the user has two or more documents that share no
common original — independent drafts each written from scratch, or otherwise unrelated
versions — and wants them combined into a single draft. The result is one Word document
carrying each document's content as tracked changes labeled by the document it came from,
ready to accept or reject in Word.

Reach for the merge tools instead whenever the documents are edits of one original: merge
diffs each revision against that shared base, which reads more cleanly. Combine is for the
case where there is no shared base to diff against — so it builds one, constructing a shared
base from the documents before folding them together. When it is unclear whether the
documents share an original, ask the user in their own terms ("Did these all start from one
document, or were they written separately?") and choose merge or combine from the answer. The
order of the documents does not affect the result, so never ask the user about it.

The combined document labels each document's tracked changes with its author. Before
combining, propose who authored each document and confirm it with the user:
`read_document_authors` reads each document's stored author properties (creator, last editor),
and file names or the documents' own text often name the party. When nothing gives a basis for
a proposal, ask the user who authored each document — never guess silently.

When the combined document is ready, put it in front of the user: pass the path in
saved_files to this environment's file-presentation tool (present_files, or SendUserFile in
Cowork remote) so the document appears in the conversation. Writing a path into a sentence is not delivery.
Then stop: do not summarize, describe, or list what changed. If the user asks you to analyze, summarize, or explain it, call
read_text with the version_story_id and combine_version_id from the tool result —
it serves the combined document's tracked changes as plain text — rather than reading the
docx; the server's instructions cover its format and parameters. The content is not a user
deliverable, so never save or present it as a file.

The tools carry their own current workflow: follow the connected server's instructions,
each tool's description, and the guidance in every tool result. To the user, Version Story
is simply combining their documents and producing their combined document — describe the work
in those outcome terms, never in terms of servers, transfers, or tooling internals.

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
