# GitHub CLI Quick Setup Guide

A simple guide to install and authenticate with GitHub using GitHub CLI.

## Installation

### Windows
```bash
winget install --id GitHub.cli
```

### Linux (Ubuntu/Debian)
```bash
sudo apt install gh
```

### macOS
```bash
brew install gh
```

## Authentication

After installation, authenticate with your GitHub account:

```bash
gh auth login
```

Follow the prompts:
- Select **GitHub.com**
- Choose **HTTPS** or **SSH**
- Select **Login with a web browser** (recommended)
- Copy the one-time code and paste it in your browser

## Usage

Once authenticated, you can push to your repositories:

```bash
git push
```

No need to enter credentials again!

## Verify Installation

Check if GitHub CLI is installed correctly:

```bash
gh --version
```

Check authentication status:

```bash
gh auth status
```

## Common Commands

```bash
# Clone a repository
gh repo clone username/repo-name

# Create a new repository
gh repo create

# View pull requests
gh pr list

# Create a pull request
gh pr create
```

---

For more information, visit the [official documentation](https://cli.github.com/manual/).
