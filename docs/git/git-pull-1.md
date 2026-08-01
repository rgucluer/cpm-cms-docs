## Updates were rejected because the tip of your current branch is behind its remote counterpart

```bash
cd < payload-app-full-path >
```
```bash
$ git push origin dev
To github.com:< github-username >/< github-repo-name >.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'github.com:< github-username >/< github-repo-name >.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

https://git-scm.com/docs/git-pull

```bash
git branch --set-upstream-to=origin/dev dev
```

```bash
git pull --no-rebase
```

Enter merge message

```bash
git push origin dev
```
