### Setting the Maximum Allowable Connections for a Mysql Database


If your application is opening many simultaneous connections to a mysql database you may be presented with the error:

~~~~

  [1040] Too many connections
  
~~~~

You can view the current connection count by issuing the "show processlist" command:

~~~~

mysql> show processlist;
+-----+--------+------------------+--------------+---------+------+----------+------------------+
| Id  | User   | Host             | db           | Command | Time | State    | Info             |
+-----+--------+------------------+--------------+---------+------+----------+------------------+
|  38 | appusr | 172.17.0.4:40020 | testdb_host0 | Sleep   |    0 |          | NULL             |
|  41 | appusr | 172.17.0.4:40026 | testdb_host0 | Sleep   |    0 |          | NULL             |
|  50 | appusr | 172.17.0.4:40044 | testdb_host0 | Sleep   |   43 |          | NULL             |
| 476 | appusr | 172.17.0.4:42206 | testdb_host0 | Sleep   |   55 |          | NULL             |
| 532 | root   | localhost        | NULL         | Query   |    0 | starting | show processlist |
+-----+--------+------------------+--------------+---------+------+----------+------------------+

~~~~


You can view the maximum connection count allowed by a mysql server by checking the variable "max_connections"

~~~~

MySQL [(none)]> show variables like "max_connections";
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
1 row in set (0.01 sec)

~~~~

To temporarily edit this to a higher value execute the following:

~~~~

MySQL [(none)]> set global max_connections = 800;
Query OK, 0 rows affected (0.00 sec)

~~~~


To permanently set this in the mysql config files in /etc/ /etc/my.cnf or /etc/mysql/conf.d/ add the variable there:


~~~~

max_connections         = 500

~~~~
