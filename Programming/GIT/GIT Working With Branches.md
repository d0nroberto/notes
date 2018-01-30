# Working With Branches

Branches in GIT allow you to separate your development from a main project 
and then merge in your changes to the overall project at a later date.


### With Branches:

You can create different versions of a project e.g. 
~~~
Rel_2016_10
~~~
, 
~~~
Rel_2016_11
~~~


Have different stages of development working from the same base e.g. 
~~~
development
~~~
, 
~~~
production
~~~


### Branch Naming:

The default branch of a project is the
~~~
master
~~~
branch. 

You will define a new name for any branches from this you create.

The branch you are working on is known as the 
~~~
current
~~~
branch.

The most recent commit of a branch is known as the 
~~~
head
~~~
of the branch.


### Branch Commands:

1. Creating a Branch

~~~
# git branch development/Release-2017-0
1~~~

Now change to the branch so you can work on it.

~~~
# git checkout development/Release-2017-01
~~~

The two commands can also be accomplished with:

~~~
# git checkout -b development/Release-2017-01
~~~


2. Displaying branches

~~~
# git branch
* development/Release-2017-01
  master
~~~

The 
~~~
*
~~~
shows the current checked-out branch. 


~~~
# git show-branch
* [development/Release-2017-01] Initial Commit of 2.c
 ! [master] Fix Bug
--
*  [development/Release-2017-01] Initial Commit of 2.c
*+ [master] Fix Bug
~~~


3. Working on a branch

Here I switch to a dev branch of code, add a file etc. 
and switch between the dev and master branches to show the file difference.

~~~
# git checkout development/Release-2017-01
Switched to branch 'development/Release-2017-01'
~~~

~~~
# vim 2.c
# git add 2.c
# git commit -m "Initial Commit of 2.c"
[development/Release-2017-01 ad2be39] Initial Commit of 2.c
 1 file changed, 5 insertions(+)
 create mode 100644 2.c
~~~


~~~
# git ls-files
1.c
2.c
~~~

~~~
# git branch
* development/Release-2017-01
  master
~~~

~~~
# git checkout master
Switched to branch 'master'
# git ls-files
1.c
~~~

~~~
# git checkout development/Release-2017-01
Switched to branch 'development/Release-2017-01'
~~~

~~~
# git ls-files
1.c
2.c
~~~

4. Deleting a branch

The command '~~~git branch -d~~' deletes a branch that is ready for deletion.

~~~
# git branch
* development/Release-2017-01
  master
~~~

~~~
# git branch -d development/Release-2017-01
error: Cannot delete the branch 'development/Release-2017-01' which you are currently on.
~~~

~~~
# git checkout master
Switched to branch 'master'
~~~

~~~
# git branch
  development/Release-2017-01
* master
~~~

~~~
# git branch -d development/Release-2017-01

error: The branch 'development/Release-2017-01' is not fully merged.

If you are sure you want to delete it, run 'git branch -D development/Release-2017-01'.
~~~



So we cannot simply delete a branch that we have checked out or have un-merged changes.


5. Merging and deleting a branch

~~~
# git merge development/Release-2017-01
Updating 0754825..ad2be39
Fast-forward
 2.c | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 2.c
~~~

~~~
# git checkout master
Already on 'master'
~~~

~~~
# git ls-files
1.c
2.c
~~~

~~~
# git branch -d development/Release-2017-01
Deleted branch development/Release-2017-01 (was ad2be39).
~~~
