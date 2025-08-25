## Branch Naming Conventions

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

### Descriptive and Concise Names

Branch names should clearly describe the purpose of the branch. Keep names relatively short to enhance readability and ease of use in the command line.

**Lowercase and Hyphen-Separated (Kebab Case)**:
- Use lowercase letters for consistency.
- Separate words with hyphens (-) for readability (e.g., feature/add-user-authentication). Avoid spaces, underscores, or camelCase.

**Inclusion of Issue/Ticket IDs**:
- If using an issue tracking system (e.g., Jira, GitHub Issues), include the relevant issue or ticket ID in the branch name for easy cross-referencing (e.g., bugfix/PROJ-123-fix-login-issue).
- Avoid Reserved Names and Invalid Characters:
- Do not use Git's reserved names (e.g., HEAD, FETCH_HEAD).
- Avoid characters that are invalid in Git branch names, such as spaces, colons, question marks, asterisks, or opening square brackets.

## Useful Commands
### Git Config
#### View Config

List config settings (e.g. default branch, username, user email).
```bash
git config --list
```

Show specific values.
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

### Remote
#### View Existing Remote

Display the current remote URL.
```bash
git remote -v
```

#### Set Remote

Update the remote URL.
```bash
git remote set-url origin <NEW_REMOTE_URL>
```

### Checkout

Checkout into new branch based off of remote branch.
```bash
git fetch origin
git checkout -b <LOCAL_BRANCH_NAME> origin/<REMOTE_BRANCH_NAME>
```