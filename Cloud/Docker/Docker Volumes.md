### Docker Volumes

Docker volumes are used to store data outside the docker container.
That way if a docker container is deleted there is persistent storage available for the next instance of the container.

Use the '-v' option to the docker run command to specify a volume to use.

<code># docker run -d -v /usr/share/nginx/html webserver01</code>
	
This will create a folder under /var/lib/docker/…….

Another container can then access the same volume with --volumes-from[container_name]

<code># docker run -d --volumes-from=webserver01 webserver02</code>

To mount a folder from the host into a container, again use the '-v' option -v [source_folder]:[/dest_folder]
e.g.

<code># docker run -d -v /html:/usr/share/nginx/html webserver03</code>

