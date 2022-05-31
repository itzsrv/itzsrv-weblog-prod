---
title: Work, Stage and Commit 
date: 2022-03-03
description: Complete guide to Git 
tags: ['post']
draft: true
---

There is a quote, that goes back to 2006, "Data is the new oil", by UK Mathematician and architect of Tesco’s Clubcard, Clive Humby. And it has picked up steam in mainstream media when The Economist published a report titled ["The world’s most valuable resource is no longer oil, but data"](https://www.economist.com/leaders/2017/05/06/the-worlds-most-valuable-resource-is-no-longer-oil-but-data).

And when it comes to programer's world, it is not only essential to manage and store data, but also to track the changes in data. A change in program file (or code), as small as changing an and operator to or operator can totally disrupt the outcome of what it was originally supposed to. We all have faced the scenarios where that last minute change had disrupted the entire application and brought it down to its knees. In these scenarios, our beloved saviour backup/version control come to our rescue. 

A version control is a system that efficiently records changes to a file or set of files over time in an optimized way by tracking and indexing the changes, and taking point in time backups, so that you can recall specific versions later.

Git, VCS, SCM

gitbash

create versions of file - snapshot

file info in staging area

working area        staging area        commit area         --      repository

creating a repository - initiaizing

```bash
$ git status
fatal: not a git repository (or any of the parent directories): .git
$
```


```bash
$ git init
Initialized empty Git repository in /Users/macpro/git_training/first-project/.git/
```

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png
        file1
        hello.c

nothing added to commit but untracked files present (use "git add" to track)
```

```bash
$ git add hello.c 

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.c

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png
        file1


```


If you want to check the files that are present in staging area

```bash
$ git ls-files
hello.c
```

If you want to check the files that are present in commit area

```bash
$ git show
fatal: your current branch 'master' does not have any commits yet
```

Let's add one more file file1 in staging area and have a look at outputs of ls-files and status

```bash
$ git add file1

$ git ls-files
file1
hello.c

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   file1
        new file:   hello.c

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png

```

In our commit area, we don't have anything as of now

```bash
$ git show
fatal: your current branch 'master' does not have any commits yet
```

Let's add the file hello.c to commit, or in other words, create a snapshot of hello.c. So that, if our file hello.c in working area gets corrupted/deleted, we can restore it back in working area from commit area


```bash
$ git commit hello.c -m "first commit for hello.c file"
[master (root-commit) ee5dede] first commit for hello.c file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello.c
```

```bash
$ git show
commit ee5dede5faeb6b6bddcbe03705ec58b8bdf71423 (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:23:17 2022 +0530

    first commit for hello.c file

diff --git a/hello.c b/hello.c
new file mode 100644
index 0000000..e69de29
```

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file1

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png
```

For every commit, git associates it with a commit id

```bash
$ git log
commit ee5dede5faeb6b6bddcbe03705ec58b8bdf71423 (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:23:17 2022 +0530

    first commit for hello.c file

```

```bash
$ git ls-files
file1
hello.c
```

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file1

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.c

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png

```

```basg
$ git add hello.c 

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file1
        modified:   hello.c

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png

```


```bash
$ git commit hello.c -m "adding first line in hello.c"
[master 17d206f] adding first line in hello.c
 1 file changed, 1 insertion(+)
```

```bash
$ git log
commit 17d206f6406ae56f525c124c576e5f008e658fe4 (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:46:28 2022 +0530

    adding first line in hello.c

commit ee5dede5faeb6b6bddcbe03705ec58b8bdf71423
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:23:17 2022 +0530

    first commit for hello.c file
```

```bash
$ git log --oneline
17d206f (HEAD -> master) adding first line in hello.c
ee5dede first commit for hello.c file
```

```bash
$ git restore --staged file1

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png
        file1

nothing added to commit but untracked files present (use "git add" to track)

```

To commit the latest change in file1 from working area, you need to add the info after the changes again to staging area, otherwise it would refer to the previous state of file1 that you added last time

```bash
$ git add file1

$ cat >> file1
first line added in file1      
^D

$ cat file1
first line added in file1

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file1

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file1

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png


$ git add file1

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file1

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png

```

```bash
$ git commit file1 -m "commiting file1 with first line"
[master 7cd3b0d] commiting file1 with first line
 1 file changed, 1 insertion(+)
 create mode 100644 file1


$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file.mp4
        file.png

nothing added to commit but untracked files present (use "git add" to track)

$ git show
commit 7cd3b0d46be67464d6609bd3dfe5010d29af56af (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 17:09:07 2022 +0530

    commiting file1 with first line

diff --git a/file1 b/file1
new file mode 100644
index 0000000..d89814c
--- /dev/null
+++ b/file1
@@ -0,0 +1 @@
+first line added in file1

$ git log
commit 7cd3b0d46be67464d6609bd3dfe5010d29af56af (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 17:09:07 2022 +0530

    commiting file1 with first line

commit 17d206f6406ae56f525c124c576e5f008e658fe4
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:46:28 2022 +0530

    adding first line in hello.c

commit ee5dede5faeb6b6bddcbe03705ec58b8bdf71423
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 16:23:17 2022 +0530

    first commit for hello.c file


$ git show 7cd3b
commit 7cd3b0d46be67464d6609bd3dfe5010d29af56af (HEAD -> master)
Author: Saurav Singh <itzsrv@outlook.com>
Date:   Thu Mar 3 17:09:07 2022 +0530

    commiting file1 with first line

diff --git a/file1 b/file1
new file mode 100644
index 0000000..d89814c
--- /dev/null
+++ b/file1
@@ -0,0 +1 @@
+first line added in file1

```

So make it less complicated, git provides the latest commit id in form of HEAD 

```bash
$ git config --global user.name
Saurav Singh

$ git config --global user.email
itzsrv@outlook.com

$ git config --global -l
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
user.name=Saurav Singh
user.email=itzsrv@outlook.com
```

vi .gitignore

git add .gitignore



```bash
$ git ls-tree HEAD
100644 blob 3df8cdaf8cb6344edec5acd0f242283290d4b173    .gitignore
100644 blob d89814c521c38ed860e30e5095a859febded47a0    file1
100644 blob 20aeba2bad864cf6904f9caaea55f46f03ce6ac1    hello.c

$ git ls-tree HEAD --name-only
.gitignore
file1
hello.c
```

How to remove commits from commit area : Homework


RESET
    hard    soft    mixed


```bash
$ git log --oneline
91491cc (HEAD -> master) gitignore file added
dc69bce added line two and three in hello.c but staging area has only line two which should be commited
7cd3b0d commiting file1 with first line
17d206f adding first line in hello.c
ee5dede first commit for hello.c file

$ git reset 17d206f hello.c 
Unstaged changes after reset:
M       hello.c

$ git log --oneline
91491cc (HEAD -> master) gitignore file added
dc69bce added line two and three in hello.c but staging area has only line two which should be commited
7cd3b0d commiting file1 with first line
17d206f adding first line in hello.c
ee5dede first commit for hello.c file

$ ls -latr
total 24
drwxr-xr-x   4 macpro  staff  128  3 Mar 15:44 ..
-rw-r--r--   1 macpro  staff    0  3 Mar 15:59 file.mp4
-rw-r--r--   1 macpro  staff    0  3 Mar 15:59 file.png
-rw-r--r--   1 macpro  staff   26  3 Mar 17:00 file1
-rw-r--r--   1 macpro  staff   34  3 Mar 17:36 hello.c
-rw-r--r--   1 macpro  staff   18  3 Mar 17:53 .gitignore
drwxr-xr-x   8 macpro  staff  256  3 Mar 17:53 .
drwxr-xr-x  13 macpro  staff  416  3 Mar 18:13 .git

$ cat hello.c 
first line
second line
third line

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   hello.c

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.c

```

```bash
$ git checkout hello.c 
Updated 1 path from the index

$ cat hello.c 
first line
```

```bash
$ git config --global alias.hist "log --oneline"

$ git hist
91491cc (HEAD -> master) gitignore file added
dc69bce added line two and three in hello.c but staging area has only line two which should be commited
7cd3b0d commiting file1 with first line
17d206f adding first line in hello.c
ee5dede first commit for hello.c file

$ git config --global -l
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
user.name=Saurav Singh
user.email=itzsrv@outlook.com
alias.hist=log --oneline

```

remove the files from all the three areas

```bash
git rm a

git rm -r .
```
```bash

```

```bash
$ git hist
4d041b7 (HEAD -> master) commiting the third line
bd281bb commiting for second line
4e4b368 first commit

$ git cat-file -p bd28
tree 7b91b00a8107ceb5654969965b549940b2481af0
parent 4e4b3689984e90f012a4fb3231da2750232bbd5e
author Saurav Singh <itzsrv@outlook.com> 1646337125 +0530
committer Saurav Singh <itzsrv@outlook.com> 1646337125 +0530

commiting for second line
```

```bash

```

#### Session 3

Timelie -> branch (default standard name master)   
Last commit -> special variable(pointer) -> Head

GitHub Cloud   

origin(any name) --> cloud url

git push origin(where) master(which branch from local you want to push)

your local data will be pushed to cloud

master branch --> entire commits of the timeline will be pushed(v1 v2 v3)

technically they use HEAD, HEAD -> v3, technically you do almost any operation in commit area, that operation will always check where your head is.

HEAD -> v3

from HEAD they get to know about entire data. { v3 v2 v1}

by switching the head, you can control till where you want to push in cloud (HEAD -> v2)

wherever your HEAD is pointing, most of the git related commands will work



```bash
$ git branch
* master


$ git log --oneline
4d041b7 (HEAD -> master) commiting the third line
bd281bb commiting for second line
4e4b368 first commit

$ git branch dev1

$ git branch
  dev1
* master


$ git checkout dev1
Switched to branch 'dev1'


$ git branch
* dev1
  master

$ git branch -d dev1
error: Cannot delete branch 'dev1' checked out at '/Users/macpro/Docs/code/git_training/third-project'


$ git branch
* dev1
  master


$ git cheout master
git: 'cheout' is not a git command. See 'git --help'.

The most similar command is
        checkout


$ git checkout master
Switched to branch 'master'


$ git branch -d dev1
Deleted branch dev1 (was 4d041b7).
```

if the changes are done in dev1 branch, and need to merge in master from dev1
```bash
git checkout master

git switch -

git merge dev1

```
merging strategy is fast-forward



```bash

```
```bash

```

```bash

```

```bash

```
```bash

```

```bash

```

```bash

```

We can host our static sites freely using Github Hosting Service. Whether be it a simple HTML CSS page, or Modern JS Framework Pages with React, Angular or Veu, untill its a static website we can host it from Github Pages. 

As mentioned on Github about Github Pages :

>GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub, optionally runs the files through a build process, and publishes a website.

I have created an npm React project, and I am setting gh-pages into my project. For that, I need to first install gh-pages and then configure the package.json file. But before going further, we need to register one domain and configure DNS settings for our Github Servers. If you don't have any registered domain, still you can host your site under GitHub subdomain.

I have registered my domain from GoDaddy, and to configure DNS for Github servers, below configurations need to be added in DNS.

![image-in-post-gd-1.png](./image-in-post-gd-1.png)
![image-in-post-gd-2.png](./image-in-post-gd-2.png)

In modern JS Frameworks, when the build command is run, the static website with all html, css and js are created under new directory, often called **build**, **dist**, or as in my case, **public**. These directories are usually marked in gitignore and are neither tracked nor commited to master branch of our repository. We will exclusively publish this directory to our repository with new branch named gh-pages with the help of below configurations. And then in the Github settings for our repository, we will host this gh-pages branch with Github Pages.

**Setting Github Pages for our Project**

We can install gh-pages into our project using below npm command
```js
npm install gh-pages
```

**Configuring values for homepage and scripts: deploy in my package.json**

We have to add the url of our github repo in **`homepage`**, as shown below in our package.json file.

![image-in-post-homepageValue.png](./image-in-post-homepageValue.png)

And also, we have to add a **script** to deploy our build directory **`public`** to our gh-pages branch as shown below,

![image-in-post.jpg](./image-in-post-script.jpg)

If your build directory is some other directory, you can give your directory name in place of public here. 

Now lets publish our build directory to our repository's gh-pages branch

```js
npm run deploy
```
Now you can check your branches in your Github repo and you will see a gh-pages branch will your build directory contents.

![image-in-post-gh-pages.png](./image-in-post-gh-pages.png)

Select the branch gh-pages, then open Settings from top right corner and navigate to Github Pages as shown below

![image-in-post-GP.png](./image-in-post-GP.png)

Provide the registered custom domain where you want to host your website, and you are good to go!  

One advantage of hosting on Github Pages over other hosting platforms is that Github provides free SSL-Certificate for its hosted site.


