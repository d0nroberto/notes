# Perl Regular Expressions

Regular expressions are used to describe string patterns.
You can match a pattern or replace characters in a string.

The pattern used in the expression is usually encased between / /


### Matching

A single character matches directly.

A . matches any single character

A character list enclosed in [ ] matches any character in the list

e.g.
~~~~
/a/ - Matches 'a'

/[abc]/ - Matches either 'a' or 'b' or 'c'

/./ - Matches any character
~~~~

^ inverts the character or list selection e.g.

~~~~
/[^abc]/ - Matches any character except 'a', 'b' or 'c'
~~~~

Character shortcuts are used instead of providing long ranges of characters e.g.

~~~~
\d - Digits [0-9]

\D - Non-digits [^0-9]

\w - Word characters [A-Za-z0-9_]

\W - Non-word characters [^A-Za-z0-9_]

\s - Space characters e.g. '\t', '\n'

\S - Non-space characters
~~~~

You can also match multiple characters within the / / 

e.g.

~~~~
/abc/ - Matches 'abc' exactly
~~~~

You can use the OR operator in a pattern to provide multiple options

e.g.

~~~~
/abc|xyz/ - Matches 'abc' or 'xyz'
~~~~

You can also use a pattern multiplier to apply a pattern a number of times to a character.

~~~~
? - Zero or more

* - Zero or more

+ - One or more

{n,m} - From n to m occurrences

{n,} - n or more occurrences

{,m} - no more than m occurrences

{i} - exactly i occurrences
~~~~

e.g.

~~~~
/ha?t/ - Matches 'ht', 'hat'

/no*/ - Matches 'n', 'no', 'noo', 'nooo', 'noooo' etc.

/yes{5}/ - Matches 'yesssss'
~~~~

Be aware that * greedy matches - working all the way to the end and match up to the last occurrence.

To prevent this, use the * and ? together e.g.

~~~~
.*?
~~~~

You can store the text matched in an expression within brackets ().

The text matched can then be accessed later using \1 for the first set, \2 for the second set, \3 for the third and so on e.g.

~~~~
/(.).\1/ - This matches 3 character palindromes 'mum', 'bib' etc.
~~~~

The text stored in \1 etc. is only available within the expression.

You should use the variables $1, $2 etc. to access the variables outside the expression e.g.

~~~~
$word = 'bib';

$word =~ /(.).\1/;

$first_and_last_letter = $1;
~~~~

You can anchor the patterns using ^ and $

~~~~
^ - Anchors a pattern to the start of a string e.g. /^cat/

$ - Anchors a pattern to the end of a string e.g. /dog$/
~~~~

The pattern match operators used to check a variable are:

=~ - Check if a pattern matches e.g. 

~~~~
if ($animal =~ /cat/) {}
~~~~

!~ - Check if a pattern does not match e.g.

~~~~
if ($animal !~ /dog/) {}
~~~~


### Substitution

The substitution operator replaces a pattern in a string with an alternative.

The syntax is as follows:

~~~~
$variable =~ s/dog/cat/;
~~~~

It replaces only a single occurrence of the pattern to match.

You must use modifiers to change this behaviour. e.g.

/g - Every occurrence  

/i - Ignore case  

~~~~
$variable =~ s/dog/cat/ig;
~~~~

You can debug matching using the following special variables:

~~~~
$` - Text pre matching

$& - Text matched

$' - Text after match
~~~~

e.g.

~~~~
$var = "the dog barked";
$var =~ /dog/;
~~~~
~~~~
$` - 'the'
$& - 'dog'
$' - 'barked'
~~~~
