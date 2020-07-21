# Git命令

## Revert local add or commit

{% embed url="https://github.com/microsoft/vscode/issues/44776" %}

* When the last commit is **not** the initial commit
  * No changes. i.e. Execute `git reset HEAD~`
* When the last commit **is** the initial commit
  * Execute `git update-ref -d HEAD` followed by ~~`git rm -fr .`~~ `git rm --cached -r .` cf. [How to revert initial git commit?](https://stackoverflow.com/questions/6632191/how-to-revert-initial-git-commit)

## Clone submodules

With version 2.13 of Git and later, `--recurse-submodules` can be used instead of `--recursive`:

```text
git clone --recurse-submodules -j8 git://github.com/foo/bar.git
cd bar
```

Editor’s note: `-j8` is an optional performance optimization that became available in version 2.8, and fetches up to 8 submodules at a time in parallel — see `man git-clone`.

With version 1.9 of Git up until version 2.12 \(`-j` flag only available in version 2.8+\):

```text
git clone --recursive -j8 git://github.com/foo/bar.git
cd bar
```

With version 1.6.5 of Git and later, you can use:

```text
git clone --recursive git://github.com/foo/bar.git
cd bar
```

For already cloned repos, or older Git versions, use:

```text
git clone git://github.com/foo/bar.git
cd bar
git submodule update --init --recursive
```

### To pull all submodules to the latest commit:

```text
git pull --recurse-submodules
```

