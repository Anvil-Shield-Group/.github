# Git Setup Guide

**Target Audience**: New Developers, Team Members  
**Last Updated**: 2025-06-10 by System  
**Estimated Time**: 30 minutes

# Git Configuration and Setup

This guide will help you configure Git with our team standards and best practices.

## Initial Git Configuration

### 1. User Information
```bash
# Set your name and email (used in commits)
git config --global user.name "Your Full Name"
git config --global user.email "your.email@company.com"

# Verify configuration
git config --global --list
```

### 2. Default Settings
```bash
# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"  # VS Code
# git config --global core.editor "vim"        # Vim
# git config --global core.editor "nano"       # Nano

# Enable color output
git config --global color.ui auto

# Set merge strategy
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Set diff tool
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'
```

### 3. Line Ending Configuration
```bash
# For Windows users
git config --global core.autocrlf true

# For Mac/Linux users
git config --global core.autocrlf input

# Ensure consistent line endings
git config --global core.eol lf
```

## SSH Key Setup

### 1. Generate SSH Key
```bash
# Generate new SSH key
ssh-keygen -t ed25519 -C "your.email@company.com"

# When prompted for file location, press Enter for default
# Set a secure passphrase when prompted

# Start SSH agent
eval "$(ssh-agent -s)"

# Add SSH key to agent
ssh-add ~/.ssh/id_ed25519
```

### 2. Add SSH Key to Git Provider
```bash
# Copy SSH public key to clipboard
cat ~/.ssh/id_ed25519.pub

# On macOS:
# pbcopy < ~/.ssh/id_ed25519.pub

# On Windows:
# clip < ~/.ssh/id_ed25519.pub
```

**Add to GitHub:**
1. Go to GitHub → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste your public key
4. Give it a descriptive title

**Add to Azure DevOps:**
1. Go to Azure DevOps → User Settings → SSH public keys
2. Click "New Key"
3. Paste your public key
4. Give it a descriptive name

### 3. Test SSH Connection
```bash
# Test GitHub connection
ssh -T git@github.com

# Test Azure DevOps connection
ssh -T git@ssh.dev.azure.com

# Expected response: "Hi username! You've successfully authenticated..."
```

## Commit Message Template

### 1. Create Commit Template
```bash
# Create commit message template file
cat > ~/.gitmessage << 'EOF'
# <type>(<scope>): <subject>
#
# <body>
#
# <footer>

# Types:
# feat     - A new feature
# fix      - A bug fix
# docs     - Documentation only changes
# style    - Changes that do not affect the meaning of the code
# refactor - A code change that neither fixes a bug nor adds a feature
# test     - Adding missing tests or correcting existing tests
# chore    - Changes to the build process or auxiliary tools

# Scope examples: auth, api, ui, db, config, deploy

# Subject line rules:
# - Use imperative mood ("Add feature" not "Added feature")
# - Capitalize first letter
# - No trailing period
# - Max 50 characters

# Body:
# - Explain what and why (not how)
# - Wrap at 72 characters
# - Reference issues and pull requests

# Footer:
# - Reference breaking changes
# - Reference closed issues
EOF

# Configure Git to use the template
git config --global commit.template ~/.gitmessage
```

### 2. Verify Template
```bash
# Test the template (will open editor with template)
git commit --allow-empty
# Cancel with :q (vim) or Ctrl+C

# Check configuration
git config --global commit.template
```

## Git Aliases

### 1. Useful Aliases
```bash
# Status and log aliases
git config --global alias.st status
git config --global alias.br branch
git config --global alias.co checkout
git config --global alias.ci commit

# Advanced log aliases
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.lga "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"

# Useful shortcuts
git config --global alias.unstage "reset HEAD --"
git config --global alias.last "log -1 HEAD"
git config --global alias.visual "!gitk"

# Branch management
git config --global alias.cleanup "!git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"
```

### 2. Test Aliases
```bash
# Test your new aliases
git st          # Same as: git status
git lg          # Graphical log
git lga         # Detailed graphical log
```

## Git Hooks Setup

### 1. Pre-commit Hook
```bash
# Navigate to your project repository
cd /path/to/your/project

# Create pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
echo "Running pre-commit checks..."

# Check for staged files
if git diff --cached --quiet; then
  echo "No staged changes found."
  exit 0
fi

# Run linting (adjust command based on your project)
if command -v npm &> /dev/null && [ -f package.json ]; then
  echo "Running ESLint..."
  npm run lint
  if [ $? -ne 0 ]; then
    echo "❌ ESLint failed. Please fix linting errors."
    exit 1
  fi
fi

# Run unit tests (adjust command based on your project)
if command -v npm &> /dev/null && [ -f package.json ]; then
  echo "Running unit tests..."
  npm run test:unit
  if [ $? -ne 0 ]; then
    echo "❌ Unit tests failed. Please fix failing tests."
    exit 1
  fi
fi

echo "✅ Pre-commit checks passed!"
EOF

# Make the hook executable
chmod +x .git/hooks/pre-commit
```

