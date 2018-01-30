# A list of all commands available for a Solaris Server XSCF console.

See PDF Part Number E25381-01 for more info (XSCF User's Guide) for more details.

Supported Hardware: SPARC Enterprise M3000/M4000/M5000/M8000/M9000


### Resetting the XSCF password



The default user account is called *'default'* (has platadm and useradm privs).  

Login to the XSCF using *'default'*  
Turn the front key on the server from Locked to Service (or vice versa)  
Hit the enter key at the terminal  
Turn the key back  
Hit the enter key at the terminal  
Now use the user admin commands to create / reset a user account.  

The *'admin'* user is a special system account so cannot use for login operator account.


### Full Command List


~~~~
addboard            setdate             showdomainmode
addcodlicense       setdcl              showdomainstatus
addfru              setdomainmode       showdscp
adduser             setdomparam         showdualpowerfeed
applynetwork        setdscp             showemailreport
cfgdevice           setdualpowerfeed    showenvironment
clockboard          setemailreport      showfru
confdidr            sethostname         showhardconf
console             sethttps            showhostname
deleteboard         setldap             showhttps
deletecodlicense    setlocale           showldap
deletefru           setlocator          showlocale
deleteuser          setloginlockout     showlocator
disablemodes        setlookup           showloginlockout
disableuser         setnameserver       showlogs
dumpconfig          setnetwork          showlookup
enableescalation    setntp              showmodes
enableservice       setpasswordpolicy   showmonitorlog
enableuser          setpowerupdelay     shownameserver
escalation          setprefetchmode     shownetwork
exit                setprivileges       shownotice
flashupdate         setrci              showntp
fmadm               setrcic             showpasswordpolicy
fmdump              setroute            showpowerupdelay
fmstat              setservicetag       showprefetchmode
getflashimage       setshutdowndelay    showresult
ioxadm              setsmtp             showroute
man                 setsnmp             showservicetag
moveboard           setsnmpusm          showshutdowndelay
nslookup            setsnmpvacm         showsmtp
password            setssh              showsnmp
ping                setsunmc            showsnmpusm
poweroff            settelnet           showsnmpvacm
poweron             settimezone         showssh
prtfru              setupfru            showstatus
rebootxscf          setupplatform       showsunmc
replacefru          showaltitude        showtelnet
reset               showapcs            showtimezone
resetdateoffset     showarchiving       showuser
restoreconfig       showaudit           snapshot
restoredefaults     showautologout      switchscf
sendbreak           showboards          testsb
service             showcod             traceroute
setaltitude         showcodlicense      unlockmaintenance
setapcs             showcodusage        version
setarchiving        showconsolepath     viewaudit
setaudit            showdate            who
setautologout       showdcl
setcod              showdevices
~~~~

Use manpages to get more details on each command e.g. *XSCF> man showboards*


### View / Connect to Domains Console

~~~~
XSCF> showdomainstatus -a
DID         Domain Status
00          Running
01          -
02          -
03          -
XSCF> console -d 0
~~~~

To disconnect from the console:
~~~~
#.
~~~~


### Reboot domains

~~~~
XSCF> poweroff -a

XSCF> poweroff -d 0
~~~~

~~~~
XSCF> poweron -a

XSCF> poweron -d 0
~~~~


The 3 modes to reset a domain are :

~~~~
por: To reset the domain
panic: To panic the domain
xir: To reset the CPU in domain
~~~~

~~~~
XSCF> reset -d 0 por

XSCF> reset -d 0 panic

XSCF> reset -d 0 xir
~~~~

Send a break signal to a domain / dumped to OK prompt

~~~~
XSCF> sendbreak -d 0
~~~~

~~~~
XSCF> poweroff -a
DomainIDs to power off:00
Continue? [y|n] :y
00 :Powering off
~~~~


*Note*

This command only issues the instruction to power-off.

The result of the instruction can be checked by the *"showlogs power"*.


### User Administration

~~~~
XSCF> adduser sysadmin

XSCF> deleteuser sysadmin

XSCF> disableuser sysadmin

XSCF> enableuser sysadmin

XSCF> showuser -l

XSCF> showuser -a

XSCF> setprivileges sysadmin platadm auditadm useradm fieldeng mode

XSCF> password sysadmin

XSCF> setautologout -s 60
~~~~


### Network related commands

~~~~
XSCF> shownetwork -a

XSCF> setnetwork xscf#0-lan#0 -m 255.255.255.0 192.168.1.10

XSCF> setroute -c add -n 0.0.0.0 -g 192.168.1.1 xscf#0-lan#0

XSCF> setdscp -q -y -i 192.168.1.0 -m 255.255.255.0

XSCF> setnameserver -c add 10.1.1.10 10.1.1.11

XSCF> sethostname xscf#0 hostname

XSCF> sethostname -d domainname.com

XSCF> showhostname -a

XSCF> showtimezone -c tz

XSCF> settimezone -c settz -s UTC

XSCF> setntp 192.168.1.10 192.168.1.20

XSCF> sethttps ....

XSCF> applynetwork -y

XSCF> rebootxscf

~~~~

### Hardware Diagnostics

~~~~
XSCF> showhardconf

XSCF> showstatus

XSCF> showdevices

XSCF> fmdump -v

XSCF> showboards -av

XSCF> showdcl -a

XSCF> showfru -a sb 0

XSCF> showlogs error

XSCF> showlogs event

XSCF> showlogs power

XSCF> showlogs console -d 00 (this shows the OS console without logging in)
~~~~


### Email Administration:

~~~~
XSCF? showsmtp

XSCF> showemailreport
~~~~
