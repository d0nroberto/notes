### Saving / Manually Moving Images

Use the docker-save and docker-load commands to move images around without using a repo.

<code># docker save -o motd_busted.tar motd_busted</code>

# docker rmi motd_busted
Untagged: motd_busted:latest
Deleted: sha256:4582043192ad6bea65e4e05ff7beadf1033510df83485c91171d7ab412fcde88
Deleted: sha256:f790deb8f53882c11cd182724e29328b7baeae0d35354130fff290825d8020ab

# docker load -i motd_busted.tar
299ecf0bdfa4: Loading layer  2.56 kB/2.56 kB
Loaded image: motd_busted:latest

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
motd_busted         latest              4582043192ad        7 minutes ago       192 MB
<none>              <none>              022a68fb5f87        4 days ago          269 MB
hello-world         latest              48b5124b2768        3 weeks ago         1.84 kB
centos              centos7             67591570dd29        7 weeks ago         192 MB
centos              latest              67591570dd29        7 weeks ago         192 MB
coreos/apache       latest              5a3024d885c8        2 years ago         294 MB
#</code>
