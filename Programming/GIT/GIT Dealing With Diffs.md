# git-diff - Show changes between commits, commit and working tree, etc.

Show changes between the working tree and the index or a tree, changes between the index and a tree, changes between two trees, or changes between two files on disk.

~~~
# git diff
~~~
 - difference between your working directory and your index (git add). Shows what can be staged in your next commit.


~~~
# git diff [commit]
~~~
 - difference between your working directory and a commit e.g. HEAD.


~~~
# git diff --cached [commit]
~~~
 - difference between your index (git add) and a commit e.g. HEAD.


~~~
# git diff [commit] [commit]
~~~
 - difference between two commits.



### GIT Diff options


~~~
# --ignore-all-space
~~~
 - Ignore whitespace in comparison

~~~
# --color
~~~
 - show diffs in colour

~~~
# --stat
~~~
 - Generate a diffstat

~~~
# --dirstat
~~~
 - Output the distribution of relative amount of changes for each sub-directory

~~~
# -S<string>
~~~
 - Look for string in diffs


### Examples


First up setup an empty repo:


~~~
# mkdir codebase
# cd codebase/
# git init

Initialized empty Git repository in /git_repos/codebase/.git/

# echo "1111" > file1.txt
# echo "2222" > file2.txt
# git add file1.txt file2.txt
# git commit -m "Added file1.txt and file2.txt"
[master (root-commit) 5e5216d] Added file1.txt and file2.txt
 2 files changed, 2 insertions(+)
 create mode 100644 file1.txt
 create mode 100644 file2.txt
 ~~~

And edit the file.

~~~
# echo "AAAA" > file1.txt
~~~


#### 1. difference between your working directory and your index


~~~
# git diff
diff --git a/file1.txt b/file1.txt
index 5f2f16b..b194361 100644
--- a/file1.txt
+++ b/file1.txt
@@ -1 +1 @@
-1111
+AAAA
~~~

#### 2. difference between your working directory and a commit e.g. HEAD.


~~~
# git diff HEAD
diff --git a/file1.txt b/file1.txt
index 5f2f16b..b194361 100644
--- a/file1.txt
+++ b/file1.txt
@@ -1 +1 @@
-1111
+AAAA
~~~


#### 3. difference between your index (git add) and a commit e.g. HEAD.


~~~
# git diff --cached

#
~~~

Now carry out some updates so we have more than one commit to compare.

~~~
# git add file1.txt
# git commit -m "Updated file1.txt"
[master 7bda11f] Updated file1.txt
 1 file changed, 1 insertion(+), 1 deletion(-)
# echo "BBBB" > file1.txt
# git add file1.txt
# git commit -m "Updated file1.txt again"
[master 87d8cf4] Updated file1.txt again
 1 file changed, 1 insertion(+), 1 deletion(-)
 ~~~


#### 4. difference between two commits.


~~~
# git diff HEAD^ HEAD
diff --git a/file1.txt b/file1.txt
index b194361..a07e243 100644
--- a/file1.txt
+++ b/file1.txt
@@ -1 +1 @@
-AAAA
+BBBB
~~~


~~~
# echo "3333" > file2.txt

# git add file2.txt

# git commit -m "Updated file2.txt"

[master 2017254] Updated file2.txt
~~~


~~~
# git diff HEAD~2 HEAD
diff --git a/file1.txt b/file1.txt
index b194361..a07e243 100644
--- a/file1.txt
+++ b/file1.txt
@@ -1 +1 @@
-AAAA
+BBBB
diff --git a/file2.txt b/file2.txt
index c7dc989..3bd9bce 100644
--- a/file2.txt
+++ b/file2.txt
@@ -1 +1 @@
-2222
+3333
~~~


~~~
# git diff --stat master~2 master
 file1.txt | 2 +-
 file2.txt | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)
~~~


~~~
# git diff -S"2222" master~2 master
diff --git a/file2.txt b/file2.txt
index c7dc989..3bd9bce 100644
--- a/file2.txt
+++ b/file2.txt
@@ -1 +1 @@
-2222
+3333
~~~



