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

Try to "unhitch" the module and reconnect it using the public `pull` URL.

```text
git rm SM
rm -rf .git/modules/SM
git config remove-section submodule.SM
```

```text
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git rm SM
error: the following file has changes staged in the index:
    SM
(use --cached to keep the file, or -f to force removal)
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git rm -f SM
rm 'SM'
hbarta@rocinante:~/Programming/Fun_with_Submodules$ rm -rf .git/modules/SM
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git config remove-section submodule.SM
hbarta@rocinante:~/Programming/Fun_with_Submodules$ 
```

Let's try this again.

```text
git submodule add https://github.com/HankB/SM.git
git config submodule.SM.url git@github.com:HankB/SM.git
git diff --cached SM
git diff --cached --submodule
git commit -am 'Add SM module'
git submodule update --remote SM
git status
git push
```

```text
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git submodule add https://github.com/HankB/SM.git
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
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git config submodule.SM.url git@github.com:HankB/SM.git
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git diff --cached SM
diff --git a/SM b/SM
new file mode 160000
index 0000000..112e4d0
--- /dev/null
+++ b/SM
@@ -0,0 +1 @@
+Subproject commit 112e4d064a390929cceef92c18a990bff8abd16d
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git diff --cached --submodule
diff --git a/.gitmodules b/.gitmodules
index e69de29..9464223 100644
--- a/.gitmodules
+++ b/.gitmodules
@@ -0,0 +1,3 @@
+[submodule "SM"]
+       path = SM
+       url = https://github.com/HankB/SM.git
Submodule SM 0000000...112e4d0 (new submodule)
hbarta@rocinante:~/Programming/Fun_with_Submodules$ 
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git submodule update --remote SM
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
hbarta@rocinante:~/Programming/Fun_with_Submodules$ git push
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 1006 bytes | 1006.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:HankB/Fun_with_Submodules.git
   019af42..42f838c  main -> main
hbarta@rocinante:~/Programming/Fun_with_Submodules$ 
```

Seems good.
