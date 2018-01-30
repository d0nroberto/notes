# Perl IO


The print function writes a list of comma separated variables to a destination.

The default destination is STDOUT e.g.

~~~~
print($var1, $var2, $var3);

print("test\n");
~~~~


The printf function provides formatted printing.

~~~~
printf("format", $var1, $var2, $var3);

%s - String
%d - Decimal 
%f - Float
%c - Char
%e - Exp
%o - Octal
%x - Hex
%% - %
~~~~

e.g.

~~~~
printf("%10s $14.4f %e\n", $id, $val, $cnt);

print($format, $name, $address);
~~~~

The <> operator reads in data from a file handle.  
The default file handle is STDIN. It reads one line at a time.  
By default this will contain the line delimiter "\n". Use chomp() to remove this.  

e.g.

~~~~
$input = <STDIN>;

chomp($input);

print($input);
~~~~

Variable $/ defines the delimiter character used on a platform.
