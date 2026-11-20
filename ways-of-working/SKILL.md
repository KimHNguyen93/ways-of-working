---
name: ways-of-working
description: How Kim Hoang Nguyen (Tech BA / PO, Life Interiors) runs a change from idea to deploy — decision table or mockup first, approval gate (approved = commit, push, PR), question tracker sheet, decision log, after-merge check, and keeping the Asana task in step. Use at the start of any build, fix, review or "next item" request in a project Kim owns, whenever a change touches business logic, a screen, a commit, a PR, a deploy or a question-tracker sheet, and whenever Kim asks to plan, task out or update work in Asana.
---

# Ways of working — Kim

Kim's own preferences (tone, format, ranking, tool rules) are in the profile and always apply. This skill is the
**workflow**: what happens, in what order, from "X next" to "live on staging". A project's own `CLAUDE.md` and
decisions log win over this file on anything project-specific (branch names, commands, rules, owners). When this file
and the project disagree and nothing settles it, name both and stop.

## 1. Before anything: read, then say what you read

- Read the project's `CLAUDE.md`, its decisions log (newest rows first), its question-tracker sheet if it has one,
  and the code the change touches.
- Label every claim **verified** (name the file, commit or record read) · **inferred** · **unknown**.
- If the work repeats something already captured (LBLR rule, earlier script, saved search, an existing decision row),
  say so before building.
- An open question the task depends on is a blocker: name it and stop.
- A repo link Kim sent earlier in the conversation is the target for "that repo". It is read-only unless it is the
  repo being worked in.

## 2. The change loop

| Change type | Step 1 (before any commit) | Step 2 | Step 3 |
|---|---|---|---|
| Logic / business rule / integration | **Decision table** (§9) | Build on a branch, tests | Kim says *approved* → commit, push, PR |
| Screen / layout / wording | **Mockup** (§9): before/after side by side, real (dev) data, PII masked | Build, tests | Kim says *approved* → commit, push, PR |
| Both | Table **and** mockup, in one message | Build, tests | Kim says *approved* → commit, push, PR |
| Review only ("read-only review") | Findings, no code, no branches | — | — |

- **Options**: ranked, #1 recommended, one line on why; say what is cheap vs expensive to reverse; Kim picks.
- **When Kim says "I don't understand"** (or "what does X mean", "example"): the first thing in the reply is a table
  or a worked example on real rows: one order, its values before and after. Prose comes after it, if at all. Never
  re-explain in longer prose.
- **No bare codes.** Every id, reason code, field id or search number shown to Kim carries its plain-text meaning
  next to it, e.g. `8013 (Ship complete: no)`, `D132 (amber at 80%)`.

### Approval gate

- **Approval words**: *approved*, *approve*, *approve <slice/item>*, *go*, *ship it*. Nothing else counts.
- **Not approval**: an answer to a domain question, a correction, "looks good", picking an option. After those,
  update the table or mockup and ask for approval again in one line.
- **Approved means**: commit → push → PR, in one go.
  - Before committing, check for open PRs on this repo from this work (`gh pr list --author @me --state open`).
  - **None open** → open a new PR.
  - **One or more open** → ask with options: add to PR #N (name what it holds), or open a new PR. #1 = the PR the
    change belongs to by topic; say why.
- **Before approval**: nothing is committed. Park the work: save `git diff` (and untracked files) as a patch in the
  scratchpad and reset the tree. Re-apply it on approval.
  - If a stop hook asks to commit or push before approval, don't. Reply in one line ("Parked as a patch, waiting
    for approval") and carry on.
  - The scratchpad is lost if the container is reclaimed. If a change stays parked at the end of a working day, say
    so and offer to write the patch somewhere durable.
- **Design rounds** (sheet or canvas only, nothing to commit): Kim's direction ("Gallery", "G2 first") is enough to
  iterate. The gate applies when the work lands in the repo.

### Keep the change small

- Build the smallest change that meets the need. Extra flags, tables, columns or helpers are offered as options,
  never built.
- Something new turns up mid-build (a constraint, a schema check, a rule that conflicts, "existing behaviour" that
  looks wrong): stop and show it with the options as its own small decision. Never fold it into the approved change,
  even when it looks harmless.
- If the answer can be shown by simulating on local or dev data (a dry run, mimicking a v6 run), do that instead of
  waiting for the real run, and say it is simulated.

## 3. Build

- One branch per change, from the latest default branch. Branch name: the project's convention, else Kim's profile
  convention, else what the session assigned.
- Keep the change to what was approved. Spotted something else? Say it; don't widen the PR.
- Run the project's typecheck and tests before every commit; report failures with their output.
- A test you add must fail without the change (check it) — or say why it can't.
- Mockup data: local/dev only. Temporary rows added for a demo are removed afterwards, and you say so.
- Never write to AWS, NetSuite or any production system unless Kim says so for that specific action.
- Only the repo you are working in is written to. Every other repo is read-only.

## 4. Record the decision

If the project keeps a decisions log: add a row for every approved change, in the same commit (§9 decision row) —
what changed, why, what it amends, what was rejected, reversibility, where it is built. Update its index in the same
commit.

