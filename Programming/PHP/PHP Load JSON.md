# How to load a json file into an array in PHP

Use the following functions:

*file_get_contents* to load the file into a string

*json_decode* to parse the string into array structure

e.g.

~~~~
<?php

$string = file_get_contents("/home/user01/test.json");
$json_a = json_decode($string, true);

var_dump($json_a);

foreach ($json_a as $person_name => $person_a) {
    echo $person_a['status'];
}

?>
~~~~

See the following for more detail:

[file_get_contents](http://php.net/manual/en/function.file-get-contents.php)

~~~~
string file_get_contents ( string $filename [, bool $use_include_path = false [, resource $context [, int $offset = 0 [, int $maxlen ]]]] )
~~~~


[json_decode](http://php.net/manual/en/function.json-decode.php)

~~~~
mixed json_decode ( string $json [, bool $assoc = false [, int $depth = 512 [, int $options = 0 ]]] )
~~~~
