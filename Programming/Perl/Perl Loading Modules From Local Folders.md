#Loading Modules From Local Folders

Copied verbatim from:

<a href="http://search.cpan.org/~haarg/local-lib-2.000019/">http://search.cpan.org/~haarg/local-lib-2.000019/</a>



By default local::lib installs itself and the CPAN modules into ~/perl5.



Download and unpack the local::lib tarball from CPAN.

Do this as an ordinary user, not as root or administrator. Unpack the file in your home directory or in any other convenient location.

Run this: 

 perl Makefile.PL --bootstrap



In order to install local::lib into a directory other than the default, you need to specify the name of the directory when you call bootstrap, as follows:

 perl Makefile.PL --bootstrap=~/foo



Run this: (local::lib assumes you have make installed on your system)

 make test && make install





Setup the environment variables needed:

 echo 'eval "$(perl -I$HOME/perl5/lib/perl5 -Mlocal::lib)"' >>~/.bashrc



Another option supported:

 perl Makefile.PL --bootstrap --no-manpages



You can now install and load modules from your local folder.



Environment Variables:

 PERL_MB_OPT="--install_base \"/home/user/perl5\""; export PERL_MB_OPT;

 PERL_MM_OPT="INSTALL_BASE=/home/user/perl5"; export PERL_MM_OPT;



To install modules, you use:

 # perl -MCPAN -Mlocal::lib -e shell
