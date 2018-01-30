#### Patching Source Code With diff and patch

A simple way to patch the source of a project across multiple hosts is by using the diff and patch commands.

The 'diff' can be used to track differences between an original file and the modified code.

The 'patch' can then be deployed into the source path on any hosts that need the patch.

#### Creating The Patch

Create a working copy of the source you want to edit.
Complete all updates to the working copy.
Then simply use the 'diff' command to capture the differences.

~~~~
cd /usr/share
cp -rp codebase codebase.v10.1

vim codebase.v10.1/index.php
vim codebase.v10.1/login.php
vim codebase.v10.1/users.php

diff -Nua codebase/index.php codebase.v10.1/index.php > v10.1.patch
diff -Nua codebase/login.php codebase.v10.1/login.php > v10.1.patch
diff -Nua codebase/users.php codebase.v10.1/users.php > v10.1.patch
~~~~

Use the '-r' option to recursively traverse the code looking for diffs e.g.

~~~~
diff -Nua codebase codebase.v10.1 > v10.1.patch
~~~~


#### Deploying The Patch

The patch command carried out the updates to the source tree.

~~~~
cd /usr/share/codebase
patch -p1 < v10.1.patch
~~~~

The -p option specifies how many slashes to remove from the path of the files in the patch file.
e.g. '-p1' removes the first slash so 'codebase.v10.1/users.php' is interpreted as 'users.php'.

