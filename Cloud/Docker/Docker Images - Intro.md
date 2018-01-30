### Docker Images

An image is a static version of a container that has been 'built' and is ready to 'run'.

You can download an image locally form a repo or build your own from a Dockerfile.

# docker pull centos:latest
latest: Pulling from library/centos
45a2e645736c: Pull complete
Digest: sha256:c577af3197aacedf79c5a204cd7f493c8e07ffbce7f88f7600bf19c688c38799
Status: Downloaded newer image for centos:latest

Also docker run can pull down the latest image to run.

# docker run -it fedora /bin/bash
Unable to find image 'fedora:latest' locally
latest: Pulling from library/fedora
0fc456f626d7: Pull complete
Digest: sha256:a99209cbb485b98d17b47be2bf990a7fbd63b4d3fa61395a313308d99a326930
Status: Downloaded newer image for fedora:latest
[root@a185641040d9 /]#

Docker hub is a public registry of images available to us.

'fedora:latest' is the latest version available.


# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              67591570dd29        7 weeks ago         192 MB
fedora              latest              a1e614f0f30e        7 weeks ago         197 MB


The images you create / download are layered images, running a base OS, an application and your updates / patches etc.

The top layer config always wins where there is a conflict.

Union mounts allow us to mount the filesystems multiple times onto the same mount point.

The topmost layer is the only one that is read / write.

An example docker pull downloading the multiple layers of an image:

<code>
# docker pull coreos/apache
Using default tag: latest
latest: Pulling from coreos/apache
a3ed95caeb02: Pull complete
5e160ca0bb5a: Extracting 45.12 MB/66.42 MB
1f92e2761bfd: Downloading 13.17 MB/26.17 MB

....

a3ed95caeb02: Pull complete
5e160ca0bb5a: Pull complete
1f92e2761bfd: Pull complete
Digest: sha256:9af520cee7bedcda564970ff790cdf2e72b6daccce8539f6b3c880ed7fc21091
Status: Downloaded newer image for coreos/apache:latest

# docker rmi coreos/apache
Untagged: coreos/apache:latest
Untagged: coreos/apache@sha256:9af520cee7bedcda564970ff790cdf2e72b6daccce8539f6b3c880ed7fc21091
Deleted: sha256:5a3024d885c81e35825971f9c9bd9045773bfafef543cdbb15fc2180aeccdd36
Deleted: sha256:ac54ebe8bf8defd1c687031da510fc32e650a4ec71f55906ef6a5d7209d617db
Deleted: sha256:108baedc5a6926d0ae87fe48454cfbb2fd51724beb282019510205cf93e789b5
Deleted: sha256:170b376f64fb30995c140276be3d71dfb256b308d86183ca3b22aa93a79ad548
Deleted: sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef</code>


