# Claude Code Configuration

Personal [Claude Code](https://claude.ai/code) configuration — settings, instructions, and customizations applied globally across all projects.

## Usage

To apply this configuration to an existing `.claude` directory:

```sh
cd ~/.claude
git init
git remote add origin git@github.com:threeal/.claude.git
git fetch
git checkout main -tf
```

> The `-tf` flags force the checkout and set up remote tracking, so existing files in the directory are overwritten by the repository contents.

## License

This project is licensed under the [MIT License](LICENSE).

Copyright © 2026 [Alfi Maulana](https://github.com/threeal)
