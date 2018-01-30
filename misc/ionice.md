<strong>ionice</strong>

Use this to reduce say IO issues from scheduled jobs.

From the man page:

NAME

ionice - set or get process I/O scheduling class and priority

SYNOPSIS

`ionice [-c class] [-n level] [-t] -p PID...`
`ionice [-c class] [-n level] [-t] command [argument...]`


A  process  can  be  in  one  of  three  scheduling classes:

-c

3       Idle

A program running with idle I/O priority will only get disk time
when no other program has asked for disk I/O for a defined grace
period.   The  impact  of  an  idle I/O process on normal system
activity should be zero.  This scheduling class does not take  a
priority  argument.  Presently, this scheduling class is permit
ted for an ordinary user (since kernel 2.6.25).

2       Best-effort

This is the effective scheduling class for any process that  has
not  asked for a specific I/O priority.  This class takes a pri
ority argument from 0-7, with a lower number being higher prior
ity.   Programs  running  at  the  same best-effort priority are
served in a round-robin fashion.


Note that before kernel 2.6.26 a process that has not asked  for
an  I/O  priority  formally uses "none" as scheduling class, but
the I/O scheduler will treat such processes as if it were in the
best-effort  class.   The  priority within the best-effort class
will be dynamically derived from  the  CPU  nice  level  of  the
process: io_priority = (cpu_nice + 20) / 5.

For  kernels  after 2.6.26 with the CFQ I/O scheduler, a process
that has not asked for an I/O priority inherits its CPU schedul
ing  class.  The I/O priority is derived from the CPU nice level
of the process (same as before kernel 2.6.26).

1       Realtime

The RT scheduling class is  given  first  access  to  the  disk,
regardless  of what else is going on in the system.  Thus the RT
class needs to be used with some care, as it  can  starve  other
processes.  As with the best-effort class, 8 priority levels are
defined denoting how big a  time  slice  a  given  process  will
receive on each scheduling window.  This scheduling class is not
permitted for an ordinary (i.e., non-root) user.


OPTIONS

`-c, --class class`

Specify the name or number of the scheduling class to use; 0 for
none, 1 for realtime, 2 for best-effort, 3 for idle.


`-n, --classdata level`

Specify  the  scheduling class data.  This only has an effect if
the class accepts an argument.  For  realtime  and  best-effort,
0-7 are valid data (priority levels).


`-p, --pid PID...`

Specify the process IDs of running processes for which to get or
set the scheduling parameters.


`-t, --ignore`

Ignore failure to set the requested priority.   If  command  was
specified,  run  it  even in case it was not possible to set the
desired scheduling priority, which can happen  due  to  insuffi
cient privileges or an old kernel version.


EXAMPLES


`# ionice -c 3 -p 89`

Sets process with PID 89 as an idle I/O process.

`# ionice -c 2 -n 0 bash`

Runs 'bash' as a best-effort program with highest priority.

`# ionice -p 89 91`

Prints the class and priority of the processes with PID 89 and 91.

`# nice -n 19 ionice -c3 /usr/sbin/logrotate /etc/logrotate.conf`
