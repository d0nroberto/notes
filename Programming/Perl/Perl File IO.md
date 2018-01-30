# Perl File IO

There are a number of global variables that have an impact on I/O

|Var|Description|
|---|-----------|
|$\|Output Record Separator. Printed after every print(). Default undefined.|
|$,|Output Field Separator. Printed between every parameter passed to print().|
|$"|List Separator.|
|$\||Auto-flush Output. Good for network I/O. Set to 1 ($\| = 1;).|
|$/|Input Record Separator (default "\n").  Setting it to undef will read the entire file in as one record.  Set to "" uses a space as a delimiter.  "\n\n" sets it as a blank line.|
|$.|Input Line Number.|

### FILEHANDLES

A filehandle links to an open file.
Every program has 3 already opened by default on startup:

|File Handle|Description|
|-----------|-----------|
|STDIN|Standard Input|
|STDOUT|Standard Output|
|STDERR|Standard Error|

A filehandle you use should be opened and closed.

~~~~
open(FILEHANDLE, "filename");

close(FILEHANDLE);
~~~~

They return true for success and false for failure.

The default action for open is to open the file in read-only mode.

You need to include special characters with the filename to select different modes.

|Character|Description|
|---------|-----------|
|"\<filename" | Open file to read.|
|">filename" | Open file to trite (truncate first).|
|">>filename" | Open file to append.|


### Writing to a file:

This is managed through print and printf.

~~~~
print(FILEHANDLE string);
~~~~

Check to make sure filehandle is open before writing. e.g.

~~~~
unless(open(FILEHANDLE, ">>$filename")) {
 die("Cannot open $filename: $!");
}
~~~~

### Reading from a file:

~~~~
open(FILEHANDLE, "<$filename") || die("Cannot open $filename: $!");
while(&lt;FILEHANDLE&gt;){
 print($_);
}
open(FILEHANDLE);
~~~~

$! contains the OS error message.  
$_ is an inbuilt variable containing the value of the last expression.


You can log STDOUT to another destination.

Use select to choose a different filehandle.

~~~~
select FILEHANDLE;
print "Writing STDOUT to file\n";
~~~~

You can store the filehandle in a variable set by a previous select e.g.

~~~~
select FH1;
$fh1 = select FH1;
select $fh1
~~~~
