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