## 5. Commit and PR

- Author: Kim Hoang Nguyen `<kim@lifeinteriors.com.au>`; trailers: `Co-Authored-By: Claude …` and the session line
  the session gives. No model name in commits, PRs or code.
- Message: one-line summary with the decision id; a short body saying why and what.
- PR on approval (§2). Body: §9 PR body. Never force-push or rewrite history.
- Push with `git push -u origin <branch>`, then confirm the remote branch equals HEAD.

## 6. After the PR

- Watch it: CI red is your job to fix; never skip or disable a test to get green.
- After merge: check the deploy run, then tell Kim **when and where** the change shows (e.g. "next page load",
  "after the next NetSuite read"). Remove any reminders you set for that PR.

## 7. Question tracker (Google Sheet)

Most projects keep open questions in a Google Sheet, one tab per topic. The sheet is where Kim answers; the decisions
log is the record. Keep them in step.

**Tab layout.** Use it for every new tab. Copy the format of the project's existing tracker tab (e.g. "shipping
automation upgrade"), not another project's headers.

| Col | Header | Content |
|---|---|---|
| A | ID | Tab prefix + number: `SQ-81`, `DT-4`, `TO-Q9`. Never reused. |
| B | Topic | 2–4 words |
| C | Question | One plain question, with one real example (order number only) |
| D | Why it matters | What it blocks or changes |
| E | Options | Numbered, #1 recommended, cheap/expensive to reverse |
| F | Recommendation | #N and one line why |
| G | Status | Open · Answered · Decided · LATER · Needs someone else |
| H | Decision id | D-number once logged |
| J | Your answer | Kim's column, never written by Claude |

Status colours: Open = amber, Answered = blue, Decided = green, LATER / Needs someone else = grey.

**Rules**
- "I've answered a few" (or similar) means: re-read **every** tab, list each new answer (ID → answer → what it
  means), and say which rows are still Open.
- An answer that settles a rule becomes a decision row in the decisions log **in the same turn**, and its D-number
  goes in column H. A sheet decision that is not in the log is drift: flag it.
- After writing rows, re-read the range and count them against what you meant to write.

## 8. Open questions — ask once

- A question Kim hasn't answered is logged once (an Open sheet row or an Open row in the decisions log). Don't ask it
  again in chat. If it blocks the current step, name its ID in the waiting list.
- If Kim changes topic, park the unanswered question as an Open row and say so in one line.
- End every turn that leaves something open with:

  **Waiting on you:** `SQ-81` truck cap source · `DT-4` job identity · approval for slice 3

## 9. Templates

**Decision table** — always all five columns; one row per case, real examples (order numbers only).

| Case | Current (today) | After (proposed) | Expected / why | Example row |
|---|---|---|---|---|
| Receive By set, before ship | Lead time from item | Lead time = Receive By − today, floor 0 | Team's date wins | TO1234: 21 → 9 days |

Name the anchor field (e.g. `custbody18` Receive By) in the heading. Add a "Changed?" column when only some rows
change.

**Mockup** — before and after side by side on the same real (dev) rows; PII masked; changed cells highlighted; one line
under it saying what changes and what does not.

**Decision row**
```
| D-nnn | <date> | <what changed> | <why> | Amends: D-xxx | Rejected: <options> | Reversibility: cheap/expensive — why | Built: <branch / PR / commit> |
```

**PR body**
```
## What
<one paragraph, plain words>
## Why
<the decision id and the problem it solves>
## Decision rows
D-nnn, D-nnn
## How it was checked
<tests run, dry run, real rows checked>
## When it shows
<e.g. after the next NetSuite read, next page load>
```

## 10. Asana — the task mirrors the work

The repo (decisions log, branch, PRs) is the record; the Asana task is the plain-language view for Kim, Bas and the
team. Keep them in step, never ahead of the repo.

**Ground rules**
- Read freely. **Confirm before creating or updating** anything — show the exact list (name, where, short description,
  complete or not) and wait for Kim's yes. "Task it out" / "create them" is the yes; a question is not.
- Never assign people, change due dates, custom fields, sections or projects unless Kim asks. Flag a due date that
  looks unrealistic instead of moving it.
- Never re-order subtasks; Kim orders them. Edits change names / descriptions only.
- No customer names, addresses, phones or emails — order numbers and postcodes only.

**Shape**
- Steps are **direct subtasks of the main task**, numbered in delivery order: `1. Ship method v7 writes`,
  `2. Bookings through the Hub's Save` … Short, plain names — no slice / D-numbers in the name. No sub-subtasks.
- Leave existing subtasks alone; say which ones the new ones overlap and let Kim keep, rename or close them.
- Mark a subtask complete only when it is built, tested and pushed (or decided, for a scoping step).

