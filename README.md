# Fun_with_Submodules

Play with git submodules.

## 2025-03-19 Motivation

I tried using git submodules on a *real* and quickly learned I need to learn a bit more about git submodules.

* <https://git-scm.com/book/en/v2/Git-Tools-Submodules> The docs.
* <https://github.com/HankB/SM> A submodule

## 2025-03-19 module + submodule

Adding a submodule to this project and playing with it.

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

## 2025-03-20 another host

```text
hbarta@olive:~/Programming$ git clone --recurse-submodules git@github.com:HankB/Fun_with_Submodules.git
Cloning into 'Fun_with_Submodules'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 22 (delta 6), reused 16 (delta 4), pack-reused 0 (from 0)
Receiving objects: 100% (22/22), 4.86 KiB | 4.86 MiB/s, done.
Resolving deltas: 100% (6/6), done.
Submodule 'SM' (https://github.com/HankB/SM.git) registered for path 'SM'
Cloning into '/home/hbarta/Programming/Fun_with_Submodules/SM'...
remote: Enumerating objects: 4, done.        
remote: Counting objects: 100% (4/4), done.        
remote: Compressing objects: 100% (4/4), done.        
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)        
Receiving objects: 100% (4/4), done.
Submodule path 'SM': checked out '112e4d064a390929cceef92c18a990bff8abd16d'
hbarta@olive:~/Programming$ cd Fun_with_Submodules
hbarta@olive:~/Programming/Fun_with_Submodules$ tree
.
├── LICENSE
├── README.md
└── SM
    ├── LICENSE
    └── README.md

2 directories, 4 files
hbarta@olive:~/Programming/Fun_with_Submodules$ code -n .
hbarta@olive:~/Programming/Fun_with_Submodules$ 
```

Now to "fix" the URL so I can edit and (easily) push `SM`.

```text
barta@olive:~/Programming/Fun_with_Submodules$ cd SM
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git remote -v
origin  https://github.com/HankB/SM.git (fetch)
origin  https://github.com/HankB/SM.git (push)
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git config submodule.SM.url git@github.com:HankB/SM.git
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git remote -v
origin  https://github.com/HankB/SM.git (fetch)
origin  https://github.com/HankB/SM.git (push)
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ cd ..
hbarta@olive:~/Programming/Fun_with_Submodules$ git config submodule.SM.url git@github.com:HankB/SM.git
hbarta@olive:~/Programming/Fun_with_Submodules$ cd SM
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git remote -v
origin  https://github.com/HankB/SM.git (fetch)
origin  https://github.com/HankB/SM.git (push)
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ cat ../.git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[submodule]
        active = .
[remote "origin"]
        url = git@github.com:HankB/Fun_with_Submodules.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
        vscode-merge-base = origin/main
[submodule "SM"]
        url = git@github.com:HankB/SM.git
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ 
```

We'll see when I try to commit `SM`. Add a blank line between the page title and first line in `SM/README.md`. Commit (and read the section aboiut working in the submodule before pushing.)

```text
git config status.submodulesummary 1
```
## 2025-03-20 Working on a Submodule

> Git would get the changes and update the files in the subdirectory but will leave the sub-repository in what’s called a “detached HEAD” state. This means that there is no local working branch (like master, for example) tracking changes. With no working branch tracking changes, that means even if you commit changes to the submodule, *those changes will quite possibly be lost* the next time you run git submodule update.

I don't like the sound of that. Reading on...

```text
hbarta@olive:~/Programming/Fun_with_Submodules$ cd SM
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git branch
* (HEAD detached from 112e4d0)
  main
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git checkout main
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  376af78 formatting

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 376af78

Switched to branch 'main'
Your branch is up to date with 'origin/main'.
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git branch
* main
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git merge 376af78
Updating 112e4d0..376af78
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ 
```

### prepare to push

```text
hbarta@olive:~/Programming/Fun_with_Submodules$ git commit -m "another host, work in submodule"
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        modified:   SM (new commits)

no changes added to commit (use "git add" and/or "git commit -a")
hbarta@olive:~/Programming/Fun_with_Submodules$ git commit -m "another host, work in submodule" README.md
[main a1bbef9] another host, work in submodule
 1 file changed, 114 insertions(+)
hbarta@olive:~/Programming/Fun_with_Submodules$ git submodule update --remote --merge
Already up to date.
Submodule path 'SM': merged in '112e4d064a390929cceef92c18a990bff8abd16d'
hbarta@olive:~/Programming/Fun_with_Submodules$ git diff
diff --git a/README.md b/README.md
index 306a956..d2a6692 100644
--- a/README.md
+++ b/README.md
@@ -260,3 +260,5 @@ Fast-forward
 hbarta@olive:~/Programming/Fun_with_Submodules/SM$ 
 ```
 
+### prepare to push
+
diff --git a/SM b/SM
index 112e4d0..376af78 160000
--- a/SM
+++ b/SM
@@ -1 +1 @@
-Subproject commit 112e4d064a390929cceef92c18a990bff8abd16d
+Subproject commit 376af7875f1d0bfd91488be61f441bfad1600b06
hbarta@olive:~/Programming/Fun_with_Submodules$ git submodule update --remote --merge
Already up to date.
Submodule path 'SM': merged in '112e4d064a390929cceef92c18a990bff8abd16d'
hbarta@olive:~/Programming/Fun_with_Submodules$ git diff
diff --git a/README.md b/README.md
index 306a956..d2a6692 100644
--- a/README.md
+++ b/README.md
@@ -260,3 +260,5 @@ Fast-forward
 hbarta@olive:~/Programming/Fun_with_Submodules/SM$ 
 ```
 
+### prepare to push
+
diff --git a/SM b/SM
index 112e4d0..376af78 160000
--- a/SM
+++ b/SM
@@ -1 +1 @@
-Subproject commit 112e4d064a390929cceef92c18a990bff8abd16d
+Subproject commit 376af7875f1d0bfd91488be61f441bfad1600b06
hbarta@olive:~/Programming/Fun_with_Submodules$ git push --recurse-submodules=check
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.67 KiB | 1.67 MiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:HankB/Fun_with_Submodules.git
   882f845..a1bbef9  main -> main
hbarta@olive:~/Programming/Fun_with_Submodules$ cat SM/README.md 
# SM

Submodule (for https://github.com/HankB/Fun_with_Submodules)
hbarta@olive:~/Programming/Fun_with_Submodules$ git push --recurse-submodules=on-demand
Everything up-to-date
hbarta@olive:~/Programming/Fun_with_Submodules$ cd SM
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git push
Username for 'https://github.com': ^C
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ cd ..
hbarta@olive:~/Programming/Fun_with_Submodules$ git push --recurse-submodules=on-demand
Everything up-to-date
hbarta@olive:~/Programming/Fun_with_Submodules$ 
```

Checking the Github repo, I see that `README.md` is ccurrent but not `SM/README.md` Can't push from `SM` w/out providing a password. :-/ This works - Yay!

```text
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ git push git@github.com:HankB/SM.git
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 330 bytes | 330.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:HankB/SM.git
   112e4d0..376af78  main -> main
hbarta@olive:~/Programming/Fun_with_Submodules/SM$ 
```

Seems like a lot to remember here. I'm going to repeat this and use repos `MainM` and `SubM` to make sure I have it right. (Ahh... What fun!) See <https://github.com/HankB/MainM> and follow along there.

No. I thought the submodule had been pushed but if it was, it has now reverted and I can't see how to fix this. It's maddening and I'm just wasting time now. I'm not going to use submodules for the intended project.
