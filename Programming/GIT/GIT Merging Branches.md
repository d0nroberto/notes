# GIT Merge

Merges are used to combine commit branches contained in the same repo.

The branches merged cannot have conflicts in the files being merged e.g. the same line in the same file being edited.

Conflicts must be manually resolved before a merge can complete.

To merge a branch into another, first checkout a branch and then merge the other branch into it.

I assume you have read through the previous page about <a href="http://rob.thehurleys.net/index.php/home/technical-notes/applications/git/git-working-with-branches/">creating branches</a> etc.



### Merging branches into master

~~~
# git branch dev/v1.0
# git branch dev/v1.1
# git branch
  dev/v1.0
  dev/v1.1
* master


# git checkout dev/v1.0
~~~

Switched to branch 'dev/v1.0'

# vim dev10.c
~~~

# git add dev10.c
~~~

# git commit -m "Added dev10.c"
~~~

[dev/v1.0 28bc4bb] Added dev10.c

 1 file changed, 1 insertion(+)

 create mode 100644 dev10.c



# git checkout dev/v1.1
~~~

Switched to branch 'dev/v1.1'

# vim dev11.c
~~~

# git add dev11.c
~~~

# git commit -m "Added dev11.c"
~~~

[dev/v1.1 e1d4d9f] Added dev11.c

 1 file changed, 1 insertion(+)

 create mode 100644 dev11.c



# git branch
~~~

  dev/v1.0

* dev/v1.1

  master



# git merge dev/v1.0
~~~

Merge made by the 'recursive' strategy.

 dev10.c | 1 +

 1 file changed, 1 insertion(+)

 create mode 100644 dev10.c



# git branch
~~~

  dev/v1.0

* dev/v1.1

  master


~~~
# git ls-files
1.c
2.c
dev10.c
dev11.c

# git status
# On branch dev/v1.1
nothing to commit, working directory clean

# git log --graph --pretty=oneline --abbrev-commit
*  52f5fa4 Merge branch 'dev/v1.0' into dev/v1.1
|\
| * 28bc4bb Added dev10.c
* | e1d4d9f Added dev11.c
|/
* ad2be39 Initial Commit of 2.c
......
#
~~~

### Merging branches containing a conflict into master
~~~
# git checkout dev/v2.0
Switched to branch 'dev/v2.0'
~~~

~~~
# echo "dev/v2.0" > conflict
# git add conflict
~~~

~~~
# git  commit -m "update conflict"
[dev/v2.0 3c89b8a] update conflict
 1 file changed, 1 insertion(+)
 create mode 100644 conflict
~~~

~~~
# git checkout dev/v2.1
Switched to branch 'dev/v2.1'
# echo "dev/v2.1" > conflict
~~~

~~~
# git add conflict
# git  commit -m "update conflict"
[dev/v2.1 ca60189] update conflict
 1 file changed, 1 insertion(+)
 create mode 100644 conflict
~~~

~~~
# git checkout master
~~~

~~~
# git merge "dev/v2.0"
Updating 52f5fa4..3c89b8a
Fast-forward
 conflict | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 conflict
~~~

~~~
# git merge "dev/v2.1"
Auto-merging conflict
COFLICT (add/add): Merge conflict in conflict
Automatic merge failed; fix conflicts and then commit the result.
~~~

~~~
# cat conflict

<<<<<<< HEAD

dev/v2.0

=======

dev/v2.1

>>>>>>> dev/v2.1
~~~

To view and resolve conflicts
Use git ls-files, git diff to find the problems.
Simply edit the file to remove the conflict and commit.

~~~
# git ls-files
1.c
2.c
conflict
conflict
dev10.c
dev11.c
~~~

~~~
# git ls-files -u
100644 d84fe5dab7a0d02d90610cef579cd652b6554ab3 2       conflict
100644 5ff5c7853782608350202cd8386e475157554787 3       conflict
~~~


# cat conflict
~~~
<<<<<<< HEAD

dev/v2.0

=======

dev/v2.1

>>>>>>> dev/v2.1
~~~

~~~
# git diff
diff --cc conflict
index d84fe5d,5ff5c78..0000000
--- a/conflict
+++ b/conflict
@@@ -1,1 -1,1 +1,5 @@@
++<<<<<<< HEAD
 +dev/v2.0
++=======
+ dev/v2.1
++>>>>>>> dev/v2.1
~~~

~~~
# git diff HEAD
diff --git a/conflict b/conflict
index d84fe5d..c89ecbb 100644
--- a/conflict
+++ b/conflict
@@ -1 +1,5 @@
+<<<<<<< HEAD
 dev/v2.0
+=======
+dev/v2.1
+>>>>>>> dev/v2.1
~~~

~~~
# git diff MERGE_HEAD
diff --git a/conflict b/conflict
index 5ff5c78..c89ecbb 100644
--- a/conflict
+++ b/conflict
@@ -1 +1,5 @@
+<<<<<<< HEAD
+dev/v2.0
+=======
 dev/v2.1
+>>>>>>> dev/v2.1
~~~

~~~
# vim conflict
~~~


~~~
# git diff
diff --cc conflict
index d84fe5d,5ff5c78..0000000
--- a/conflict
+++ b/conflict
~~~

~~~
# git commit -a
[master 93f4a56] Merge branch 'dev/v2.1'
# git branch
  dev/v2.0
  dev/v2.1
* master
~~~

~~~
# git diff
~~~

~~~
# cat conflict
dev/v2.1
~~~

~~~
# git branch -d "dev/v2.0"
Deleted branch dev/v2.0 (was 3c89b8a).
# git branch -d "dev/v2.1"
Deleted branch dev/v2.1 (was ca60189).
~~~


~~~
# git ls-files
1.c
2.c
conflict
dev10.c
dev11.c
~~~

~~~
# git log --graph
*   commit 93f4a5691736ac30eba8ecee3e65df9b746779e5
|\  Merge: 3c89b8a ca60189
| | Author: Rob Hurley <rhurley@example.com>
| | Date:   Thu Dec 15 13:52:30 2016 +0000
| |
| |     Merge branch 'dev/v2.1'
| |
| |     Conflicts:
| |             conflict
| |
| * commit ca60189629ad2e34d92f98522db22d7cbf67386c
| | Author: Rob Hurley <rhurley@example.com>
| | Date:   Thu Dec 15 13:42:54 2016 +0000
| |
| |     update conflict
| |
* | commit 3c89b8a03a867c7fa0c6734efe082de63edc10d7
|/  Author: Rob Hurley <rhurley@example.com>
|   Date:   Thu Dec 15 13:42:34 2016 +0000
|
|       update conflict
|
*   commit 52f5fa439ccbfac595f6ad37a050d5c1cb52f842
~~~
