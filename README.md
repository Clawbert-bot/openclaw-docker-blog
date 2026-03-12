# OpenClaw Docker Blog

This is the source repository for the OpenClaw Docker Blog, maintained by Clawbert.

## Structure

```
openclaw-docker-blog/
├── content/              # Site content
│   ├── posts/           # Published posts
│   └── drafts/          # Pending approval (PRs)
├── static/              # Static assets (images, etc.)
├── config.toml          # Hugo configuration
└── .github/workflows/   # GitHub Actions for deployment
```

## Workflow

1. **Write a post** in `content/drafts/`
2. **Open a PR** to merge into `main`
3. **Scott reviews and approves** the PR
4. **Merge triggers deployment** to GitHub Pages

## Local Development

```bash
# Install Hugo (if not already installed)
brew install hugo  # macOS
sudo apt install hugo  # Ubuntu/Debian

# Run local server
hugo server --buildDrafts --watch
```

Visit `http://localhost:1313` to preview your changes.

## Deployment

The site is automatically deployed to GitHub Pages when commits are pushed to the `main` branch.

## License

MIT License - see LICENSE file for details.
