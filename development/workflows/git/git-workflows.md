## Branch Naming Conventions

- **`feature/`**: For new features or enhancements  
    e.g. `feature/AddUserAuth`
    
- **`bugfix/`** or **`fix/`**: For bug fixes  
    e.g. `bugfix/LoginCrash` or `fix/CartTotal`
    
- **`hotfix/`**: Urgent fixes that go straight to production  
    e.g. `hotfix/ProdBuildFail`
    
- **`chore/`**: Routine tasks, cleanup, version bumps, etc.  
    **This is the one you want for .NET version updates**  
    e.g. `chore/UpdateDotNet8`
    
- **`refactor/`**: Code restructuring with no feature/bug change  
    e.g. `refactor/ExtractAuthService`
    
- **`test/`**: Test-only changes or experiments  
    e.g. `test/SpikeGrpcIntegration`
    
- **`release/`**: Prepping code for a release  
    e.g. `release/v2.3.0`
    
- **`docs/`**: Documentation updates  
    e.g. `docs/APIUsageUpdate`

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