# Lists & Arrays



A perl list is a comma separated group of <a href="http://rob.thehurleys.net/index.php/technical-notes/programming/programming-perl/perl-scalar-variables/">scalars</a> e.g.



<code>(item1, item2, item3)

(1,2,3,4,5)

('blue', 'green', 'red')

</code>

You can specify a range in list creation e.g.

<code>

(1..5)

("a".."z")

</code>



While an array is a variable that contains an indexed list of scalars.

The array variable is prefixed with an @ symbol.

The variable has a different namespace to scalars, so $val and @val are different variables.

Array indexes are circular, so index [-1] is the last element.



Examples:



<code>@count = (1,2,3,4);

@nums = @count;

@some_nums = $count[0,2,3];

$how_many_elements = @nums;

$last_num = $#nums;

$second_num = $nums[1];</code>



Reverse the order of elements in a list:



<code>@array = reverse(@array);</code>



Sort list elements in text order:



<code>sort(@elements);</code>



Remove the end element of an array



<code>chop(@array); // Remove last element

chomp(@array); // Remove end of list marker.</code>





Adding or removing elements at the end of an array (LIFO Functions).



<code>push(@num, 23); // Append number 23

$number = pop(@num); // Remove the last element</code>



Adding or removing elements at the beginning of an array (FIFO Functions).



<code>unshift(@num, 99); // Insert number 99 to beginning

$number = shift(@num); // Remove first element</code>
