# Git命令

## Revert local add or commit

{% embed url="https://github.com/microsoft/vscode/issues/44776" %}

* When the last commit is **not** the initial commit
  * No changes. i.e. Execute `git reset HEAD~`
* When the last commit **is** the initial commit
  * Execute `git update-ref -d HEAD` followed by ~~`git rm -fr .`~~ `git rm --cached -r .` cf. [How to revert initial git commit?](https://stackoverflow.com/questions/6632191/how-to-revert-initial-git-commit)

