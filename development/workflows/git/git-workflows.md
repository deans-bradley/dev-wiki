# Git Workflows

## Table of Contents

- [Git Setup & Configuration](#git-setup--configuration)
  - [Git Config](#git-config)
    - [View Config](#view-config)
    - [Update Config](#update-config)
  - [Git Remote](#git-remote)
    - [View Existing Remote](#view-existing-remote)
    - [Set Remote](#set-remote)
- [Branch Management](#branch-management)
  - [Branch Naming Conventions](#branch-naming-conventions)
    - [Branch Types](#branch-types)
    - [Naming Guidelines](#naming-guidelines)
  - [Git Checkout](#git-checkout)
- [Release Management](#release-management)
  - [Git Tags](#git-tags)
    - [Types of Tags](#types-of-tags)
    - [Release Workflow](#release-workflow)
    - [Common Pitfalls & Solutions](#common-pitfalls--solutions)
    - [Useful Tag Commands](#useful-tag-commands)
    - [Workflow Template](#workflow-template)
    - [Best Practices](#best-practices)

---

## Git Setup & Configuration

### Git Config

#### View Config

List config settings (e.g. default branch, username, user email)
```bash
git config --list
```

Show specific values
```bash
git config user.name
git config user.email
```

#### Update Config
```bash
git config user.name "<YOUR_NAME>"
```

To update a variable globally.
```bash
git config --global user.name "<YOUR_NAME>"
```

### Git Remote

#### View Existing Remote
Display the current remote URL
```bash
git remote -v
```

#### Set Remote
Update the remote URL
```bash
git remote set-url origin <NEW_REMOTE_URL>
```

---

## Branch Management

### Branch Naming Conventions

#### Branch Types

- **`feature/`**: For new features or enhancements  
    e.g. `feature/add-user-auth`
    
- **`bugfix/`** or **`fix/`**: For bug fixes  
    e.g. `bugfix/login-crash` or `fix/cart-total`
    
- **`hotfix/`**: Urgent fixes that go straight to production  
    e.g. `hotfix/prod-build-fail`
    
- **`chore/`**: Routine tasks, cleanup, version bumps, etc.  
    **This is the one you want for .NET version updates**  
    e.g. `chore/update-dot-net8`
    
- **`refactor/`**: Code restructuring with no feature/bug change  
    e.g. `refactor/extract-auth-service`
    
- **`test/`**: Test-only changes or experiments  
    e.g. `test/spike-grpc-integration`
    
- **`release/`**: Prepping code for a release  
    e.g. `release/v2.3.0`
    
- **`docs/`**: Documentation updates  
    e.g. `docs/api-usage-update`

#### Naming Guidelines

Branch names should clearly describe the purpose of the branch. Keep names relatively short to enhance readability and ease of use in the command line.

**Lowercase and Hyphen-Separated (Kebab Case)**:
- Use lowercase letters for consistency.
- Separate words with hyphens (-) for readability (e.g., feature/add-user-authentication). Avoid spaces, underscores, or camelCase.

**Inclusion of Issue/Ticket IDs**:
- If using an issue tracking system (e.g., Jira, GitHub Issues), include the relevant issue or ticket ID in the branch name for easy cross-referencing (e.g., bugfix/PROJ-123-fix-login-issue).
- Avoid Reserved Names and Invalid Characters:
- Do not use Git's reserved names (e.g., HEAD, FETCH_HEAD).
- Avoid characters that are invalid in Git branch names, such as spaces, colons, question marks, asterisks, or opening square brackets.

### Git Checkout
Checkout into new branch based off of remote branch
```bash
git fetch origin
git checkout -b <LOCAL_BRANCH_NAME> origin/<REMOTE_BRANCH_NAME>
```

---

## Release Management

### Git Tags
Tags are pointers to specific commits - think of them as "bookmarks" in your Git history. Unlike branches, tags don't move - they always point to the same commit.

#### Types of Tags
1. **Lightweight Tags**
    ```bash
    git tag v1.0.0
    ```
    - Just a name pointing to a commit
    - No additional metadata
2. **Annotated Tags** (recommended for releases)
    ```bash
    git tag -a v1.0.0 -m "Release version 1.0.0 with new features"
    ```
    - Contains metadata (author, date, message)
    - Better for release management

#### Release Workflow
Here is a solid workflow to follow:

##### Step 1
Make your changes

```bash
# Make code changes, add features, etc.
git add .
git commit -m "Add new feature X"
```

##### Step 2
Update version in code

```bash
# Update version in Promptly.csproj (if needed)
# Update CHANGELOG, README, etc.
git add .
git commit -m "Bump version to 1.0.3"
```

##### Step 3 
Push changes to `main` (or whichever is your active branch)

```bash
git push origin main
```

##### Step 4
Create Tag on Latest Commit

```bash
# CRITICAL: Make sure you're on the right commit!
git log --oneline -3  # Verify you're on the commit you want to tag

# Create annotated tag (recommended)
git tag -a v1.0.3 -m "Release v1.0.3: Added icon and improved documentation"

# OR lightweight tag
git tag v1.0.3
```

##### Step 5
Push the Tag

```bash
git push origin v1.0.3
```

#### Common Pitfalls & Solutions
##### Tagging the Wrong Commit
**DON'T**: Create tag without checking current position
```bash
git tag v1.0.3
```

**DO**: Always verify first
```bash
git log --oneline -1        # Shows current commit
git status                  # Shows if you have uncommitted changes
git tag v1.0.3              # Now create tag
```

##### Creating Tag on Old Commit
**DON'T**: Assume you're on latest
```bash
git tag v1.0.3
```

**DO**: Pull latest and verify
```bash
git pull origin main
git log --oneline -1
git tag v1.0.3
```

##### Forgetting to Push Tags
Tags are **NOT** pushed automatically with git push:
```bash
git push origin main        # This WON'T push tags
```

Tags must be pushed explicitly
```bash
git push origin v1.0.3      # Push specific tag
```
**OR**
```bash
git push origin --tags      # Push all tags (be careful!)
```

#### Useful Tag Commands
##### View Tags
```bash
git tag                    # List all tags
git tag -l "v1.*"          # List tags matching pattern
git show-ref --tags        # Show which commit each tag points to
```

##### Tag Specific Commit
Helpful if you made a mistake
```bash
git tag v1.0.3 abc1234     # Tag specific commit hash
git tag -a v1.0.3 abc1234 -m "Release message"  # Annotated tag on specific commit
```

##### Fix Wrong Tags
```bash
# Delete local tag
git tag -d v1.0.3

# Delete remote tag
git push --delete origin v1.0.3

# Create new tag on correct commit
git tag v1.0.3
git push origin v1.0.3
```

##### View Tag Details
```bash
git show v1.0.3            # Show tag details and commit info
git log v1.0.2..v1.0.3     # Show commits between two tags
```
#### Workflow Template
Here's a copy-paste template that can be used when making releases:
```bash
# 1. Finish all your changes and commit them
git add .
git commit -m "Your final changes"

# 2. Update version number if needed
# (edit Project.csproj, README, etc.)
git add .
git commit -m "Bump version to 1.0.4"

# 3. Push to main
git push origin main

# 4. Verify you're on the right commit
git log --oneline -3
git status

# 5. Create annotated tag
git tag -a v1.0.4 -m "Release v1.0.4: Brief description of changes"

# 6. Push the tag
git push origin v1.0.4

# 7. Verify tag was created correctly
git show-ref --tags | grep v1.0.4
```


#### Best Practices
- **Always use semantic versioning**: `v1.2.3` (major.minor.patch)
- **Be consistent with tag naming**: Always use `v` prefix
- **Use annotated tags for releases**: They contain more metadata
- **Test locally first**: Create a test tag to verify your - workflow
- **Keep a changelog**: Document what's in each release