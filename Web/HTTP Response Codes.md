# HTTP Response Codes


### 100s - Informational

|Code|Description|
|----|-----------|
|100|Continue|
|101|Switching Protocol|
|102|Processing|


### 200s - Successful

|Code|Description|
|----|-----------|
|200|OK|
|201|Created|
|202|Accepted|
|203|Nonauthoritative Information|
|204|No Content|
|205|Reset Content|
|206|Partial Content.  Multiple responses to request broken into parts.  Request must include the Range header while response must include the Content-Range header indicating the range|


### 300s - Server Redirects

|Code|Description|
|----|-----------|
|300|Multiple Choices|
|301|Moved Permanently|
|302|Moved Temporarily|
|303|See Other|
|304|Not Modified|
|305|Use Proxy|
|307|Temporary Redirect|


### 400s - Client Errors

|Code|Description|
|----|-----------|
|400|Bad Request.  The webserver didn't understand the request|
|401|Unauthorised.  Failed to authenticate request.  Need to send 'WWW-Authenticate' header with request.|
403|Forbidden|
404|Not Found|
405|Method Not Allowed|
406|Not Acceptable|
407|Proxy Authentication Required|
408|Request Timeout.  Client didn't send a request to the server within time.|
409|Conflict|
410|Gone|
411|Length Required.  Server wants the Content-Length header sent with the request.|
412|Precondition Failed|
413|Request Entity Too Large|
414|Request URI Too Large|
415|Unsupported Media Type|
416|Range Not Satisfied|
417|Exception Failed|
422|Unprocessable Entity|
423|Locked|
424|Failed Dependency|


### 500s - Server Errors

|Code|Description|
|----|-----------|
|500|Internal Server Error|
|501|Not Implemented|Server Doesn't understand the request method sent.|
|502|Bad Gateway|Bad response from application servers behind the webserver.|
|503|Service Unavailable.  The webserver cannot handle the request now. May be too busy.|
|504|Gateway Timeout.  No response from application servers behind the webserver.|
|505|HTTP Version Not Supported|
|506|Variant also negotiates...|
|507|Insufficient Storage|
|510|Not Extended|

