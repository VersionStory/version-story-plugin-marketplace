---
name: merge
description: Merge two or more separately edited revisions of one document into a single Word draft whose tracked changes are attributed to the revision they came from, with Version Story. Use when the user wants to merge, consolidate, or reconcile edits from several reviewers or parties into one draft.
---

# Merge document revisions with Version Story

Use the Version Story merge tools when the user has two or more separately edited copies of
one document and wants them combined into a single draft. The result is one Word document
carrying each revision's edits as tracked changes labeled by the revision and its author,
ready to accept or reject.

Call `get_instructions` once per session before the first merge and follow its workflow.

A merge needs one base document, the draft every revision started from, plus at least two
revisions edited from that base. The base is what every revision is diffed against, so it must
be right. When the user's words or the file names make it clear (a "v1" among "v2" revisions,
an "original" or "template"), proceed and say which document you used as the base. Ask only
when it is genuinely unclear, in the user's own terms: "Which draft did everyone start from?"
If the versions were edited one after another rather than each from a common original, that is
a version history, not a merge.

Revision order does not affect the result. Where revisions conflict, every edit appears as a
tracked change attributed to its revision, wrapped in a conflict marker in the text rendering,
for the user to resolve. Never ask about merge order.

Propose who authored each revision from file names, document properties, or the documents'
own text, confirm it with the user, and pass the names as `next_version_authors`. Ask when
nothing gives a basis for a proposal rather than guessing.

The shape of the work: `create_merge`, transfer the uploads from the manifest, then
`get_merged_document` with the `version_story_id` and `merge_version_id`, repeating on
`processing`. Merging generates the underlying comparisons first, so several minutes is
normal. When ready, fetch the download manifest, save the Word document, and give the user the
file and the `merged_document_url` link.

When the merged document is delivered, stop. Do not summarize what changed. If the user asks
for analysis, call `read_text` with the `version_story_id` and `merge_version_id`.

If a manifest fetch, upload, or download fails, call `report_issue` with the verbatim error,
then tell the user plainly what failed.

If a tool reports that the account's monthly upload limit has been reached, tell the user, give
them the upgrade link from the message, and stop.
