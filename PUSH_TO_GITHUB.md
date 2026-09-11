# How to Push This Documentation to GitHub

## Prerequisites

You'll need:
- GitHub account
- `git` command-line tool installed
- Access to the repository: https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo

## Steps to Push

### Option 1: Using GitHub CLI (Recommended)

```bash
# Install GitHub CLI if you don't have it
# macOS: brew install gh
# Ubuntu: sudo apt install gh
# Windows: choco install gh

# Authenticate with GitHub
gh auth login

# Navigate to the repository (in your local copy)
cd /path/to/openshift-mcp-server-demo

# Push the changes
git push -u origin main
```

### Option 2: Using Personal Access Token

1. Create a Personal Access Token:
   - Go to: https://github.com/settings/tokens
   - Click "Generate new token"
   - Select scopes: `repo` (full control)
   - Copy the token

2. Push using the token:
```bash
cd /path/to/openshift-mcp-server-demo

# Replace YOUR_TOKEN with your actual token
git push https://YOUR_TOKEN@github.com/YamunadeviShanmugam/openshift-mcp-server-demo.git
```

### Option 3: Using SSH

1. Set up SSH keys:
   - Generate: `ssh-keygen -t ed25519 -C "your_email@example.com"`
   - Add to GitHub: https://github.com/settings/keys

2. Change remote to SSH:
```bash
cd /path/to/openshift-mcp-server-demo

git remote set-url origin git@github.com:YamunadeviShanmugam/openshift-mcp-server-demo.git

# Push
git push -u origin main
```

## Verify Push

After pushing, verify on GitHub:
```
https://github.com/YamunadeviShanmugam/openshift-mcp-server-demo
```

## Setting up MkDocs Documentation Site (Optional)

After pushing, you can automatically generate and host the documentation:

### Using GitHub Pages

1. In repository settings, enable GitHub Pages
2. Set source to: main branch, /docs folder
3. Documentation will be available at: `https://YamunadeviShanmugam.github.io/openshift-mcp-server-demo/`

### Alternatively, use ReadTheDocs

1. Go to: https://readthedocs.org
2. Import project from GitHub
3. Select this repository
4. ReadTheDocs will automatically build and host

## Troubleshooting

### "fatal: could not read Username"

You need to authenticate:
```bash
# Option 1: Use gh CLI
gh auth login

# Option 2: Store credentials
git config --global credential.helper store

# Option 3: Use SSH (see above)
```

### "Permission denied"

Make sure you have write access to the repository:
1. Check your GitHub permissions
2. Verify you're using the correct token/SSH key
3. Add your SSH key to GitHub if using SSH

---

**Questions?** Check the README.md for more information!
