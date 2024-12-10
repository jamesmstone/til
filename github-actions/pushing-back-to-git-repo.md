# To push back to a repo in a github action

I wanted to have a GitHub Action step push back to the repo it is running in. 

For that you need the `write`  permission. which can be added like so:

```yaml
permissions:
  contents: write
```

from: [git - Push to origin from GitHub action - Stack Overflow](https://stackoverflow.com/a/58393457)

eg in: [add write permission · jamesmstone/chess@d0a3250 · GitHub](https://github.com/jamesmstone/chess/commit/d0a3250e8cdafa56fa549a7d5513cd0b170a83c8)


```yaml
name: Push commit
on: push
permissions:
  contents: write
jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Create report file
        run: date +%s > report.txt
      - name: Commit report
        run: |
          git config --global user.name 'Your Name'
          git config --global user.email 'your-username@users.noreply.github.com'
          git commit -am "Automated report"
          git push
```


It it discussed on the github blog: [GitHub Actions: Control permissions for GITHUB\_TOKEN - GitHub Changelog](https://github.blog/changelog/2021-04-20-github-actions-control-permissions-for-github_token/)


and here is the docs: [Automatic token authentication - GitHub Docs](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication#modifying-the-permissions-for-the-github_token)