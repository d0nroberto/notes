# Cloning A Repo

This is typically how you work with GIT repos in the wild. You take a clone of a repo and then work from that source. 
The repo you clone from can be local or available over the network at a GIT URL.

Again, assuming you have viewed the <a href="http://rob.thehurleys.net/index.php/home/technical-notes/applications/git/git-installation-and-initial-repo-setup/">initial setup page</a>.


~~~
# pwd
/git_repos
# git clone website dev_website
Cloning into 'dev_website'...
done.

# ls -ltra
total 4
dr-xr-xr-x. 18 root root 4096 Nov 24 16:42 ..
drwxr-xr-x.  3 root root   51 Nov 25 12:14 website
drwxr-xr-x.  4 root root   38 Nov 25 12:21 .
drwxr-xr-x.  3 root root   51 Nov 25 12:21 dev_website
~~~