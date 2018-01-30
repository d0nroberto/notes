# GIT Configuration Files

Config file options are applied as they appear in the files in order of precedence from highest to lowest:


~~~
# [repo location]/.git/config
~~~

Change these with the option
~~~
# --file
~~~

~~~
# ~/.gitconfig
~~~

Change these with the option
~~~
# --global
~~~ 

~~~
# /etc/gitconfig
~~~



Change these with the option
~~~
# --system
~~~ 


e.g. setting the username / email used for your GIT commits for a repo.


~~~
# git config user.name "Rob Hurley"
# git config user.email "rhurley@example.com"
~~~


e.g. while setting the username / email used for your GIT commits # for all repos.


~~~
# git config --global user.name "Rob Hurley"
# git config --global user.email "rhurley@example.com"
~~~


# List all global options set:


~~~
# cd /
# git config -l
user.name=Rob Hurley
user.email=rhurley@example.com
~~~


#### List all the config options set for a repo:

~~~
# cd /git_repos/website/

# git config -l
user.name=Rob Hurley
user.email=rhurley@example.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true

# cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
~~~


#### To disable options:


~~~
# git config --unset --global user.email
~~~
