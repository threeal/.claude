---
name: github-actions-facts
description: Reference facts about GitHub Actions defaults and behavior (token permissions, etc). TRIGGER - editing/reviewing a .github/workflows/*.yaml file, or discussing GITHUB_TOKEN permissions, workflow default permissions, or GitHub Actions security defaults.
---

## GITHUB_TOKEN default permissions

Since February 2023, GitHub sets the default `GITHUB_TOKEN` permissions for **newly created repositories** to read-only (`contents: read`, etc.), not read-write. Source: https://github.blog/changelog/2023-02-02-github-actions-updating-the-default-github_token-permissions-to-read-only/

This default is a repo/org/enterprise-level setting (Settings → Actions → General → Workflow permissions), so it can still be overridden — repos created before Feb 2023, or ones where someone flipped the toggle to "Read and write permissions," will default to read-write instead.

To check the actual current setting for a specific repo rather than assuming:

    gh api repos/<owner>/<repo>/actions/permissions/workflow
