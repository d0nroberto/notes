# Apache level Redirection with mod_rewrite

RewriteMap defines an external location for storing rewrites outside apache config.
The relevant module is called mod_rewrite & is usually loaded by default e.g. in /etc/httd/conf/httpd.conf:

~~~~
LoadModule rewrite_module modules/mod_rewrite.so
~~~~

Once the module is loaded the rewrite directives are available to you.


Syntax:

~~~~
RewriteMap MapName MapType:MapSource
~~~~

Arguments passed to the map for lookups by RewriteRule or RewriteRule.

~~~~
${ MapName : LookupKey }
${ MapName : LookupKey | DefaultValue }
~~~~

MapName = Name of map to do lookup in  
LookupKey = Value to search map for  
DefaultValue = If no match found, use this value  

e.g.

~~~~
RewriteMap examplemap "txt:/path/to/file/map.txt"
RewriteRule "^/ex/(.*)" "${examplemap:$1}"
~~~~

When MapType is set to txt a flat file is used
The format of the file is:

~~~~
# Comment line
MatchingKey SubstValue
MatchingKey SubstValue # comment
~~~~

For example, we can use a mapfile to translate product names to product IDs:

~~~~
RewriteMap product2id "txt:/etc/apache2/productmap.txt"
RewriteRule "^/product/(.*)" "/prods.php?id=${product2id:$1|NOTFOUND}" [PT]
~~~~

~~~~
## productmap.txt - Product to ID map file
television 993
stereo 198
fishingrod 043
basketball 418
telephone 328
~~~~

Thus, when http://example.com/product/television is requested, the RewriteRule is applied, and the request is internally mapped to /prods.php?id=993.

When MapType is set to dbm an indexed dbm database is used.

The type can be sdbm, gdbm, ndbm or db.  
However, it is recommended that you just use the httxt2dbm utility that is provided with Apache.

First create a text map file as described in the txt section. 

Then run httxt2dbm:

~~~~
$ httxt2dbm -i mapfile.txt -o mapfile.map
~~~~

You can then reference the resulting file in your RewriteMap directive:

~~~~
RewriteMap mapname "dbm:/etc/apache/mapfile.map" 
~~~~
