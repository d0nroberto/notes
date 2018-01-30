Moving files:

From where to where you ask?

On the same filesystem available to the recipe:

<code>remote_file "copy_file" do 
  path "/etc/rc.d/init.d/service_init_file" 
  source "file:////home/rhurley/service_init_file"
  owner 'root'
  group 'root'
  mode 0755
end</code>


or as a 1-liner (4 lines...)


<code>execute "copy_file" do
    command "cp /home/rhurley/service_init_file /etc/rc.d/init.d/"
    user "root"
end</code>


From the 'files' folder of the recipe:


<code>cookbook_file "/etc/rc.d/init.d/service_init_file" do
  source "service_init_file"
  owner "root"
  group "root"
  mode 0755
  action :create_if_missing
end</code>