### 2. Commit Message Hook
```bash
# Create commit-msg hook for message validation
cat > .git/hooks/commit-msg << 'EOF'
#!/bin/sh

# Regular expression for conventional commit format
commit_regex='^(feat|fix|docs|style|refactor|test|chore|perf)(\(.+\))?: .{1,50}'

# Read the commit message
commit_message=$(cat "$1")

# Check if commit message matches the pattern
if ! echo "$commit_message" | grep -qE "$commit_regex"; then
  echo "❌ Invalid commit message format!"
  echo ""
  echo "Format: <type>(<scope>): <description>"
  echo ""
  echo "Types: feat, fix, docs, style, refactor, test, chore, perf"
  echo "Scope: optional, e.g., (auth), (api), (ui)"
  echo "Description: brief description, max 50 characters"
  echo ""
  echo "Examples:"
  echo "  feat(auth): add JWT token validation"
  echo "  fix(api): resolve timeout issue"
  echo "  docs: update README with setup instructions"
  echo ""
  exit 1
fi

echo "✅ Commit message format is valid."
EOF

# Make the hook executable
chmod +x .git/hooks/commit-msg
```

## Repository Cloning

### 1. Clone with SSH
```bash
# Clone using SSH (recommended)
git clone git@github.com:company/repository-name.git
cd repository-name

# Set up remote tracking
git remote -v
```

### 2. Initial Setup After Clone
```bash
# Install dependencies (if applicable)
npm install        # Node.js
dotnet restore     # .NET
mvn install        # Java Maven
flutter pub get    # Flutter

# Copy environment configuration
cp .env.example .env
# Edit .env with your local settings

# Verify everything works
npm test           # Run tests
npm run build      # Build project
```

## Global .gitignore

### 1. Create Global Ignore File
```bash
# Create global gitignore file
cat > ~/.gitignore_global << 'EOF'
# macOS
.DS_Store
.AppleDouble
.LSOverride
Icon
._*

# Windows
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db
*.tmp
*.temp
Desktop.ini

# Linux
*~
.fuse_hidden*
.directory
.Trash-*

# IDE files
.vscode/
.idea/
*.swp
*.swo
*~

# Logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env.local
.env.*.local

# Temporary files
*.tmp
*.temp
.cache/
EOF

# Configure Git to use global gitignore
git config --global core.excludesfile ~/.gitignore_global
```

## Verification Checklist

### Configuration Check
```bash
# Verify your Git configuration
git config --global --list

# Should include:
# - user.name
# - user.email
# - init.defaultBranch=main
# - commit.template
# - Various aliases
```

### Test Setup
```bash
# Test SSH connection
ssh -T git@github.com

# Test commit template
cd /tmp
git init test-repo
cd test-repo
git commit --allow-empty  # Should show template
# Cancel and clean up
cd ..
rm -rf test-repo
```

### Hook Testing
```bash
# Test pre-commit hook (in a real repository)
git add .
git commit -m "test commit"  # Should run pre-commit checks

# Test commit message validation
git commit --allow-empty -m "invalid message"  # Should fail
git commit --allow-empty -m "test: valid conventional commit"  # Should pass
```

## Troubleshooting

### Common Issues

#### Permission Denied (SSH)
```bash
# Check SSH agent
ssh-add -l

# If empty, add your key
ssh-add ~/.ssh/id_ed25519

# Check SSH config
cat ~/.ssh/config
```

#### HTTPS to SSH Migration
```bash
# Check current remote
git remote -v

# Change to SSH
git remote set-url origin git@github.com:username/repository.git
```

#### Line Ending Issues
```bash
# Fix line ending issues
git config core.autocrlf true  # Windows
git config core.autocrlf input # Mac/Linux

# Refresh repository
git rm --cached -r .
git reset --hard HEAD
```

### Getting Help
- **Team Chat**: #git-help on Slack
- **Documentation**: [Git Workflow Guide](git-workflow.md)
- **Official Docs**: [Git Documentation](https://git-scm.com/doc)

---

**Guide Owner**: [@tech-lead]  
**Last Updated**: 2025-06-10  
**Next Review**: When onboarding process changes
