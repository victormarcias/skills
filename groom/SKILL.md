---
name: groom
description: Walk the user through the open questions and pending decisions one at a time, each as a multiple-choice question with options, backed by a table or chart when that makes the choice clearer. Use when the user runs /groom, usually after a report that left several questions open.
---

# Groom

1. **Collect the backlog:** the open questions, skipped items and pending
   decisions from the conversation so far, or only the ones the user named
   in the request. If there are none, say so and stop.
2. **One at a time,** in the order they came up. For each item, ask with
   `AskUserQuestion`, one question per call:
   - The question stands on its own: it names the repo, file or item and
     says in one line what was found, since the dialog may hide the chat.
   - 2–4 options, each a concrete outcome with its trade-off in the
     description. The recommended one goes first, marked `(Recommended)`.
   - Use `preview` when the options are concrete content to compare side by
     side (the resulting text, a diff, a snippet).
3. **Links before every question:** each URL, file or page the item refers
   to goes in the chat text right before the question, not only inside the
   dialog, where links are not clickable. URLs are written in full
   (`https://…`) so they open in the browser. A file gets a VS Code link
   (`vscode://file/<absolute path>:<line>`) only when the change to review in
   it is large (about 20 lines or more, or hard to judge from the chat).
4. **Context before the question, only when it helps the choice:**
   - A Markdown table when the options differ on several attributes, or the
     item covers several things at once (repos, files, branches).
   - A chart (`mcp__easel__chart`, when available) only for numbers across
     categories or over time. Never a chart for a single number or a yes/no.
5. **Act on each answer before the next question:** apply the decision if it
   is a change in the working tree, leaving it uncommitted. A free-text
   answer that changes the plan is followed as the user wrote it. If an
   answer needs a follow-up to be applied, ask it next.
6. When the backlog is empty, stop.
