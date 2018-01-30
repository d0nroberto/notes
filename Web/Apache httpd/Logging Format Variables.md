# Apache Log Format Variables
Apache Variables to specify in a httpd.conf LogFormat directive e.g.

~~~~

    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    
    CustomLog "logs/access_log" combined

~~~~

|Variable|Description|
|------------|:------------|
|%a          |Remote IP of Client                        |
|%A          |Local IP                                   |
|%b %B       |Bytes sent (excluding header)              |
|%X          |Connection Status                          |
|%D          |Time Taken to process request in microsec. |
|%{variable}e|Environment Variable value                 |
|%f          |Filename                                   |
|%h          |Client hostname                            |
|%H          |Request Protocol                           |
|%{header}i  |Request Header Variable value              |
|%l          |Remote log name                            |
|%m          |Request Method (GET etc.)                  |
|%{label}n   |Labelled note from a module                |
|%{header}o  |Response Header Variable value             |
|%p          |Server Port                                |
|%P          |Process ID handling request                |
|%q          |Query String                               |
|%r          |First line of client request               |
|%s          |HTTP Status                                |
|%t          |Date / Time of request                     |
|%{format}t  |Date / Time of request in a specific format|
|%T          |Time Taken to serve request in secs.       |
|%u          |Remote User                                |
|%U          |URL                                        |
|%v          |ServerName                                 |
|%V          |ServerName                                 |
