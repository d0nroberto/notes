# XSCF Hardware Information 

If you've had a random reboot you may want to check for a CPU dying or memory board issues.
The below commands may prove useful to help check.

The web gui also provides an easy to understand graphical display of issues (if it works in your browser....)

### Hardware Related Commands

~~~~
XSCF> showboards -av
XSB  R DID(LSB) Assignment  Pwr  Conn Conf Test    Fault    COD
---- - -------- ----------- ---- ---- ---- ------- -------- ----
00-0   00(00)   Assigned    y    y    y    Passed  Normal   n
01-0   00(01)   Assigned    y    y    y    Passed  Normal   n
~~~~

~~~~
XSCF> showdcl -a
DID   LSB   XSB   Status
00                Running
      00    00-0
      01    01-0
~~~~

~~~~
XSCF> showfru -a sb 0
Device  Location    XSB Mode        Memory Mirror Mode
sb      00          Uni             no
sb      01          Uni             no
~~~~

~~~~
XSCF> showhardconf
SPARC Enterprise M5000;
    + Serial:XXXXXXXXX; Operator_Panel_Switch:Locked;
    + Power_Supply_System:Single; SCF-ID:XSCF#0;
    + System_Power:On; System_Phase:Cabinet Power On;
    Domain#0 Domain_Status:Running;
~~~~

~~~~
XSCF> showstatus
~~~~

~~~~
XSCF> showdevices
~~~~

### Fault Management configuration tool

To view fault management logs

~~~~
XSCF> fmdump -v

TIME                    UUID                                    MSG-ID

Nov 10 20:44:55.1283    9f773e33-e46f-466c-be86-fd3fcc449935   FMD-8000-0W

   100%  defect.sunos.fmd.nosub

   .....
~~~~

Display Very Verbose Event Detail for a UUID

~~~~
XSCF> fmdump -e -V -u 5f88d7d5-a107-4435-99c9-7c59479d22ed TIME CLASS
~~~~

### To view the logs:

~~~~
XSCF> showlogs -v
XSCF> showlogs error
XSCF> showlogs power
~~~~
