# Steps to install a minimal desktop with CentOS.


### Install CentOS-7 - Minimal e.g. CentOS-7-x86_64-Minimal-1511.iso


### After install:
~~~~
 yum -y groupinstall "GNOME Desktop"
~~~~
or
~~~~
yum -y groupinstall "X Window System"

yum install gnome-classic-session gnome-terminal nautilus-open-terminal control-center liberation-mono-fonts
~~~~

~~~~
unlink /etc/systemd/system/default.target

ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target

reboot
~~~~

Now you have a CentOS-7 Minimal Desktop installation LIKE CentOS-6 Minimal Desktop


### For VNC Access to Remote Desktop

Edit ~/.vnc/xstartup


~~~~
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources

xsetroot -solid grey
vncconfig -iconic &
xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &

unset SESSION_MANAGER

gnome-session &
~~~~

Restart the vncserver:

~~~~
vncserver :1 -extension XFIXES -localhost
~~~~


The localhost option should be used along with ssh port forwarding to ensure a secure connection.

e.g. in PuTTY:

~~~~
Connection > SSH > Tunnels

Source Port e.g. 5901
Destination: <IP>:5901
~~~~
