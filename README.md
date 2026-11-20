# ways-of-working

Kim's **ways-of-working** skill for Claude: how a change runs from idea to deploy (decision table or mockup first,
approval gate, decisions log, question tracker, Asana, which other skills to use).

This repo is the master copy. Every change goes through a branch and a commit, so there is always a history of what
changed and why. The copy in claude.ai is just an upload of this file.

## What's in here

| Path | What it is |
|---|---|
| `ways-of-working/SKILL.md` | The skill itself, the only file Claude reads |
| `.github/workflows/package-skill.yml` | Builds the upload zip automatically on every change to `main` |

## Put the latest version into claude.ai (about 1 minute)

1. **Download the zip:**
   <https://github.com/KimHNguyen93/ways-of-working/releases/latest/download/ways-of-working.zip>
   This link always gives the newest version from `main`. It is rebuilt a minute or so after each merge.
2. In claude.ai, open **Customize → Skills**.
3. Delete (or turn off) the old **ways-of-working** skill, so there aren't two copies.
4. Click **+** → **Create skill** → **Upload a skill**, and choose `ways-of-working.zip`.
5. Check it shows in the list and is switched on.

If **Skills** isn't there: **Settings → Capabilities** → turn on **Code execution and file creation**
(also called *Create and edit files*).

> Don't use GitHub's green **Code → Download ZIP** button. That zip has the wrong layout
> (`ways-of-working-main/ways-of-working/SKILL.md`), and claude.ai won't find the skill in it.

### Fallback: build the zip yourself (Mac)

Use this only if the download link above isn't working.

```bash
git clone https://github.com/KimHNguyen93/ways-of-working
```

```bash
cd ways-of-working && zip -r ~/Downloads/ways-of-working.zip ways-of-working
```

Then upload `~/Downloads/ways-of-working.zip` as in steps 2–5.

## Changing the skill

1. Ask Claude for the change. It shows the before/after first; nothing is committed until you say **approved**.
2. Claude commits on a branch and pushes it; you merge to `main` (or ask for a PR).
3. Wait about a minute, then re-upload with the steps above.

Check which version is live: the [latest release](https://github.com/KimHNguyen93/ways-of-working/releases/latest)
says which commit the zip was built from.

## Rules for the zip (from Claude's docs)

- The zip must contain the **folder** `ways-of-working/` with `SKILL.md` inside it. A `SKILL.md` on its own at the
  top of the zip isn't recognised.
- The folder name must match `name:` at the top of `SKILL.md` (`ways-of-working`).
- `description:` can be up to 1,024 characters.
