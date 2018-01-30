Template files are a way to include static files in a recipe that you may want to have programatically customised on the target node they run on.


The can contain embedded ruby snippets which are evaluated when the recipe is run.


The files can be found in the cookbooks//templates/default/ folder.


For example:


<code>template "/etc/sample_script.sh" do
source "sample_script.erb"
action :create
owner "root"
group "root"
mode '0755'
end</code>


You can directly pass variables to the template file in the template code block.

Use the variables option e.g.


<code>template '/file/name.txt' do
  variables :partials => {
    'partial_name_1.txt.erb' => 'message',
    'partial_name_2.txt.erb' => 'message',
    'partial_name_3.txt.erb' => 'message'
  }
end</code>


In the template file:


<code><% @partials.each do |partial, message| %>
  Here is <%= partial %>
  <%= render partial, :variables => {:message => message} %>
<% end %>
</code>


Another example of code in a template file:


<code><%
case node.type
when "master"
%>
        <host key="Server.Host.Master" value="localhost.localdomain"/>
<%
when "slave"
%>
        <host key="Server.Host.Slave" value="localhost.localdomain"/>
<%</code>



See the <a href="https://docs.chef.io/resource_template.html">chef docs</a> for the full details of options supported by the template resource.
