The link resource is used to create symlinks in a recipe.


Syntax


<code>link 'name' do
  target_file 'path to target. Defaults to 'name''
  to '/path/to/file'
  owner 'UID or name'
  group 'GID or name'
  link_type <:symbolic|:hard>
  action <:create|:delete|:nothing>
end</code>


e.g.


<code>link '/home/rhurley/.bashrc'
  to '/home/rhurley/.bash_profile'
  owner 'rhurley'
  group 'users'
end</code>

