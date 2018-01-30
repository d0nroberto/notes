You can install & run the chef-client locally on a system without access to a chef server.


First to install the client. Find chef client package [here](https://downloads.chef.io/chef-client/):


For RHEL based systems: 

~~~~
# yum install https://packages.chef.io/stable/el/7/chef-12.11.18-1.el7.x86_64.rpm

Dependencies Resolved

================================================================================
 Package  Arch       Version              Repository                       Size
================================================================================

Installing:

 chef     x86_64     12.11.18-1.el7       /chef-12.11.18-1.el7.x86_64     155 M

Transaction Summary

================================================================================

Install  1 Package
~~~~


From version 12 onward, you can use the option '--local-mode'.


    -z, --local-mode                                               Point chef-client at local repository

    -j JSON_ATTRIBS, --json-attributes JSON_ATTRIBS                Load attributes from a JSON file or URL

    -l, --log_level LEVEL                                          Set the log level (auto, debug, info, warn, error, fatal)

        
e.g.


`# chef-client -l info --local-mode -j ./custom-server.json`

The json file provided on the CLI contains the run list for this node.

custom-server.json:


`{ "run_list": ["recipe[custom-server]"] }`
