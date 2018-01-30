# Using curl in your PHP scripts

Before you begin, check if you have curl support in your build of php.

~~~~
# php --info | grep -i curl
Configure Command =>  '...... '--with-curl' .....
curl
cURL support => enabled
~~~~

If not, you may need to enable curl in your php.ini

~~~~
# pwd
/etc/php.d
# cat curl.ini
; Enable curl extension module
extension=curl.so
~~~~


First initialize a cURL session using:

~~~~
curl_init()
~~~~

Then you can set all your options using:

~~~~
curl_setopt()
~~~~

Then you can execute the session with:

~~~~
curl_exec()
~~~~

And then you finish off your session using:

~~~~
curl_close()
~~~~

Here's a simple example to follow:

~~~~

<?php

$output_dir = "/var/tmp/";
$ch = curl_init("http://example.com&mode=json&APIID=asd777asd78ag04545gw2266mhj78");
$fp = fopen($output_dir . "test.json", "w");

curl_setopt($ch, CURLOPT_FILE, $fp); // File to write output to
curl_setopt($ch, CURLOPT_HEADER, 0); // Include or suppress header
curl_exec($ch);

if(curl_errno($ch)){
 echo "Error: " . curl_error($ch);
 unlink($output_dir . test.json");
}

curl_close($ch);
fclose($fp);
?>
~~~~

For curl_setopt options, see [here](http://php.net/manual/en/function.curl-setopt.php)

Curl Functions:

|Function|Description|
|--------|-----------|
|curl_close|Close a cURL session|
|curl_copy_handle|Copy a cURL handle along with all of its preferences|
|curl_errno|Return the last error number|
|curl_error|Return a string containing the last error for the current session|
|curl_escape|URL encodes the given string|
|curl_exec|Perform a cURL session|
|curl_file_create|Create a CURLFile object|
|curl_getinfo|Get information regarding a specific transfer|
|curl_init|Initialize a cURL session|
|curl_multi_add_handle|Add a normal cURL handle to a cURL multi handle|
|curl_multi_close|Close a set of cURL handles|
|curl_multi_exec|Run the sub-connections of the current cURL handle|
|curl_multi_getcontent|Return the content of a cURL handle if CURLOPT_RETURNTRANSFER is set|
|curl_multi_info_read|Get information about the current transfers|
|curl_multi_init|Returns a new cURL multi handle|
|curl_multi_remove_handle|Remove a multi handle from a set of cURL handles|
|curl_multi_select|Wait for activity on any curl_multi connection|
|curl_multi_setopt|Set an option for the cURL multi handle|
|curl_multi_strerror|Return string describing error code|
|curl_pause|Pause and unpause a connection|
|curl_reset|Reset all options of a libcurl session handle|
|curl_setopt_array|Set multiple options for a cURL transfer|
|curl_setopt|Set an option for a cURL transfer|
|curl_share_close|Close a cURL share handle|
|curl_share_init|Initialize a cURL share handle|
|curl_share_setopt|Set an option for a cURL share handle.|
|curl_strerror|Return string describing the given error code|
|curl_unescape|Decodes the given URL encoded string|
|curl_version|Gets cURL version information|
