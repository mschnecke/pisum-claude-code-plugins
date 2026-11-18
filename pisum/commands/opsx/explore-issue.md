---
description: Read a GitLab issue with glab by number and hand it to OpenSpec's /opsx:explore workflow
argument-hint: '<issue-number>'
disable-model-invocation: false
---

# Explore a GitLab issue with OpenSpec

Read GitLab issue `$ARGUMENTS` with `glab`, then delegate exploration to OpenSpec's native `openspec-explore` skill. This command is the **GitLab adapter**: it gathers the issue content, but it does **not** author the exploration itself — that is owned by the `openspec-explore` skill.

If no issue number was provided, ask which GitLab issue to explore before continuing.

## Instructions

1. **Confirm the GitLab account** before reading (per project GitLab rules):

   ```bash
   glab auth status
   ```

   If not authenticated, stop and tell the user to run `glab auth login` first.

2. **Gather the issue context** using the issue number from `$ARGUMENTS` (the IID). This assembled context is the input you will hand to OpenSpec — collect it, don't format it into a proposal yourself.

   ```bash
   glab issue view <iid> --output json
   ```

   Capture:
   - `.title`, `.description` — the request itself
   - `.labels` — for context and type (feature/chore/etc.)
   - `.milestone` — name and `due_date` (if set); informs scope/timeline
   - `.web_url` — the canonical issue URL (keep verbatim)
   - `.iid` — the issue number

   Then **read all comments** (`--output json` omits them) and drop system notes:

   ```bash
   glab api "projects/:id/issues/<iid>/notes?per_page=100" --paginate \
     | jq '[.[] | select(.system == false) | {author: .author.username, created_at, body}]'
   ```

   Also pull **linked issues and related merge requests** for dependencies/prior work:

   ```bash
   glab api "projects/:id/issues/<iid>/links"                    # linked issues
   glab api "projects/:id/issues/<iid>/related_merge_requests"   # related MRs
   ```

   Where a comment refines, contradicts, or supersedes the description, prefer the latest. Note unresolved discussion as open questions rather than picking a side. If the issue does not exist or a command fails, stop and report the error — do not invent content.

3. **Start the work on a fresh branch** so the exploration and any follow-up work can be reviewed and merged together.

   - Confirm the working tree is clean (`git status --porcelain`). If there are uncommitted changes, stop and ask the user how to proceed — do not stash or discard work silently.
   - Detect the default branch. Prefer `glab repo view -F json | jq -r '.defaultBranch'`; fall back to `git symbolic-ref --short refs/remotes/origin/HEAD` (strip the `origin/` prefix) or `main`.
   - Switch to the default branch and pull the latest with fast-forward only:
     ```bash
     git checkout <default-branch>
     git pull --ff-only
     ```
   - Derive a kebab-case `<slug>` from the issue title (lowercase, ASCII alphanumerics and hyphens only, capped around 40 characters), then create and check out the feature branch:
     ```bash
     git checkout -b <iid>-<slug>
     ```
   - If that branch already exists locally, switch to it instead of recreating it and let the user know.

4. **Verify OpenSpec is initialized.** Confirm an `openspec/` directory exists (`ls openspec/`). If it does not, stop and tell the user to run `openspec init` first — do not scaffold it yourself.

5. **Delegate exploration to OpenSpec.** Invoke OpenSpec's native `openspec-explore` skill, passing the issue context from step 2 — include the title, a faithful summary of the description, the salient comment/decision points, any linked issues/MRs, and the issue URL. Let `openspec-explore` drive the exploration per the repo's current OpenSpec conventions.

   - Keep the handoff faithful to the issue; flag ambiguities as open questions for the user rather than guessing at requirements.
   - Do not pre-empt OpenSpec's exploration with your own conclusions — your job is to supply accurate issue context.
   - Ask the user until all open threads are settled.
