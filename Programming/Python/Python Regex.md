# Python Regular Expressions

Import the module 're' into your scripts

~~~~

import re

~~~~


You can create a 'Regex' object by passing a regex string to the re.compile() function.

~~~~

dateFormat = re.compile(r'\d\d/\d\d/\d\d\d\d')

~~~~

You can pass additional options to the compile function e.g. re.DOTALL, re.IGNORECASE

You search a string for a match using the Regex.search() function
The search function finds the FIRST OCCURENCE of a match returns a Match object if it finds the string or 'None' if there is no match.
The March.group() function returns the string that matched the pattern.

~~~~

dob = dateFormat.search('My DOB is 07/11/1977')
if (dob != None):
	print('DOB Found : ' + dob.group())
else:
	print('DOB Not Found')

~~~~


You can include multiple strings in a regex using brackets to group the strings.
You can then pass an integer to the group function to access each group.

~~~~

dateFormat = re.compile(r'(\d\d)/(\d\d)/(\d\d\d\d)')
dob = dateFormat.search('My DOB is 07/11/1977')
print('Day Found?: ' + dob.group(1))
print('Month Found?: ' + dob.group(2))
print('Year Found?: ' + dob.group(3))

~~~~


The 'Match.groups()' function returns all groups

~~~~

dob.groups()
('07', '11', '1977')
day, month, year = dob.groups()
Print("Day: " + day)

~~~~


The pipe character | passwd to a regex causes the search method to search for either string e.g.

('Red Card|Yellow Card')

? - Match Single Char
* - Match zero or more
+ - Match one or more
{NUM} - Match preceeding regex NUM times e.g. B(o){14}
. - Match any single character


The search function finds the FIRST OCCURENCE of a match.
The Match.findall() function returns ALL OCCURRENCES


The Regex.sub() function can be used to carry out a find an replace of matching strings.

~~~~

>>> import re
>>> regex = re.compile(r'Roberto', re.IGNORECASE)
>>> regex.sub('Bob', 'My name is roberto')
'My name is Bob'
>>>

~~~~

