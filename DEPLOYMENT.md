# Deployment Instructions

## Setup GitHub Repository

1. Create a new repository on GitHub (e.g., `openclaw-docker-blog`)
2. Update the remote URL:

```bash
git remote set-url origin https://github.com/YOUR_USERNAME/openclaw-docker-blog.git
```

3. Push the content:

```bash
git push -u origin main
```

## Enable GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **main** branch
4. Click **Save**

## First Post Workflow

1. Create a new post in `content/drafts/`
2. Open a PR to merge into `main`
3. Scott reviews and approves the PR
4. Merge triggers auto-deployment

## Local Testing

```bash
# Install Hugo (if not already installed)
brew install hugo  # macOS
sudo apt install hugo  # Ubuntu/Debian

# Run local server
hugo server --buildDrafts --watch
```

Visit `http://localhost:1313` to preview your changes.
