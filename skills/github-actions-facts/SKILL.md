---
name: github-actions-facts
description: GitHub Actions defaults that are easy to assume wrong, and how to check them. TRIGGER - editing/reviewing a .github/workflows/*.yaml file, or discussing GITHUB_TOKEN permissions, workflow default permissions, or GitHub Actions security defaults.
---

## GITHUB_TOKEN Default Permissions

Don't assume a repo's default `GITHUB_TOKEN` permissions — check them:

```
gh api repos/<owner>/<repo>/actions/permissions/workflow
```

Why: the read-only default ([since February 2023](https://github.blog/changelog/2023-02-02-github-actions-updating-the-default-github_token-permissions-to-read-only/)) applies only to repos created after that change, and it's a repo/org/enterprise setting (Settings → Actions → General → Workflow permissions) that anyone can flip. An older repo, or one where someone selected "Read and write permissions," defaults to read-write instead.
