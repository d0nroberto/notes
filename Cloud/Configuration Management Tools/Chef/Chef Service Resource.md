The chef service resource is used to manage the state of a service on a node.


Syntax:


<code>service 'name' do
 supports { :restart => true, :start => true, :stop => true}
 action <:enable|:disable|:reload|:start|:stop|:restart>
end</code>


e.g.


<code>  cookbook_file "/etc/init.d/customservice.sh" do
    source "customservice.sh"
    action :create_if_missing
    owner "root"
    group "root"
    mode '0744'
  end
  # Setup service
  service "customservice" do
    action [:enable, :start]
  end
</code> 
