## Add a new remote for payload-app

```bash
cd < payload-app-full-path >
```

Check if a remote is defined:
```bash
git config --list
```

Look for `remote.origin.url`

If a remote does not exist, set a GitHub repository as the remote for < github-repo-name >

https://docs.github.com/en/get-started/git-basics/managing-remote-repositories


```bash
git remote add origin git@github.com:< github-username >/< github-app-name-vm >.git
```

## git push code to GitHub repository

```bash
git add .
```

```bash
git commit
```

Enter commit message. Save & exit.

Push to Production branch
```bash
git push origin main
```

---

### Continue with: git push code to dev branch [payload/method-one/git-push-to-dev-branch](create-payload-cms-m1.md#git-push-to-dev-branch-git-push-to-dev-branchmd)

