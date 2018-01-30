### Recovering a Lost mysql root Password

If the mysql root user password is lost you need to restart the server using the '--init-file' flag.
When the server restarts, it will read and execute the contents of the file.

First stop the running instance using systemd etc.

Next create the init file with contents similar to:

~~~~

UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
FLUSH PRIVILEGES;

~~~~

Now execute mysqld with the init file parameter.
Make sure the mysqld command is executed with the same user account that runs the service normally e.. 'mysql' user
or specify the '--user' option also

~~~~

# mysqld --init-file=/tmp/password_reset.sql

~~~~
