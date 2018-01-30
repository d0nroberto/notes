### Updating Docker Images

The docker images command shows all images on the system:

<code># docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              022a68fb5f87        4 days ago          269 MB
hello-world         latest              48b5124b2768        3 weeks ago         1.84 kB
centos              centos7             67591570dd29        7 weeks ago         192 MB
coreos/apache       latest              5a3024d885c8        2 years ago         294 MB</code>

<code># docker run centos /bin/bash -c "echo 'Hello World' > /etc/motd"
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
Digest: sha256:c577af3197aacedf79c5a204cd7f493c8e07ffbce7f88f7600bf19c688c38799
Status: Downloaded newer image for centos:latest</code>

To get the container ID of the container just run:


<code># docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
78cb38674975        centos              "/bin/bash -c 'ech..."   4 seconds ago       Exited (0) 3 seconds ago                       epic_almeida
a638429d20a5        centos              "/bin/bash -c 'ech..."   2 minutes ago       Exited (0) 2 minutes ago                       musing_lewin
21c76a818b95        centos              "/bin/bash -c 'ech..."   2 minutes ago       Exited (0) 2 minutes ago                       goofy_blackwell
a1c95742b3ce        hello-world         "/hello"                 26 hours ago        Exited (0) 26 hours ago                        dreamy_bassi
86c5cddf0a90        hello-world         "/hello"                 4 days ago          Exited (0) 4 days ago                          kickass_aryabhata</code>

Now commit the changes to a new image:

<code># docker commit 78cb38674975 motd_busted
sha256:4582043192ad6bea65e4e05ff7beadf1033510df83485c91171d7ab412fcde88

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
motd_busted         latest              4582043192ad        9 seconds ago       192 MB
<none>              <none>              022a68fb5f87        4 days ago          269 MB
hello-world         latest              48b5124b2768        3 weeks ago         1.84 kB
centos              centos7             67591570dd29        7 weeks ago         192 MB
centos              latest              67591570dd29        7 weeks ago         192 MB
coreos/apache       latest              5a3024d885c8        2 years ago         294 MB

# docker history motd_busted
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
4582043192ad        4 minutes ago       /bin/bash -c echo 'Hello World' > /etc/motd     12 B     
67591570dd29        7 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0 B      
<missing>           7 weeks ago         /bin/sh -c #(nop)  LABEL name=CentOS Base ...   0 B      
<missing>           7 weeks ago         /bin/sh -c #(nop) ADD file:940c77b6724c00d...   192 MB   
<missing>           5 months ago        /bin/sh -c #(nop)  MAINTAINER https://gith...   0 B      </code>

This has added an additional (12 Byte) Layer to an image

