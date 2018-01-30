# Perl String Operations

Use << to quote multi-line text.

~~~~
print <<END_OF_TEXT;
Hello World
END_OF_TEXT
~~~~

### Substrings

index() - returns the first numeric the location in a string of a substring from the left.  

rindex() - returns the first numeric the location in a string of a substring from the right.  

substr() - returns a substring of the string. You can also assign a substring to replace,  

~~~~
substr("Hello World", 0, 5) = "Goodbye";
~~~~

lc() - translate a string to lower case  

uc() - translate a string to upper case  

ucfirst() - Stranslate first character to upper case  

~~~~
$first = "robert"; $firstname = ucfirst($first);
~~~~

tr/// - translates characters. Use the =~ operator for assignment.

Additional options:

/c - compliment or invert  
/d - delete characters with no match  
/s - squash match or replace character sequences with a single character  

~~~~
tr/A-Za-z/N-ZA-Mn-za-m/; # ROT-13 (Rotate 13 places crypt)
tr/A-Za-z/N-ZA-Mn-za-m/; # ROT-13 (Rotate 13 places decrypt)
tr/ /d; # Delete Spaces
~~~~

grep expr, list; - Search list for elements matching expression and return a list of matches

~~~~
@easy_passwords = grep /changeme/, @passwords;
~~~~

split(/expression/, string, limit) - Split the input string on a specific character or expression. Returns a list.

~~~~
@fields = split(/:/, $passwd_line);
~~~~

join(delimiter, list) - Merge the input list into a single string with fields separated by delimiter. Returns a string.

~~~~
$passwd_line = join(/:/, @fields);
~~~~
