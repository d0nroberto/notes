Attributes provide details about a node.


They can be defined in:

* Node (collected by ohai)

* Attribute files in Cookbooks

* Recipe files in Cookbooks

* Environment

* Roles


Attributes have precedence!


The types of attributes are:

* default - reset at the start of every chef-client run. Lowest precedence

* force_default - makes sure Cookbook attribute is used rather than role or environment attribute

* normal - attribute persists in the node

* override - specified in a recipe

* force_override - specified in a recipe or attribute file

* automatic - data from ohai determined at the start of a chef-client run. Highest precedence



Automatic Attributes

Node info determined by ohai e.g.

* node['platform'] - 'rhel'

* node['platform_version'] - '7.0', '6.3'

* node['ipaddress'] - '192.168.1.2'

* node['fqdn'] - 'localdomain'

* node['hostname'] - 'localhost'

* node['recipes'] - List of nodes recipes

* node['roles'] - List of nodes roles



Attribute Files

They are located in the attributes/ folder of the cookbook.

default.rb is read first, then other files in lexical order.

Attributes are evaluated when a cookbook is run on a node.

Sample attributes default.rb:


<code>default['recipename']['attrib1']    = '1'

default['recipename']['attrib2']    = '2'

default['recipename']['attrib3']    = '3'</code>


Generally you will set default attribute values in an attribute file and the override them for specific nodes using roles or environment.


Attribute File Methods

* override - set attribute type to override

* default - set attribute type to default

* normal - set attribute type to normal

* _unless

* attribute? - If an attribute exists, create additional attributes


e.g.


<code>if attribute?('USA')

 # Set Attributes for USA Servers

end</code>



In a recipe:

<code>

if node.attribute?('USA')

 # Set Attributes for USA Servers

end</code>



Attributes are evaluated in this order:


    A default attribute located in a cookbook attribute file

    A default attribute located in a recipe

    A default attribute located in an environment

    A default attribute located in role

    A force_default attribute located in a cookbook attribute file

    A force_default attribute located in a recipe

    A normal attribute located in a cookbook attribute file

    A normal attribute located in a recipe

    An override attribute located in a cookbook attribute file

    An override attribute located in a recipe

    An override attribute located in a role

    An override attribute located in an environment

    A force_override attribute located in a cookbook attribute file

    A force_override attribute located in a recipe

    An automatic attribute identified by Ohai at the start of the chef-client run


Examples with low to high precedence.


Default attribute in /attributes/default.rb

<code>* default['apache']['dir'] = '/etc/apache2'</code>

Default attribute in node object in recipe

<code>* node.default['apache']['dir'] = '/etc/apache2'</code>

Default attribute in /environments/environment_name.rb

<code>* default_attributes({ 'apache' => {'dir' => '/etc/apache2'}})</code>

Default attribute in /roles/role_name.rb

<code>* default_attributes({ 'apache' => {'dir' => '/etc/apache2'}})</code>

Normal attribute set as a cookbook attribute

<code>* set['apache']['dir'] = '/etc/apache2'

* normal['apache']['dir'] = '/etc/apache2'  #set is an alias of normal.</code>

Normal attribute set in a recipe

<code>* node.set['apache']['dir'] = '/etc/apache2'

* node.normal['apache']['dir'] = '/etc/apache2' # Same as above

* node['apache']['dir'] = '/etc/apache2'       # Same as above</code>

Override attribute in /attributes/default.rb

<code>* override['apache']['dir'] = '/etc/apache2'</code>

Override attribute in /roles/role_name.rb

<code>* override_attributes({ 'apache' => {'dir' => '/etc/apache2'}})</code>

Override attribute in /environments/environment_name.rb

<code>* override_attributes({ 'apache' => {'dir' => '/etc/apache2'}})</code>

Override attribute in a node object (from a recipe)

<code>* node.override['apache']['dir'] = '/etc/apache2'</code>

Ensure that a default attribute has precedence over other attributes

When a default attribute is set like this:

<code>* default['attribute'] = 'value'</code>

any value set by a role or an environment will replace it. 

To prevent this value from being replaced, use the force_default attribute precedence:

<code>* force_default['attribute'] = 'I will crush you, role or environment attribute'</code>

or:

<code>* default!['attribute'] = "The '!' means I win!"</code>

Ensure that an override attribute has precedence over other attributes

When an override attribute is set like this:

<code>* override['attribute'] = 'value'</code>

any value set by a role or an environment will replace it. 

To prevent this value from being replaced, use the force_override attribute precedence:

<code>* force_override['attribute'] = 'I will crush you, role or environment attribute'</code>

or:

<code>* override!['attribute'] = "The '!' means I win!"</code>



More info in the <a href="https://docs.chef.io/attributes.html">Chef docs</a>.
