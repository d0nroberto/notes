# Uploading to Remote Repos
Before pushing to a new master, setup the initial repo locally.

~~~
# pwd
/opt/git/chef-repo

# git add monitoring

# git commit -m "Initial Commit"
[master (root-commit) aadc7d6] Initial Commit
...

# git status
# On branch master
nothing to commit, working directory clean
~~~

On github I have setup a new project. I will push this repo to it.

~~~
# git remote add origin https://github.com/[username]/[reponame]

# git remote -v
origin  https://github.com/[username]/[reponame] (fetch)
origin  https://github.com/[username]/[reponame] (push)

# git push origin master
Username for 'https://github.com': [username]
Password for 'https://[username]@github.com':
Counting objects: 28, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (28/28), 4.98 KiB | 0 bytes/s, done.
Total 28 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/[username]/[reponame]
 * [new branch]      master -> master
 ~~~


