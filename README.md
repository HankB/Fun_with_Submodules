# Fun_with_Submodules

Play with git submodules.

## 2025-03-19 Motivation

I tried using git submodules on a *real* and quickly learned I need to learn a bit more about git submodules.

* <https://git-scm.com/book/en/v2/Git-Tools-Submodules> The docs.
* <https://github.com/HankB/SM> A submodule

```text
git submodule add git@github.com:HankB/SM.git # Note: "clone" URL, not "repo" URL

```

Result:

```text
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git submodule add git@github.com:HankB/SM.git
Cloning into '/home/hbarta/Programming/Fun_with_Submodules/SM'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (4/4), done.
hbarta@rocinante:~/Programming/Fun_with_Submodules$ tree
.
├── LICENSE
├── README.md
└── SM
    ├── LICENSE
    └── README.md

2 directories, 4 files
hbarta@rocinante:~/Programming/Fun_with_Submodules$ 
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitmodules
        new file:   SM

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

hbarta@rocinante:~/Programming/Fun_with_Submodules$ 
```

Shoot. Already messed up.

> Since the URL in the .gitmodules file is what other people will first try to clone/fetch from, make sure to use a URL that they can access if possible. For example, if you use a different URL to push to than others would to pull from, use the one that others have access to. You can overwrite this value locally with git config submodule.DbConnector.url PRIVATE_URL for your own use. When applicable, a relative URL can be helpful.