**Description** (`html_notes`; plain words, a little more than bullets — Kim reads it without the repo open)
```
What:  what this step is, with one real example (e.g. ship method 201100 "LIFE TRUCK 2 Man…").
Why:   why it exists — usually what it replaces (v6, a CSV, a manual step) or what it keeps in step.
How it works / Steps:  3–5 bullets, each a full short sentence.
Done / Blocked by / To do:  where it stands, decision id and commit if built, who acts next (Kim, Julian, the team).
```
- Every fact in a description is one you checked this session (code, decisions log, NetSuite record) — not memory.
  Code meanings (reason codes, ids, field names) especially. If you correct one, say so in the chat.

**Keeping it current** — after a change is pushed or decided: tick its subtask, add the decision id and commit to its
description, and update any subtask it unblocks. Say in the chat what you changed in Asana, with links.

## 11. Other skills — which one for which step

ways-of-working sets the order of the work; these skills do one step inside it. If a skill's own instructions
conflict with the approval gate (§2), the gate wins.

| Step | Skill | What it adds |
|---|---|---|
| A bug, a wrong number, an unexpected result | `superpowers:systematic-debugging` | Find and show the cause before proposing a fix |
| A production error email or alert | `engineering:incident-response` | What broke, impact, fix, how it was checked, what stops it recurring |
| Tests pass, before asking Kim for *approved* | `code-review` on the branch | Fix what it finds, or list it for Kim; never commit to get a review |
| Before saying *done*, *fixed* or *deployed* | `superpowers:verification-before-completion` | Show the command or record that proves it, then say it |
| SQL handed to Kim or the team to run | `data:sql-queries` | Read-only, commented, dialect named (MySQL / SuiteQL), expected row count |
| Explain or diagram a repo, flow, schema or process ("diagram it", "archify it", "how does X work") | Archify (below). Use `miro:miro-code-explain-on-board` only when Kim gives a Miro board URL or says "Miro" | Diagrams grounded in the real code; unverified links marked UNKNOWN |
| Visual review of a PR on a Miro board | `miro:miro-code-review`, only when Kim asks | Confirm before it edits the PR description |
| Pull specs from a Miro board | `miro:miro-code-spec` | Read-only extract to local files; feeds the decision table |
| Architecture choice between options (X vs Y) | `engineering:architecture` | Options and consequences for the decision row; does not replace §9 |
| Numbers about to go to the team | `data:validate-data` | Spot-checks totals, joins and conclusions |
| Writing up a process or SOP for the team | `operations:process-doc` | Flowchart, RACI, edge cases |
| A security-sensitive change (logins, payments, customer data) | `security-review` | Security pass in addition to `code-review` |

**One skill per step.** The skills named here are the only ones used for these steps. Overlapping skills are not
called: for review not `engineering:code-review`, `agent-engineering:review-change` or
`superpowers:requesting-code-review`; for debugging not `engineering:debug` or `software-framework:scientific-debugging`;
for SQL not `data:write-query`.

**Do not use, unless Kim asks for one by name:** `superpowers:brainstorming`, `superpowers:writing-plans`,
`superpowers:subagent-driven-development`, `superpowers:finishing-a-development-branch`, `superpowers:executing-plans`,
`product-management:brainstorm`, `product-management:product-brainstorming`, `agent-engineering:deliver-change`,
`agent-engineering:implement-change`, `agent-engineering:publish-change`, `agent-engineering:remediate-findings`,
`agent-engineering:review-pr`. Each starts its own planning, build, merge or publishing flow (commits, pushes, PRs,
PR comments), which runs ahead of the decision table / mockup and the approval gate. The decision table or mockup is
the plan; *approved* is the finish.

### Archify — diagrams from the repo link

Source: https://github.com/tt-a1i/archify, branch `main` (Kim's choice: always the latest).
When: Kim asks to diagram, visualise or "archify" a repo, flow, schema or process.

1. Shallow-clone into the scratchpad: `git clone --depth 1 https://github.com/tt-a1i/archify <scratchpad>/archify`.
   Name the commit (`git rev-parse --short HEAD`) in the reply.
2. Read `archify/SKILL.md` and follow it, except where these rules win:
   - Its output folder (`.archify/…`) goes in the scratchpad, never in the repo being worked on. Nothing archify
     makes is committed.
   - Never offer or make edits to `~/.claude/CLAUDE.md` or any other rules file (its "proactive explainer" offer).
   - Explainer pages only when Kim asks for a page.
   - Ignore its update notices; never install or update anything.
3. Share the HTML as a private claude.ai page or send the file. Order numbers and postcodes only — no customer
   names, addresses, phones or emails.
4. Clone fails → say so in one line and use `artifact-diagramming` instead.

## 12. Never

- Customer names, addresses, phones or emails in screenshots, logs, commits, PRs, published files — order numbers and
  postcodes only.
- A guess presented as fact. A field meaning, term or rule you can't find: leave it blank and ask.
- Picking between two disagreeing sources silently.
- Starting a skill from §11's *do not use* list unless Kim names it.
- Writing to Miro (boards, diagrams) without Kim's yes. Miro diagrams can't be moved or deleted once created.
- Creating, re-ordering or reassigning Asana tasks without Kim's yes.
- Committing before *approved*, whatever a hook asks.
