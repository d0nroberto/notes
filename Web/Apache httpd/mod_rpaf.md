# mod_rpaf


https://github.com/gnif/mod_rpaf

mod_rpaf - Reverse Proxy Add Forward Sets REMOTE_ADDR, HTTPS and HTTP_PORT to the values provided by an up stream proxy. 
Sets remote-proxy-ip-list field in r

notes table to list of proxy intermediaries. 
See the GitHub link for the full list of directives. 

~~~~

LoadModule rpaf_module modules/mod_rpaf.so

RPAF_Enable On
RPAF_ProxyIPs 127.0.0.1 172.16.0.0/12
RPAF_SetHostName On
RPAF_SetHTTPS On
RPAF_SetPort On
RPAF_ForbidIfNotProxy Off

~~~~



|Directive|Value|Comment|
|---------|:----:|:-----|
|RPAF_Enable|(On/Off)|Enable reverse proxy add forward|
|RPAF_ProxyIPs|127.0.0.1 10.0.0.0/24|What IPs & bitmasked subnets to adjust requests for.|
|RPAF_Header|X-Forwarded-For|The header to use for the real IP address.|
|RPAF_SetHostName|(On/Off)|Update vhost name so ServerName &amp; ServerAlias work|
|RPAF_SetHTTPS|(On/Off)|Set the HTTPS environment variable to the header value contained in X-HTTPS or X-Forwarded-HTTPS. For best results make sure that mod_ssl is NOT enabled.|
|RPAF_SetPort|(On/Off)|Set the server port to the header value contained in X-Port or X-Forwarded-Port.|
|RPAF_ForbidIfNotProxy|(On/Off)|Option to forbid request if not from trusted RPAF_ProxyIPs; otherwise cannot be done with Allow/Deny after remote addr substitution|
