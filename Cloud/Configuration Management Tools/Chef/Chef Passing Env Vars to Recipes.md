A child process inherits it's environment from it's parent.

Also, a child process editing it's copy of the environment does not effect it's parent.

Consider the following shell examples:


<code>
% WOO="woo" #Set a shell variable
% echo $WOO
woo
% bash #Start a new process, a child of our first shell
% echo $WOO # Shell variable is empty because we didn't export it
% exit # Return to original shell
exit
% export WOO # Export variable
% bash
% echo $WOO #Shell variable now available in children.
woo
% exit
% WOO="woo" #Set a shell variable
% echo $WOO
woo
% bash #Start a new process, a child of our first shell
% export WOO="hello" # Change and export the shell variable
% exit # Return to original shell
exit
% echo $WOO #The parent's value remains unchanged.
woo
% exit
</code>



In ruby, the same is true.

The environment is exported in the 'ENV' variable.

<code>
ENV['FOO'] = 'bar'
bash 'env_test0' do
code &lt;&lt;-EOF
echo $FOO
EOF
end

bash 'env_test1' do
code &lt;&lt;-EOF
export BAZ='bar'
EOF
end

bash 'env_test2' do
code &lt;&lt;-EOF
echo $BAZ
EOF
end
</code>


Cobbled together from <a href="https://docs.chef.io/environment_variables.html">here.</a>
