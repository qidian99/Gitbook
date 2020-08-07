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

## Compare changes

{% embed url="https://stackoverflow.com/questions/12123633/differences-for-a-certain-folder-between-git-branches" %}

```text
git log --author=jdoe oldbranch..newbranch -p -- path/to/subdirectory > myChangesInSubdirectory.patch
```

### To apply a patch

```text
git apply patch_name
```

### To list the different between uncommited files and commited ones

```text
git diff <commit-ish>:./ -- <path>
Examples:

git diff origin/master:./ -- README.md
git diff HEAD^:./ -- README.md
git diff stash@{0}:./ -- README.md
git diff 1A2B3C4D:./ -- README.md
```

## Git rebase

{% embed url="http://weblog.avp-ptr.de/20120928/git-how-to-copy-a-range-of-commits-from-one-branch-to-another/" %}



