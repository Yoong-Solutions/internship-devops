# Git Basics

## Introduction to Git
Git is a distributed version control system that helps track changes in source code during software development. It's designed for coordinating work among programmers, but it can be used to track changes in any set of files.

## Why Git?
- Distributed version control
- Fast and efficient
- Strong support for non-linear development
- Data integrity
- Branching and merging capabilities

## Basic Concepts

### Repository
- Local repository
- Remote repository
- Working directory
- Staging area
- Commit history

### Key Terms
- Commit: A snapshot of changes
- Branch: A separate line of development
- Merge: Combining changes from different branches
- Clone: Creating a local copy of a repository
- Pull: Updating local repository with remote changes
- Push: Sending local changes to remote repository

## Basic Commands

### Setup and Configuration
```bash
# Configure user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Initialize a new repository
git init

# Clone a repository
git clone <repository-url>
```

### Basic Workflow
```bash
# Check status of working directory
git status

# Add files to staging area
git add <file>
git add .

# Commit changes
git commit -m "Commit message"

# View commit history
git log
```

### Branching
```bash
# Create a new branch
git branch <branch-name>

# Switch to a branch
git checkout <branch-name>

# Create and switch to a new branch
git checkout -b <branch-name>

# List all branches
git branch
```

### Remote Operations
```bash
# Add remote repository
git remote add origin <repository-url>

# Push changes to remote
git push origin <branch-name>

# Pull changes from remote
git pull origin <branch-name>

# Fetch changes from remote
git fetch origin
```

## Best Practices

### Commit Messages
- Use clear and descriptive messages
- Start with a verb
- Keep it concise
- Reference issue numbers if applicable

### Branching Strategy
- Use meaningful branch names
- Keep branches short-lived
- Delete merged branches
- Use feature branches for new features
- Use release branches for releases

### Workflow
1. Pull latest changes
2. Create feature branch
3. Make changes
4. Commit changes
5. Push to remote
6. Create pull request
7. Review and merge

## Common Scenarios

### Undoing Changes
```bash
# Discard changes in working directory
git checkout -- <file>

# Unstage changes
git reset <file>

# Amend last commit
git commit --amend
```

### Resolving Conflicts
1. Pull latest changes
2. Identify conflicts
3. Resolve conflicts manually
4. Add resolved files
5. Commit changes

### Stashing Changes
```bash
# Save changes temporarily
git stash

# List stashed changes
git stash list

# Apply stashed changes
git stash apply

# Drop stashed changes
git stash drop
```

## Advanced Topics

### Git Hooks
- pre-commit
- post-commit
- pre-push
- post-merge

### Git Aliases
```bash
# Create alias
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
```

### Git Workflows
- Centralized Workflow
- Feature Branch Workflow
- Gitflow Workflow
- Forking Workflow

## Resources
- [Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [GitHub Guides](https://guides.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf) 