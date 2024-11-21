

# How to add gitignore and removed cached files

1. create gitigonre file `nano .gitignore`
2. add stuff in there (use https://www.toptal.com/developers/gitignore)
3. `git add .gitignore` and `git commit ...`
4. Remove Unwanted Tracked Files
	1. `git rm --cached <file_or_directory_path>` ex: `git rm -r --cached node_modules`
	2. if you want to remove all untracked files from git
5. Commit changed files
6. push


# How to create and pull a branch that is not in my local but it's in my origin

```sh
git checkout -b <branch-name> origin/<branch-name>
```


# How to increase size of git repo???

```sh
git config --global http.postBuffer 524288000
```

# How to unstage a file?

```sh
git restore --staged <file>
```