---
allowed-tools: Bash(git checkout -b:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(gh pr create:*), Bash(glab mr create:*)
description: Commit, push, and open a pull request (GitHub) or merge request (GitLab)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Origin remote URL: !`git remote get-url origin`

## Your task

Based on the above changes:

1. Create a new branch if on main
2. Create a single commit with an appropriate message
3. Push the branch to origin
4. Open the request with the CLI that matches the host in the origin remote URL:
   - Host contains `github` (e.g. `github.com`): create a pull request with `gh pr create --title "<title>" --body "<body>"`
   - Host contains `gitlab` (e.g. `gitlab.com`): create a merge request with `glab mr create --title "<title>" --description "<description>" --squash-before-merge --yes`
   - Host matches neither: skip this step and name the remote URL you found
5. You have the capability to call multiple tools in a single response. You MUST do all of the above in a single message. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
