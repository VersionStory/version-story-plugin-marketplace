---
name: combine
description: Combine two or more independent documents that share no common original into a single Word draft whose tracked changes are attributed to the document they came from, with Version Story. Use when the user wants separately written drafts folded into one document.
---

# Combine independent documents with Version Story

Use the Version Story combine tools when the user has two or more documents that share no
common original, such as drafts each written from scratch, and wants them folded into a single
draft. The result is one Word document carrying each source document's content as tracked
changes labeled by the document and its author.

Call `get_instructions` once per session before the first combine and follow its workflow.

Use the merge tools instead whenever the documents are edits of one original. Merge diffs each
revision against that shared base and reads more cleanly. Combine is for the case with no
shared base: it constructs one from the documents, then folds them together. When it is unclear
whether the documents share an original, ask in the user's own terms ("Did these all start from
one document, or were they written separately?") and choose merge or combine from the answer.
Document order does not affect the result, so never ask about it.

Propose who authored each document from file names, document properties, or the documents'
own text, confirm it with the user, and pass the names as `document_authors`. Ask when nothing
gives a basis for a proposal rather than guessing.

The shape of the work: `create_combine`, transfer the uploads from the manifest, then
`get_combined_document` with the `version_story_id` and `combine_version_id`, repeating on
`processing`. Building the shared base takes time, so several minutes is normal. When ready,
fetch the download manifest, save the Word document, and give the user the file and the
`combined_document_url` link.

When the combined document is delivered, stop. Do not summarize what changed. If the user asks
for analysis, call `read_text` with the `version_story_id` and `combine_version_id`.

If a manifest fetch, upload, or download fails, call `report_issue` with the verbatim error,
then tell the user plainly what failed.

If a tool reports that the account's monthly upload limit has been reached, tell the user, give
them the upgrade link from the message, and stop.
