Execute resources are used to execute a command on a host.


Syntax:


<code>execute 'name' do
  command 'command to run. Defaults to the same value as 'name''
  cwd 'current working directory of command'
  environment { "VARIABLE" => "VALUE"}
  user 'UID or username'
  group 'GID or groupname'
  action <:nothing|:run>
  retries 0
  timeout 10
  notifies :action, 'resource[name], <:before|:delayed|:immediate>
  not_if ''
  only_if ''
end</code>


Guards (not_if|only_if):

A guard property accepts either a string value or a Ruby block value:

A string is executed as a shell command.
If the command returns 0, the guard is applied.
If the command returns any other value, then the guard property is not applied.

A block is executed as Ruby code that must return either true or false.
If the block returns true, the guard property is applied.
If the block returns false, the guard property is not applied.

e.g.

<code>execute "install-custom-service" do
  command "chkconfig --add /etc/init.d/customservice"
  not_if { ::File.exists?("/etc/rc.d/rc3.d/S80customservice") }
end</code>
