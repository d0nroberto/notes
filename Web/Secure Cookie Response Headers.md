# Secure Cookies over HTTPS

You can have authentication cookies from a website you accessed via HTTPS e.g. JSESSIONID stored in a header.
Then when you move to HTTP, the cookies can be sent plaintext.

The Secure cookie attribute in the webserver response header tells the client to 
only send the cookies over HTTPS and not HTTP.

This response header attribute can be set at a number of places.

### Load Balancer

If you are deploying an F5 device, the Response Headers can be modified by an iRule.

Have a read [here](https://support.f5.com/csp/#/article/K11324) first. 

<blockquote>
The BIG-IP cookie persistence profiles do not have a feature that allows an administrator to set the secure cookie attribute for BIG-IP persistence cookies.
</blockquote>

Quoting the same document for the solution:

<blockquote>
Using an iRule to set the secure attribute for one or multiple HTTP cookies

You can use the HTTP::cookie secure iRule command to set the secure attribute for one or multiple HTTP cookies that the BIG-IP system returns to the client. In addition, your virtual server may reference a BIG-IP cookie persistence profile, which iframes an additional BIG-IP cookie in the response to the client. The following iRule adds the secure attribute for all cookies returned in the BIG-IP response:
</blockquote>

~~~~
when HTTP_RESPONSE {
    foreach mycookie [HTTP::cookie names] {
    HTTP::cookie secure $mycookie enable
   }
}
~~~~

This can cause additional load on an F5 device as it applies the iRule.

### Webserver (Apache)

Assuming mod_headers is loaded e.g.

~~~~
00-base.conf:LoadModule headers_module modules/mod_headers.so
~~~~

Use the directive:

~~~~
Header edit Set-Cookie
~~~~

e.g.

~~~~
Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
~~~~

<blockquote>edit

edit*

If this response header exists, its value is transformed according to a regular expression search-and-replace. The value argument is a regular expression, and the replacement is a replacement string, which may contain backreferences or format specifiers. The edit form will match and replace exactly once in a header value, whereas the edit* form will replace every instance of the search pattern if it appears more than once.
</blockquote>

For additional information, read [here](http://httpd.apache.org/docs/current/mod/mod_headers.html).


### Application Server (Tomcat)

The configuration base for a tomcat instance is in *$CATALINA_HOME*

Looking at *$CATALINA_HOME/conf/server.xml* & the *Connector* tag 

~~~~
<!-- A "Connector" represents an endpoint by which requests are received 

and responses are returned. Documentation at :

Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)

Java AJP  Connector: /docs/config/ajp.html

APR (HTTP/AJP) Connector: /docs/apr.html

Define a non-SSL/TLS HTTP/1.1 Connector on port 8080

-->
~~~~

~~~~
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"

maxThreads="150" SSLEnabled="true" scheme="https" secure="true"

clientAuth="false" sslProtocol="TLS" />
~~~~



Quoting from [here](http://tomcat.apache.org/tomcat-7.0-doc/security-howto.html)

<blockquote>The SSLEnabled, scheme and secure attributes may all be independently set. 
These are normally used when Tomcat is located behind a reverse proxy and the proxy is connecting to Tomcat via HTTP or HTTPS. 
They allow Tomcat to see the SSL attributes of the connections between the client and the proxy rather than the proxy and Tomcat. 
For example, the client may connect to the proxy over HTTPS but the proxy connects to Tomcat using HTTP. 
If it is necessary for Tomcat to be able to distinguish between secure and non-secure connections received by a proxy, the proxy must use separate connectors to pass secure and non-secure requests to Tomcat. 
If the proxy uses AJP then the SSL attributes of the client connection are passed via the AJP protocol and separate connectors are not needed.</blockquote>

Setting the HttpOnly option is done in the *$CATALINA_HOME/conf/context.xml* file.  
Set the *Context* tag option *useHttpOnly* to true. This is the default option so shouldn't actually need doing.

<blockquote>
useHttpOnlyShould the HttpOnly flag be set on session cookies to prevent client side script from accessing the session ID? Defaults to true.
</blockquote>


Additional reading below: 

<a href="https://tomcat.apache.org/tomcat-7.0-doc/config/context.html">https://tomcat.apache.org/tomcat-7.0-doc/config/context.html</a>  
<a href="https://tomcat.apache.org/tomcat-8.0-doc/config/context.html">https://tomcat.apache.org/tomcat-8.0-doc/config/context.html</a>


Example of the headers being set by https://github.com

~~~~
# curl -IL https://github.com -s | grep Set-Cookie
Set-Cookie: logged_in=no; domain=.github.com; path=/; expires=Sun, 07 Dec 2036 13:05:08 -0000; secure; HttpOnly
Set-Cookie: _gh_sess=eyJzZXNzaW9uX2lkIjoiYjQxZDNhZWM3YmU2YmM5MzdmZmRjZjkwY2E.........; path=/; secure; HttpOnly
~~~~


