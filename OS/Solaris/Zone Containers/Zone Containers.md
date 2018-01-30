# Solaris Zones

Information on solaris zones. How to create them, maintain their configuration, set resource limits etc.

Solaris zones are the Oracle Solaris implementation of container technology.

A server would have Solaris installed. This base install is known as the global zone.

Any zones created on the host *(zonecfg ... / zoneadm ...)* are known as the local zones.

These are the containerised instances of the OS that can be customised to run a specific application without an additional full OS stack running.
